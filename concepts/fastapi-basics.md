---
title: FastAPI 后端基础
created: 2026-05-07
updated: 2026-05-07
type: concept
tags: [tool, framework, tutorial, devops]
sources: [raw/articles/fastapi-backend-doubao-conversation.md]
confidence: high
---

# FastAPI 后端基础

> 快速搭建 Python 后端 API 的现代框架。核心搭档：FastAPI（写接口）+ Uvicorn（跑服务）。

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

## 路径参数

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):   # :int 声明类型，自动校验
    return {"user_id": user_id}
```

- `{user_id}` 是路径参数，从 URL 中提取
- 类型注解 `:int` 让 FastAPI 自动做类型转换和校验
- 访问 `/users/42` → `user_id=42`

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

## 相关页面

- [[python-environment-management]] — Python 虚拟环境管理
- [[sqlalchemy-orm-basics]] — SQLAlchemy ORM 数据库操作
