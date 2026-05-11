# 后端开发学习笔记

> 日期：2026-05-07
> 技术栈：Python + FastAPI + SQLAlchemy + SQLite
> 项目目录：`/root/learn-backend/`

---

## 第一课：HTTP ── 服务器和浏览器的对话

### 核心思维模型

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

比喻：**餐厅模型**
- 客户端 = 顾客
- 服务器 = 厨房 + 厨师
- HTTP 请求 = 菜单（我要什么？）
- HTTP 响应 = 做好的菜
- 数据库 = 冰箱里的食材

### HTTP 请求的要素

| 部分 | 含义 | 例子 |
|------|------|------|
| 方法 (Method) | 想做什么操作 | GET(查)、POST(增)、PUT(改)、DELETE(删) |
| 路径 (Path) | 操作哪个资源 | `/users/123` |
| 头信息 (Headers) | 附加条件 | 身份认证、接受格式 |

### 状态码

- `200` = 成功
- `404` = 没找到
- `500` = 服务器内部错误

### 核心四句话

1. 后端就是写代码处理请求、返回响应
2. 四种操作 = CRUD：增(Create) 查(Read) 改(Update) 删(Delete)
3. 方法对应操作：POST=增, GET=查, PUT=改, DELETE=删
4. 后端不关心页面长什么样，只关心数据怎么存取

---

## 第二课：FastAPI ── 第一个 API

### 安装

```bash
python3 -m venv venv
source venv/bin/activate
pip install fastapi uvicorn
```

### 最简 API 代码

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

### 启动服务器

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

### 测试 API

```bash
# GET 请求
curl http://localhost:8000/

# GET 带路径参数
curl http://localhost:8000/users/42

# POST 请求（传参数）
curl -X POST 'http://localhost:8000/users?name=xiaoming&age=25'
```

### 核心概念：路由（Routing）

```
服务器 (http://localhost:8000)
│
├── GET  /            → hello()         → "你好，后端世界！"
├── GET  /users/42    → get_user(42)    → {"user_id": 42}
├── POST /users       → create_user()   → {"message": "创建成功"}
└── ...
```

---

## 第三课：数据库 ── 用 SQLAlchemy 存取数据

### 为什么需要数据库？

没有数据库：数据在内存里，服务器重启就丢失
有数据库：数据存到硬盘，永久保留

### 安装 SQLAlchemy

```bash
pip install sqlalchemy
```

### 带数据库的完整 API

```python
from fastapi import FastAPI, HTTPException
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import declarative_base, Session

# 连接 SQLite（换 MySQL 只需改这一行）
DATABASE_URL = "sqlite:///./app.db"
engine = create_engine(DATABASE_URL, connect_args={"check_same_thread": False})

# 定义数据模型
Base = declarative_base()

class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String(50), nullable=False)
    age = Column(Integer, nullable=False)

# 自动创建表
Base.metadata.create_all(bind=engine)

app = FastAPI()

# 查所有
@app.get("/users")
def list_users():
    db = Session(engine)
    users = db.query(User).all()
    db.close()
    return users

# 查单个
@app.get("/users/{user_id}")
def get_user(user_id: int):
    db = Session(engine)
    user = db.query(User).filter(User.id == user_id).first()
    db.close()
    if not user:
        raise HTTPException(status_code=404, detail="用户不存在")
    return {"id": user.id, "name": user.name, "age": user.age}

# 创建用户
@app.post("/users")
def create_user(name: str, age: int):
    db = Session(engine)
    new_user = User(name=name, age=age)
    db.add(new_user)
    db.commit()
    db.refresh(new_user)
    db.close()
    return {"id": new_user.id, "name": new_user.name, "age": new_user.age, "message": "用户创建成功"}

# 删除用户
@app.delete("/users/{user_id}")
def delete_user(user_id: int):
    db = Session(engine)
    user = db.query(User).filter(User.id == user_id).first()
    if not user:
        db.close()
        raise HTTPException(status_code=404, detail="用户不存在")
    db.delete(user)
    db.commit()
    db.close()
    return {"message": f"用户 {user_id} 已删除"}
```

### CRUD 操作对照表

| 操作 | HTTP 方法 | API 路径 | 数据库操作 |
|------|-----------|----------|-----------|
| Create | POST | `/users` | `db.add()` → `db.commit()` |
| Read (全部) | GET | `/users` | `db.query(User).all()` |
| Read (单个) | GET | `/users/{id}` | `.filter(User.id == id).first()` |
| Update | PUT | 待学 | `.filter(...).update(...)` |
| Delete | DELETE | `/users/{id}` | `.filter(...).delete()` → `db.commit()` |

### 核心概念：ORM

ORM（对象关系映射）让你用 Python 对象操作数据库：

```python
# 数据库表 users
# ┌────┬──────────┬─────┐
# │ id │  name    │ age │
# ├────┼──────────┼─────┤
# │ 1  │ xiaoming │ 25  │
# │ 2  │ xiaohong │ 23  │
# └────┴──────────┴─────┘

new_user = User(name="xiaoming", age=25)  # 创建新行对象
db.add(new_user)                          # 插入数据库
db.commit()                               # 提交保存
```

### 数据持久化验证

即使服务器关闭后重启，数据库文件 `app.db` 中的数据依然存在。

### 换用 MySQL

只需改一行：
```python
DATABASE_URL = "mysql+pymysql://用户名:密码@地址:3306/数据库名"
```

---

## 后续课程预告

- 路线 A：`PUT /users/{id}` ── 补全 Update 操作
- 路线 B：JWT 用户认证 ── 注册、登录、Token 鉴权
- 后续：项目实战、部署上线

---

> 项目文件位置：`/root/learn-backend/`
> 数据库文件：`/root/learn-backend/app.db`
> Python 虚拟环境：`/root/learn-backend/venv/`
