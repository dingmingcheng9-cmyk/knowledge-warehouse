---
source_url: https://www.doubao.com/thread/w73148d3000cccd8e
ingested: 2026-05-11
sha256: c5a9037aed908d5c5c4536cfe1fd3f0f77a72deab3893f869accc5794b88accd
---

# L8 环境变量与配置分离 — 完整对话备份 (2026-05-11)

**日期**: 2026-05-11 22:39 ~ 00:15
**学生**: 零基础，中文沟通，偏好概念先行的教学方式
**项目**: /root/learn-backend/
**技术栈**: Python 3.10 + FastAPI + SQLAlchemy + SQLite + JWT
**当前状态**: L1-L7 已完成（HTTP基础→FastAPI→CRUD→JWT→PUT更新）

---

## 对话流程（完整记录）

### 1. 开场：回顾进度

学生说"继续上一次对话关于后端的学习"。

我查了记忆和最近会话，确认完成进度：
- L1: HTTP 基础概念（餐厅类比）
- L2: 第一个 FastAPI 应用（3个端点）
- L3: SQLAlchemy + SQLite 完整 CRUD
- L4: JWT 认证（注册/登录/验证）
- L5: PUT 更新端点（全套 CRUD 完成）

当前 main.py 是一个完整的用户管理 API（235行）：注册、登录、查看、更新、删除，全部带 JWT 保护。

### 2. 学生选择 L8

我给了 4 个后续方向：
- L6: Pydantic 请求/响应模型
- L7: 数据库关系（一对多）
- **L8: 环境变量 & 配置分离** ← 学生选的
- L9: Alembic 数据库迁移

学生选择 L8。

### 3. L8 教学开始

#### 问题陈述

当前 main.py 里硬编码了 4 个敏感配置：
```python
DATABASE_URL = "sqlite:///./app.db"
SECRET_KEY = "my-super-secret-key-change-in-production"  # ❌ 代码里写死密钥
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30
```

如果代码提交到 GitHub，任何人都能看到 SECRET_KEY，可以伪造 JWT 令牌。

#### 安装 pydantic-settings

先检查依赖：
- pydantic 2.13.4 已存在（FastAPI 自带）
- pydantic-settings 未安装

安装：
```bash
pip install pydantic-settings
```
（自动安装 python-dotenv 1.2.2 作为依赖）

⚠️ 老问题：pip install 被终端工具标记为服务进程，用 background=true + wait 绕过。

#### Step 1: 创建 .env

```ini
DATABASE_URL=sqlite:///./app.db
SECRET_KEY=my-super-secret-key-change-in-production
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

#### Step 2: 创建 config.py

```python
from pydantic_settings import BaseSettings
from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent

class Settings(BaseSettings):
    DATABASE_URL: str = "sqlite:///./app.db"
    SECRET_KEY: str = "change-me-in-production"
    ALGORITHM: str = "HS256"
    ACCESS_TOKEN_EXPIRE_MINUTES: int = 30

    model_config = {"env_file": BASE_DIR / ".env"}

settings = Settings()
```

逐行解释：
- `class Settings(BaseSettings)` — 继承 pydantic-settings，自动加载 env 和 .env
- `DATABASE_URL: str = "..."` — 类型注解 + 默认值
- `ACCESS_TOKEN_EXPIRE_MINUTES: int` — int 类型注解，如果 .env 里写了 abc，启动就报 ValidationError（快速失败）
- `model_config` — pydantic v2 配置方式，指定 .env 文件路径
- `BASE_DIR / ".env"` — pathlib 路径拼接，跨平台
- `settings = Settings()` — 全局单例

#### Step 3: 修改 main.py

用 patch 工具做了 3 次替换：

1. 添加 import:
```python
from config import settings
```

2. 替换 DATABASE_URL:
```python
DATABASE_URL = settings.DATABASE_URL
```

3. 替换 JWT 配置（带注释的版本）:
```python
# ✅ SECRET_KEY 已迁移到 .env 文件，不再硬编码
SECRET_KEY = settings.SECRET_KEY
ALGORITHM = settings.ALGORITHM
ACCESS_TOKEN_EXPIRE_MINUTES = settings.ACCESS_TOKEN_EXPIRE_MINUTES
```

注释从 `# 【重要】...不能硬编码！` 改为 `# ✅ ...不再硬编码`。

