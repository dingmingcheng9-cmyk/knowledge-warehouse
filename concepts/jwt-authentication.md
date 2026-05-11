---
title: JWT 认证与密码哈希
created: 2026-05-08
updated: 2026-05-08
type: concept
tags: [framework, tutorial, security, tool]
sources: [raw/doubao-source-jwt-conversation.md, raw/user-backend-jwt-conversation.md]
confidence: high
---

# JWT 认证与密码哈希

> 后端 API 认证的核心机制：密码哈希（bcrypt）保护密码安全，JWT（JSON Web Token）实现无状态的登录态管理。

## 为什么需要认证

任何人都可以调用 HTTP API。如果不对身份做校验，攻击者可以：
- 随意创建/删除用户
- 访问其他人的数据
- 修改系统配置

**认证的目标**：确认请求者身份，只允许合法的操作。

## 密码哈希（bcrypt）

### 为什么不能存明文密码

| | 加密 | 哈希 |
|---|---|---|
| 方向性 | 可逆（能解密） | 不可逆（不能还原） |
| 长度 | 与原文有关 | 固定长度 |
| 密码场景 | ❌ 不用 | ✅ 就用这个 |
| 例子 | AES、RSA | bcrypt、SHA-256 |

**核心逻辑**：服务器不需要知道你的原始密码。注册时把密码哈希存起来，登录时验证输入的密码能否匹配哈希值。即使数据库被拖走，黑客拿到的只是哈希。

### 盐（Salt）

**每个用户的密码都拼上一段随机字符串（盐），然后才哈希。**

```
用户 A 的密码: "abc123" + 随机盐 "A7x9" → 哈希
用户 B 的密码: "abc123" + 随机盐 "K3m2" → 完全不同的哈希
```

好处：
- 即使两个用户密码相同，哈希值也完全不同
- 彩虹表（预计算的密码→哈希对照表）彻底失效
- 一个用户的密码被破解，不影响其他用户

**盐是公开的，不需要保密**。它存在哈希值本身里。盐的价值在于让同样的密码产生不同的哈希，而不是保密。

### bcrypt 的特点

bcrypt 内嵌随机盐，不需要手动管理。哈希串格式：
```
$2b$12$LJ3m...9Xy
├─ $2b$ → bcrypt 版本
├─ 12   → 工作因子（2^12 轮迭代）
└─ 盐 + 哈希（自动生成和管理）
```

**工作因子决定了计算速度**：
- cost=12：每次哈希约 100ms
- cost=4：每次哈希约 1ms

故意设慢的原因：用户只算 1 次（登录），黑客要算 N 次（暴力破解用户密码）。100ms 对用户无感，但对黑客意味着数十亿次的等待。

### 核心安全公式

```
安全性 = 暴力破解成本 > 黑客愿意付出的代价
```

没有绝对安全，只有性价比安全。破解一个 bcrypt 密码需要数年+数十万硬件成本，远不如去找 SQL 注入漏洞划算。

### ⚠️ bcrypt 兼容性

**bcrypt 不能用 5.x 版本，必须用 4.x 才兼容 passlib。** 报错信息：
```
TypeError: Unsupported bcrypt version 5
```
解决方案：
```bash
pip install "bcrypt<5.0"    # 安装 bcrypt 4.3.0
```

## JWT（JSON Web Token）

### Token 是什么

Token 是后端发给前端的**通行证**——登录成功后服务器给你一个 Token，你每次请求带上它，服务器就知道你是你。

### 为什么需要签名

Token 内容（payload）只是 base64 编码，不是加密！任何人都可以解码查看。如果没有签名，用户可以：
1. 解码 Token
2. 把 `"sub": "1"` 改成 `"sub": "2"`
3. 重新编码后伪造身份

**签名解决了"你怎么证明这个 Token 是你服务器发的"这个问题。**

```
签名 = HMAC-SHA256(payload, SECRET_KEY)
```

