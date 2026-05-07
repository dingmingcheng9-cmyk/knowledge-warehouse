---
title: SQLAlchemy ORM 数据库操作
created: 2026-05-07
updated: 2026-05-07
type: concept
tags: [tool, framework, tutorial, devops]
sources: [raw/articles/fastapi-backend-doubao-conversation.md, raw/articles/proc-orm-doubao-conversation.md]
confidence: high
---

# SQLAlchemy ORM 数据库操作

> ORM（对象关系映射）把数据库表变成 Python 类，让开发者用 Python 语法操作数据库，无需写 SQL。

## ORM 是什么

**ORM = Object Relational Mapping（对象关系映射）**

它的核心作用：把数据库的「表、行、列」变成 Python 的「类、对象、属性」。

| 数据库 | Python ORM |
|--------|-----------|
| 表（Table） | 类（Class） |
| 行（Row） | 对象实例（Instance） |
| 列（Column） | 属性（Attribute） |
| SQL 查询 | 方法调用（.query().filter()） |

**通俗理解：** ORM 就是一个翻译官——你说 Python 话（创建对象、改属性），ORM 自动翻译成 SQL 话（INSERT、UPDATE、SELECT）。你不用懂数据库语法，照样能增删改查。

## SQLite 简介

SQLite = 超轻量、不用安装、不用配置、开箱即用的小型数据库，Python 自带，无需额外安装。

特点：
- **就是一个文件**（`.db` 结尾），复制就能带走
- 无需服务器进程，适合学习、测试、移动端
- 与 MySQL 对比：SQLite = 便当盒（打开就能吃），MySQL = 大饭店（需要配置服务）

## 数据库连接

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import declarative_base

DATABASE_URL = "sqlite:///./app.db"  # 存在当前目录下的 app.db 文件

engine = create_engine(
    DATABASE_URL,
    connect_args={"check_same_thread": False}  # SQLite 专用，MySQL 不需要
)

Base = declarative_base()
```

## 定义数据模型（表结构）

```python
from sqlalchemy import Column, Integer, String

class User(Base):
    __tablename__ = "users"  # 数据库表名

    id = Column(Integer, primary_key=True)  # 主键 ID，自动递增
    name = Column(String(50))               # 姓名，最长 50 字符
    age = Column(Integer)                   # 年龄
```

### 逐行解读

- `class User(Base):` — **类的名字就是 Python 中用的名字**，数据库里表名由 `__tablename__` 决定
- `id = Column(Integer, primary_key=True)` — 主键列，每个用户的唯一标识，数据库自动生成
- `name = Column(String(50))` — 字符串列，`50` 是最长字符数限制
- `age = Column(Integer)` — 整数列

## 增删改查（CRUD）

### 查所有用户

```python
@app.get("/users")
def list_users():
    db = Session(engine)           # 打开数据库连接
    users = db.query(User).all()   # 查询所有用户
    db.close()                     # 关闭连接
    return users                   # FastAPI 自动转 JSON
```

### 查单个用户

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):
    db = Session(engine)
    user = db.query(User).filter(User.id == user_id).first()
    db.close()
    if not user:                            # 找不到用户
        raise HTTPException(status_code=404, detail="用户不存在")
    return {
        "id": user.id,
        "name": user.name,
        "age": user.age
    }
```

> `db = Session(engine)` = 打开数据库的门，准备查数据
> `db.query(User).filter(User.id == user_id).first()` = 在 users 表中找 id 等于指定值的行，取第一条

## 关键要点

- **函数需要自己写**：FastAPI 不自带任何业务功能，只提供路由映射
- **SQLAlchemy 是 ORM 实现**：把 Python 类的操作翻译成 SQL 语句
- **Session 生命周期**：用完必须 `db.close()`，否则连接泄漏
- `check_same_thread=False` 只用于 SQLite，MySQL 不需要

## 相关页面

- [[fastapi-basics]] — FastAPI 后端框架基础
- [[python-environment-management]] — Python 环境与依赖管理
