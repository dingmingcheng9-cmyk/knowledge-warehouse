---
title: SQLAlchemy ORM 数据库操作
created: 2026-05-07
updated: 2026-05-09
type: concept
tags: [tool, framework, tutorial, devops]
sources: [raw/articles/fastapi-backend-doubao-conversation.md, raw/articles/user-backend-study-notes.md, raw/articles/proc-orm-doubao-conversation.md, raw/articles/user-backend-put-update.md]
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

## 为什么需要数据库？

**没有数据库：** 数据在内存里，服务器重启就丢失
**有数据库：** 数据存到硬盘，永久保留

即使服务器关闭后重启，数据库文件（如 `app.db`）中的数据依然存在。这就是**数据持久化**。

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

    id = Column(Integer, primary_key=True, index=True)   # 主键 ID，自动递增
    name = Column(String(50), nullable=False)             # 姓名，最长 50 字符，不能为空
    age = Column(Integer, nullable=False)                 # 年龄，不能为空
```

### 逐行解读

- `class User(Base):` — **类的名字就是 Python 中用的名字**，数据库里表名由 `__tablename__` 决定
- `id = Column(Integer, primary_key=True, index=True)` — 主键列，每个用户的唯一标识，`index=True` 提高查询速度
- `name = Column(String(50), nullable=False)` — 字符串列，`50` 是最长字符数，`nullable=False` 表示不能为空
- `age = Column(Integer, nullable=False)` — 整数列，不能为空

### 自动创建表

```python
Base.metadata.create_all(bind=engine)
```

这一行放在定义完所有模型之后，SQLAlchemy 会自动在数据库中创建所有尚未存在的表。**重复运行不会重复创建。**

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

### 创建用户

```python
@app.post("/users")
def create_user(name: str, age: int):
    db = Session(engine)
    new_user = User(name=name, age=age)   # 创建新用户对象
    db.add(new_user)                       # 加入数据库会话
    db.commit()                            # 提交事务，真正写入
    db.refresh(new_user)                   # 刷新对象（获取自增 id）
    db.close()
    return {
        "id": new_user.id,
        "name": new_user.name,
        "age": new_user.age,
        "message": "用户创建成功"
    }
```

创建流程：`new_user = User(...)` → `db.add()` → `db.commit()` → `db.refresh()`

### 删除用户

```python
@app.delete("/users/{user_id}")
def delete_user(user_id: int):
    db = Session(engine)
    user = db.query(User).filter(User.id == user_id).first()
    if not user:
        db.close()
        raise HTTPException(status_code=404, detail="用户不存在")
    db.delete(user)    # 标记删除
    db.commit()        # 提交删除
    db.close()
    return {"message": f"用户 {user_id} 已删除"}
```

### 更新用户

```python
@app.put("/users/{user_id}")
def update_user(user_id: int, name: str, age: int):
    db = Session(engine)
    user = db.query(User).filter(User.id == user_id).first()
    if not user:
        db.close()
        raise HTTPException(status_code=404, detail="用户不存在")

    # 修改字段值
    user.name = name
    user.age = age
    db.commit()           # 提交事务，将改动写回数据库
    db.refresh(user)      # 从数据库重新加载最新状态
    db.close()

    return {
        "id": user.id,
        "name": user.name,
        "age": user.age,
        "message": "更新成功"
    }
```

**更新流程：** `db.query().filter().first()` → 修改属性 → `db.commit()` → `db.refresh()`

❗ **关键区别：** 更新不需要 `db.add()`，因为 `user` 已经是数据库中的记录对象。ORM 自动追踪属性变更，`db.commit()` 时会把所有改动写回数据库。

### CRUD 操作对照表

| 操作 | HTTP 方法 | API 路径 | 数据库操作 |
|------|-----------|----------|-----------|
| Create | POST | `/users` | `db.add()` → `db.commit()` |
| Read (全部) | GET | `/users` | `db.query(User).all()` |
| Read (单个) | GET | `/users/{id}` | `.filter(User.id == id).first()` |
| Update | PUT | `/users/{id}` | 查 → 改字段 → `db.commit()` + `db.refresh()` |
| Delete | DELETE | `/users/{id}` | `db.delete(user)` → `db.commit()` |

### 换用 MySQL

只需改一行连接代码：

```python
# SQLite 版
DATABASE_URL = "sqlite:///./app.db"

# MySQL 版（需要先 pip install pymysql）
DATABASE_URL = "mysql+pymysql://用户名:密码@地址:3306/数据库名"
```

注意：MySQL 不需要 `connect_args={"check_same_thread": False}` 参数。模型和 CRUD 代码完全不变。

## 关键要点

- **函数需要自己写**：FastAPI 不自带任何业务功能，只提供路由映射
- **SQLAlchemy 是 ORM 实现**：把 Python 类的操作翻译成 SQL 语句
- **Session 生命周期**：用完必须 `db.close()`，否则连接泄漏
- `check_same_thread=False` 只用于 SQLite，MySQL 不需要

## 相关页面

- [[fastapi-basics]] — FastAPI 后端框架基础
- [[python-environment-management]] — Python 环境与依赖管理