**签名本质上就是哈希运算**（带密钥的哈希算法），与密码哈希同一原理。

### 完整 Token 结构

```
eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxIiwiZXhwIjoxNjgwMDB9.zV8sfjV5w...
├────────header────────┼───────payload─────────┼───────signature───────┤
       base64                base64               HMAC-SHA256 结果
```

### SECRET_KEY

- 存在服务器环境变量中，客户端完全不知道
- 签名和验签都在服务器内部完成
- 生产环境应使用密钥管理服务（KMS/Vault）+ 定期轮换
- **不要写死在代码里**，避免提交到 GitHub 导致泄露

## SECRET_KEY vs 环境变量安全性

| | 写死在代码里 | 存在环境变量 |
|---|---|---|
| 泄露途径 | 代码上传 GitHub | 仅限服务器被入侵 |
| 修改 | 改代码、重部署 | 改环境变量、重启即可 |
| 12-Factor App | ❌ 违反 | ✅ 符合 |

真实安全是分层防御：环境变量 → KMS → 定期轮换 → 多因素认证。

## FastAPI JWT 认证实现

### 核心代码结构

```python
from fastapi import FastAPI, HTTPException, Depends
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
from passlib.context import CryptContext
from jose import jwt
from datetime import datetime, timedelta, timezone

# --- JWT 配置 ---
SECRET_KEY = "my-super-secret-key-change-in-production"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

# --- 密码哈希工具 ---
pwd_context = CryptContext(schemes=["bcrypt"])

def hash_password(password: str) -> str:
    return pwd_context.hash(password)

def verify_password(plain_password: str, hashed_password: str) -> bool:
    return pwd_context.verify(plain_password, hashed_password)

# --- Token 生成 ---
def create_access_token(user_id: int) -> str:
    expire = datetime.now(timezone.utc) + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    payload = {"sub": str(user_id), "exp": expire}
    return jwt.encode(payload, SECRET_KEY, algorithm=ALGORITHM)

# --- 鉴权依赖 ---
app = FastAPI()
security = HTTPBearer()

def get_current_user(credentials: HTTPAuthorizationCredentials = Depends(security)):
    token = credentials.credentials
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        user_id = int(payload.get("sub"))
    except Exception:
        raise HTTPException(status_code=401, detail="无效的 token 或 token 已过期")
    # 查数据库确认用户存在
    ...
```

### 登录/注册流程

**注册：** `POST /register` → 明文密码 → `hash_password()` → 存哈希到数据库 → 返回用户信息

**登录：** `POST /login` → 查用户 → `verify_password()` 验证 → 通过后 `create_access_token()` → 返回 Token

**鉴权请求：** `GET /users/me` + `Authorization: Bearer <token>` → `get_current_user()` 验签 → 返回用户数据

### 每次请求都验证

**是的，每一个受保护的 HTTP 请求都会做一次完整的 JWT 验签。** 但 JWT 验签是纯内存计算（0.1ms-1ms），比查数据库快得多，每秒可处理 1000-10000 个请求，完全无感。

## 完整流程总结

```
密码哈希（注册/登录） ← bcrypt + 盐
    ↓
JWT 生成（服务器内部） ← HMAC-SHA256(payload, SECRET_KEY)
    ↓
Token 下发到前端（存在 localStorage）
    ↓
每次请求携带 Token
    ↓
服务器验签（服务器内部）
    ↓
通过 → 返回数据 | 失败 → 401
```

**两种哈希的本质统一：**
- 密码哈希：`明文密码 + 盐 → 哈希` → 存数据库
- Token 签名：`payload + 密钥 → 哈希` → 签名部分

都是哈希运算，只是应用场景不同。

## 相关页面

- [[fastapi-basics]] — FastAPI 后端框架基础
- [[sqlalchemy-orm-basics]] — SQLAlchemy ORM 数据库操作（配合 JWT 使用）
- [[shell-source-command]] — Shell source 命令（激活虚拟环境运行项目）
