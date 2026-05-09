---
source_url: https://www.doubao.com/thread/w4a20f3697fb0bb2c
ingested: 2026-05-09
sha256: d2c0e8f1a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9
---

# 后端学习 — 第5课：PUT 更新接口
日期：2026-05-09

## 学习内容

在第4课（JWT认证）基础上，添加了 PUT 更新接口，完成全 CRUD。

## 新增代码

在 main.py 末尾添加：

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

## 完整 API 列表

| 方法 | 路径 | 说明 | 需要登录 |
|------|------|------|---------|
| GET | / | 问候 | ❌ |
| POST | /register | 注册 | ❌ |
| POST | /login | 登录获取JWT | ❌ |
| GET | /users/me | 当前用户信息 | ✅ Bearer |
| GET | /users | 所有用户列表 | ❌ |
| GET | /users/{id} | 单个用户 | ❌ |
| PUT | /users/{id} | 更新用户 | ❌ |
| DELETE | /users/{id} | 删除用户 | ❌ |

## 测试结果

```
注册: POST /register?name=test_put&age=20&password=123  ✅
PUT 更新: PUT /users/2?name=test_put_updated&age=99    ✅ → {"id":2,"name":"test_put_updated","age":99,"message":"更新成功"}
验证 GET:  /users/2                                     ✅ → {"id":2,"name":"test_put_updated","age":99}
```

## 项目状态

- 项目路径：/root/learn-backend/
- 核心文件：main.py
- 技术栈：Python 3.10 + FastAPI + SQLAlchemy + SQLite + JWT
- 当前端口：8002

## 可选下一步

- B) 权限控制 — 让用户只能改/删自己的账户
- C) MySQL 迁移
- D) 其他功能
