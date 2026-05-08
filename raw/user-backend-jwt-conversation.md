---
source_url: https://cli-conversation
ingested: 2026-05-08
sha256: d0c9ad59330db115b5bb9ec95c92b5c1424356c483545e85abe2ad006d75fc68
---

对话完整备份 — 后端学习第4课（JWT 认证）
日期：2026-05-08
开始时间：~21:24
助手模型：deepseek-v4-flash
平台：CLI

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

用户：接着上次对话的后端学习

助手角色切换：继续后端教学
- 确认项目状态：/root/learn-backend/ 完好，main.py + app.db 都在
- 回顾已学三课：
  第1课：HTTP 思维模型
  第2课：第一个 FastAPI API
  第3课：SQLAlchemy + SQLite 完整 CRUD
- 提供两条路线：A (PUT更新) / B (JWT认证)
用户选择：路线 B

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

助手教学：
1. 解释为什么需要认证（任何人都可以调用 API 的问题）
2. 讲解 JWT 概念 = 电子身份证 + 数字签名
3. 讲解密码哈希（bcrypt，不可逆）
4. 安装依赖：
   pip install "python-jose[cryptography]" "passlib[bcrypt]"
5. 问题：bcrypt 5.0 和 passlib 不兼容
   → 降级：pip install "bcrypt<5.0"（最终用 bcrypt 4.3.0）
6. 重写 main.py，加入：
   - password_hash 字段到 User 模型
   - passlib.CryptContext 密码哈希工具
   - jwt.encode/decode JWT 工具
   - POST /register（注册，密码哈希存储）
   - POST /login（登录验证，返回 JWT token）
   - GET /users/me（需要 Bearer token 的受保护 API）
   - fastapi.security.HTTPBearer + Depends 鉴权机制
7. 删除旧 app.db（表结构变了，重建）
8. 启动服务器在端口 8002（8000/8001 被占用）
9. 测试全部通过：
   - 注册成功 → {"id":1, "name":"xiaoming", "age":25, "message":"注册成功"}
   - 登录成功 → 返回 JWT token eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
   - GET /users/me 带 token → 返回 id:1, name:xiaoming, age:25
   - 错误密码登录 → 401 "用户名或密码错误"
   - 无 token 访问 /users/me → 401 "Not authenticated"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

最终 main.py 内容：

from fastapi import FastAPI, HTTPException, Depends
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import declarative_base, Session
from passlib.context import CryptContext
from jose import jwt
from datetime import datetime, timedelta, timezone

DATABASE_URL = "sqlite:///./app.db"
engine = create_engine(DATABASE_URL, connect_args={"check_same_thread": False})
Base = declarative_base()

class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String(50), nullable=False)
    age = Column(Integer, nullable=False)
    password_hash = Column(String(128), nullable=False)

Base.metadata.create_all(bind=engine)

SECRET_KEY = "my-super-secret-key-change-in-production"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

pwd_context = CryptContext(schemes=["bcrypt"])

def hash_password(password: str) -> str:
    return pwd_context.hash(password)

def verify_password(plain_password: str, hashed_password: str) -> bool:
    return pwd_context.verify(plain_password, hashed_password)

def create_access_token(user_id: int) -> str:
    expire = datetime.now(timezone.utc) + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    payload = {"sub": str(user_id), "exp": expire}
    return jwt.encode(payload, SECRET_KEY, algorithm=ALGORITHM)

app = FastAPI()
security = HTTPBearer()

def get_current_user(credentials: HTTPAuthorizationCredentials = Depends(security)):
    token = credentials.credentials
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        user_id = int(payload.get("sub"))
    except Exception:
        raise HTTPException(status_code=401, detail="无效的 token 或 token 已过期")
    db = Session(engine)
    user = db.query(User).filter(User.id == user_id).first()
    db.close()
    if not user:
        raise HTTPException(status_code=401, detail="用户不存在")
    return user

@app.get("/")
def hello():
    return {"message": "你好，后端世界！"}

@app.post("/register")
def register(name: str, age: int, password: str):
    db = Session(engine)
    existing = db.query(User).filter(User.name == name).first()
    if existing:
        db.close()
        raise HTTPException(status_code=400, detail="用户名已被注册")
    new_user = User(name=name, age=age, password_hash=hash_password(password))
    db.add(new_user)
    db.commit()
    db.refresh(new_user)
    db.close()
    return {"id": new_user.id, "name": new_user.name, "age": new_user.age, "message": "注册成功"}

@app.post("/login")
def login(name: str, password: str):
    db = Session(engine)
    user = db.query(User).filter(User.name == name).first()
    db.close()
    if not user:
        raise HTTPException(status_code=401, detail="用户名或密码错误")
    if not verify_password(password, user.password_hash):
        raise HTTPException(status_code=401, detail="用户名或密码错误")
    token = create_access_token(user.id)
    return {"access_token": token, "token_type": "bearer", "user_id": user.id, "name": user.name}

@app.get("/users/me")
def get_my_info(current_user: User = Depends(get_current_user)):
    return {"id": current_user.id, "name": current_user.name, "age": current_user.age}

@app.get("/users")
def list_users():
    db = Session(engine)
    users = db.query(User).all()
    db.close()
    return users

@app.get("/users/{user_id}")
def get_user(user_id: int):
    db = Session(engine)
    user = db.query(User).filter(User.id == user_id).first()
    db.close()
    if not user:
        raise HTTPException(status_code=404, detail="用户不存在")
    return {"id": user.id, "name": user.name, "age": user.age}

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

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

用户状态：困了，要休息，要求保存会话

项目快照：
- 路径：/root/learn-backend/
- main.py：含 JWT 认证（6279 字节）
- app.db：已重建（原来是空数据库）
- venv：激活中，已安装 fastapi, uvicorn, sqlalchemy, passlib, python-jose, bcrypt 4.3
- 服务器：uvicorn 在 8002 端口运行中

后续可选路线：
A. PUT 更新端点（补 CRUD 的 Update）
B+. 给更多 API 加认证保护（只能修改/删除自己的账户）
C. MySQL 代替 SQLite

注意：bcrypt 不能用 5.x，必须用 4.x 才兼容 passlib
