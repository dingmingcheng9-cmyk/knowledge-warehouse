---
title: FastAPI 后端基础
created: 2026-05-07
updated: 2026-05-11
type: concept
tags: [tool, framework, tutorial, devops]
sources: [raw/articles/fastapi-backend-doubao-conversation.md, raw/articles/user-backend-study-notes.md, raw/doubao-source-jwt-conversation.md, raw/user-backend-jwt-conversation.md, raw/articles/user-backend-put-update.md, raw/articles/l8-env-config-teaching-backup.md, raw/articles/jwt-hash-env-doubao-conversation.md]
confidence: high
---

# FastAPI 后端基础

> 快速搭建 Python 后端 API 的现代框架。核心搭档：FastAPI（写接口）+ Uvicorn（跑服务）。

## HTTP 基础：服务器和浏览器的对话

### 餐厅模型（思维模型）

```
浏览器（客户端）                    服务器
    │                                │
    ├── 请求 (Request) ────────────→  │
    │                                ├── 解析请求
    │                                ├── 处理逻辑
    │                                └── 返回响应
    │                                │
    │←── 响应 (Response) ──────────── │
```

比喻：
- **客户端** = 顾客
- **服务器** = 厨房 + 厨师
- **HTTP 请求** = 菜单（我要什么？）
- **HTTP 响应** = 做好的菜
- **数据库** = 冰箱里的食材
- **后端** = 写代码处理请求、返回响应

### HTTP 请求三要素

| 部分 | 含义 | 例子 |
|------|------|------|
| 方法 (Method) | 想做什么操作 | GET(查)、POST(增)、PUT(改)、DELETE(删) |
| 路径 (Path) | 操作哪个资源 | `/users/123` |
| 头信息 (Headers) | 附加条件 | 身份认证、接受格式 |

### 常见状态码

| 状态码 | 含义 |
|--------|------|
| `200` | OK 成功 |
| `404` | 没找到 |
| `500` | 服务器内部错误 |

### 核心四句话

1. 后端就是写代码处理请求、返回响应
2. 四种操作 = CRUD：增(Create) 查(Read) 改(Update) 删(Delete)
3. 方法对应操作：POST=增, GET=查, PUT=改, DELETE=删
4. 后端不关心页面长什么样，只关心数据怎么存取

## 核心概念

### FastAPI 是什么

**FastAPI = 用来快速写后端 API（接口）的 Python 框架。** 用它写代码可以做出：
- 登录 / 注册接口
- 获取用户信息接口
- 上传文件接口
- 小程序 / APP 的后端服务
- 网站的数据接口

### Uvicorn 是什么

**Uvicorn = 用来运行 FastAPI 代码的服务器（ASGI 服务器）。** 它的作用：
- 把写好的接口变成**可以访问的网址**
- 接收前端/小程序发来的请求
- 把结果返回回去

两者必须一起用，缺一不可。FastAPI 是"厨师"（做接口），Uvicorn 是"服务员"（让接口能被访问）。

## 环境搭建

```bash
cd ~/myproject
python3 -m venv venv          # 创建虚拟环境，隔离依赖
source venv/bin/activate      # 激活虚拟环境
pip install fastapi uvicorn   # 安装核心依赖
```

### 常见错误

- `source venv/bin/activate` 在脚本中执行后，pip 安装仍在原环境 → 在虚拟环境激活后的命令行中才正确
- 直接使用 `venv/bin/pip install` 无需激活也能安装到虚拟环境

## 第一个 API

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def hello():
    return {"message": "你好，后端世界！"}
```

启动服务：
```bash
venv/bin/uvicorn main:app --host 0.0.0.0 --port 8000
```

参数说明：
- `main:app` — `main.py` 文件中的 `app` 对象
- `--host 0.0.0.0` — 允许外部访问（默认只允许 localhost）
- `--port 8000` — 监听的端口号

## 三个核心要素

每段 FastAPI 路由代码由三部分组成：

```python
@app.get("/users")           # ← @app.方法名("路径")
def list_users():             # ← 函数定义（处理逻辑）
    return [...]              # ← 返回值（响应数据）
```

1. **HTTP 方法** — `@app.get()` / `@app.post()` / `@app.put()` / `@app.delete()`，对应增删改查
2. **URL 路径** — `"/"`, `"/users"`, `"/users/{user_id}"`，决定访问地址
3. **函数** — 定义的业务逻辑，需要**自己编写**，FastAPI 不自带任何业务功能

⚠️ **重要区别：** `def hello():` 是**定义函数**（准备好等功能被调用），`hello()` 才是**调用函数**（立即执行）。FastAPI 在收到 HTTP 请求时**自动调用**你定义的路由函数。

### 路由（Routing）可视化

```
服务器 (http://localhost:8000)
│
├── GET  /            → hello()         → "你好，后端世界！"
├── GET  /users/42    → get_user(42)    → {"user_id": 42}
├── POST /users       → create_user()   → {"message": "创建成功"}
└── ...
```

每个请求到达服务器后，FastAPI 根据 **方法 + 路径** 找到对应的函数来执行。

## 路径参数

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):   # :int 声明类型，自动校验
    return {"user_id": user_id}
```

- `{user_id}` 是路径参数，从 URL 中提取
- 类型注解 `:int` 让 FastAPI 自动做类型转换和校验
- 访问 `/users/42` → `user_id=42`

## 更多 API 示例

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def hello():
    return {"message": "你好，后端世界！"}

@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id, "name": f"用户{user_id}"}

@app.post("/users")
def create_user(name: str, age: int):
    return {"name": name, "age": age, "message": "用户创建成功"}