#### Step 4: 创建 .gitignore

```gitignore
# 环境变量（存密钥、数据库地址等敏感信息）
.env

# Python 虚拟环境
venv/

# SQLite 数据库文件
*.db

# Python 缓存
__pycache__/
*.pyc
```

### 4. 测试验证

#### 测试 config.py 加载
```bash
$ python -c "from config import settings; print(settings.DATABASE_URL)"
DATABASE_URL: sqlite:///./app.db
ALGORITHM: HS256
TOKEN_EXPIRE: 30
```

#### 启动服务器
```bash
uvicorn main:app --host 0.0.0.0 --port 9090
```

#### 完整 API 测试

**GET /** → 根路径
```bash
$ curl -s http://localhost:9090/
{"message":"你好，后端世界！"}
```

**POST /register** → 注册新用户
```bash
$ curl -s -X POST 'http://localhost:9090/register?name=lisi&age=28&password=pass123'
{"id":3,"name":"lisi","age":28,"message":"注册成功"}
```

**POST /login** → 登录获取 JWT
```bash
$ curl -s -X POST 'http://localhost:9090/login?name=lisi&password=pass123'
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIzIiwiZXhwIjoxNzc4NTEyNTc3fQ.zTsgwwQ_irdwdey_As0iXf0zEaPIrzfo0mRhe1xOoxI","token_type":"bearer","user_id":3,"name":"lisi"}
```

**GET /users/me** → 用 token 访问受保护端点
```bash
$ curl -s -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIzIiwiZXhwIjoxNzc4NTEyNTc3fQ.zTsgwwQ_irdwdey_As0iXf0zEaPIrzfo0mRhe1xOoxI" http://localhost:9090/users/me
{"id":3,"name":"lisi","age":28}
```

全部通过 ✅

### 5. 关键概念总结表

| 概念 | 说明 |
|------|------|
| **BaseSettings** | pydantic-settings 基类，自动从环境变量 + .env 加载 |
| **类型注解** | `SECRET_KEY: str` — .env 里类型错误时启动即报错 |
| **默认值** | `SECRET_KEY: str = "change-me"` — 没配 .env 时的兜底 |
| **model_config** | pydantic v2 配置，`{"env_file": ...}` 指定 .env 路径 |
| **全局单例** | `settings = Settings()` — 只创建一次，到处引用 |
| **环境变量优先级** | 真实环境变量 > .env 文件 > 代码默认值 |

### 6. 课后练习

1. 试试环境变量覆盖：`ACCESS_TOKEN_EXPIRE_MINUTES=5 uvicorn main:app --host 0.0.0.0 --port 9090`
2. 改 .env 的 DATABASE_URL 为 `./test.db`，重启看数据库是不是换了

### 7. 后续方向

- L6: Pydantic 数据模型（请求/响应规范化）
- L7: 数据库一对多关系（Article + ForeignKey）
- L9: Alembic 数据库迁移

### 8. 保存备份

学生在课后要求将本次对话完整内容备份到 teaching-backend 技能参考文件。

已操作：
- 创建本备份文件: `references/l8-env-config-full-session.md`
- 更新 memory: 将 L5 记忆条目更新为 L5-L8
- 更新了 teaching-backend SKILL.md（Phase 9 已在之前的备份中存在）

---

## 技术细节

### 最终文件结构
```
/root/learn-backend/
├── main.py          # 235行，导入了 config.settings
├── config.py        # 22行，Settings 类
├── .env             # 4行，配置值
├── .gitignore       # 12行，保护敏感文件
├── venv/            # 虚拟环境（已存在）
└── app.db           # SQLite 数据库（已存在）
```

### 新增依赖
- pydantic-settings 2.14.1
- python-dotenv 1.2.2（自动安装）

### 教学用过的比喻

**房子钥匙比喻**：
- 以前: 房子钥匙(SECRET_KEY)贴在门上（代码里公开）
- 现在: 钥匙在口袋里 (.env)，门口贴着告示"不能看钥匙" (.gitignore)

**环境变量优先级比喻**：
- 总统令（真实环境变量）> 地方法规（.env）> 默认值