```

这是后端的**三个基本接口**：查首页、查单个用户、创建用户。注意 `POST` 用于创建，`GET` 用于查询。

## curl 测试

```bash
# GET 请求
curl -s http://localhost:8000/

# GET 带路径参数
curl -s http://localhost:8000/users/42

# POST 请求（带查询参数）
curl -s -X POST "http://localhost:8000/users?name=小明&age=25"
```

`curl` 是终端里的浏览器，用于在不打开浏览器的情况下测试接口。

## PUT 更新接口（完整 CRUD）

在第4课 JWT 认证 + 第2课 SQLAlchemy ORM 的基础上，第5课实现了 PUT 更新接口，至此 CRUD（增删查改）全部完成。

### PUT 更新代码模式

```python
@app.put("/users/{user_id}")
def update_user(user_id: int, name: str, age: int):
    \"\"\"更新用户的 name 和 age\"\"\"
    db = Session(engine)
    user = db.query(User).filter(User.id == user_id).first()
    if not user:
        db.close()
        raise HTTPException(status_code=404, detail="用户不存在")

    # 修改字段值
    user.name = name
    user.age = age
    db.commit()
    db.refresh(user)
    db.close()

    return {
        "id": user.id,
        "name": user.name,
        "age": user.age,
        "message": "更新成功"
    }
```

**三步曲：查 → 改 → 提交：**
1. `db.query().filter().first()` — 先查出记录
2. 修改字段值（Python 对象的属性赋值）
3. `db.commit()` 提交事务 + `db.refresh()` 刷新到最新状态

⚠️ 如果 `user` 不存在，返回 `404` 而不是报 500 服务器错误。

### 完整 CRUD 一览

| 操作 | 方法 | 路径 | 代码模式 |
|------|------|------|---------|
| **Create** 创建 | POST | /register | 查重 → 哈希密码 → 创建 → commit |
| **Read** 查询 | GET | /users, /users/{id} | query + filter → .all() 或 .first() |
| **Update** 更新 | PUT | /users/{id} | 查 → 改字段 → commit + refresh |
| **Delete** 删除 | DELETE | /users/{id} | 查 → delete → commit |

### 测试 PUT 接口

```bash
# 先注册一个用户
curl -s -X POST "http://localhost:8002/register?name=test_put&age=20&password=123"

# PUT 更新
curl -s -X PUT "http://localhost:8002/users/2?name=test_put_updated&age=99"
# → {"id":2,"name":"test_put_updated","age":99,"message":"更新成功"}

# 验证 GET
curl -s "http://localhost:8002/users/2"
# → {"id":2,"name":"test_put_updated","age":99}
```

项目路径：`/root/learn-backend/`，当前端口 8002。

## 环境变量与配置分离（pydantic-settings）

### 问题：硬编码的安全隐患

项目中敏感配置（SECRET_KEY、DATABASE_URL）直接写在 main.py 里：

```python
SECRET_KEY = "my-super-secret-key-change-in-production"  # ❌ 密钥暴露在代码中
```

如果代码提交到 GitHub，任何人都能看到 SECRET_KEY，可以伪造 JWT 令牌。

### 解决方案：pydantic-settings + .env 文件

**安装：**
```bash
pip install pydantic-settings
# 自动安装 python-dotenv 作为依赖
```

**Step 1: 创建 .env 文件**
```ini
DATABASE_URL=sqlite:///./app.db
SECRET_KEY=my-super-secret-key-change-in-production
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

**.env 文件不上传到 GitHub（见下文的 .gitignore），只存在服务器本地。**

**Step 2: 创建 config.py**
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

关键点：
- `class Settings(BaseSettings)` — 继承 pydantic-settings，自动从环境变量 + .env 加载
- `ACCESS_TOKEN_EXPIRE_MINUTES: int` — 类型注解；如果 .env 写了 abc，启动就报 `ValidationError`（快速失败原则）
- `model_config` — pydantic v2 配置方式，指定 .env 文件路径
- `BASE_DIR / ".env"` — pathlib 路径拼接，跨平台兼容
- `settings = Settings()` — 全局单例，只创建一次到处引用

**Step 3: 在 main.py 中使用**
```python
from config import settings

DATABASE_URL = settings.DATABASE_URL
SECRET_KEY = settings.SECRET_KEY
ALGORITHM = settings.ALGORITHM
ACCESS_TOKEN_EXPIRE_MINUTES = settings.ACCESS_TOKEN_EXPIRE_MINUTES
```

**Step 4: 创建 .gitignore**
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

### main.py vs config.py 分工

| 文件 | 职责 | 类比 |
|------|------|------|
| `.env` | 存敏感配置值 | 密码本 |
| `config.py` | 桥梁：读取 .env 并转换为 Python 对象 | 翻译官 |
| `main.py` | 使用 config.settings 拿配置 | 指挥官 |

**用户总结标准答案：**
> main.py 是项目的总指挥代码，config.py 是连接代码与 .env 文件的桥梁。

### 房子钥匙比喻

- **以前**：钥匙（SECRET_KEY）贴在门上（硬编码在代码里）
- **现在**：钥匙在口袋里（.env 文件），门口贴着"不能看钥匙"（.gitignore）

### 环境变量优先级

```
真实环境变量（最高优先级）
       ↓
.env 文件（中等优先级）
       ↓
代码默认值（最低优先级）
```

可以临时覆盖配置（不改 .env）：
```bash
ACCESS_TOKEN_EXPIRE_MINUTES=5 uvicorn main:app --host 0.0.0.0 --port 9090
```

## 相关页面

- [[python-environment-management]] — Python 虚拟环境管理
- [[sqlalchemy-orm-basics]] — SQLAlchemy ORM 数据库操作
- [[jwt-authentication]] — JWT 认证与密码哈希（登录/注册/鉴权）
