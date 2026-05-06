---
source_url: https://www.doubao.com/thread/ad70e7e69eb4d
ingested: 2026-05-02
---

# 原始对话：Hermes 运行方式 & uv 环境管理（豆包）

标题: Hermes配置DeepSeek API地址未生效问题
分享者: 小蠢货
平台: 豆包 (doubao.com)

## 对话一：理解 uv / python / ./ / source venv 的区别

**用户提问**:
> 我可以这么理解吗？uv是直接到对应的python的网站去下载对应的python环境，python是启动系统的python环境文件，./是建立在环境已经启动的情况，source ... venv是建立在已经安装对应的环境

**AI 回答**:

### 1. uv 是什么？
- uv 是一个"超级工具"，本地自带全部功能
- 自动找到项目需要的 Python
- 自动加载已经下载好的依赖（yaml, torch 等）
- **不用联网下载**
- 直接准备好完整环境，然后运行程序

**一句话**: uv = 自动帮你把环境全部准备好，再运行程序

### 2. python3 / python
- 用系统自带的 Python 裸跑
- 不管项目环境、不管依赖装没装
- 所以缺包会报错

### 3. ./hermes
- 直接运行文件
- **必须环境已经激活**
- 必须依赖都装好，否则直接报错

### 4. source ... venv
- venv = 传统 Python 环境
- source activate = 进入这个环境

### 最终总结
```
uv run hermes   = 工具自动准备环境 → 运行程序  ✅ 唯一正确方式
python3 hermes  = 直接用系统 Python 裸跑       ❌ 缺环境、缺依赖
./hermes        = 直接运行文件                 ❌ 必须先激活环境
source ... venv = 激活传统旧环境                ❌ 本项目不用
```

**一句话**: Hermes 用 uv 管理环境，所以永远只需 `uv run hermes`

---

## 对话二：如何判断该用什么安装/运行方式

**用户提问**: 那么如何知道用什么安装方式

**AI 回答** — 终身通用判断规则：

### 1. 命令带 uv → 用 uv run
- 适用：现代 Python 项目（Hermes, Ruff, FastAPI）
- 标志：目录下有 `pyproject.toml`、官方文档写 `uv run`

### 2. 系统工具 → 用 apt
- 适用：curl, ping, vim, git 等
- `apt install curl`

### 3. 传统 Python 工具 → 用 pip
- 适用：老项目
- `pip install 包名`

### 4. 全局命令 → 直接运行
- docker, ls, cd 等

### 终极口诀
> **看文档、看文件，有 uv 用 uv，没有用 pip，系统工具用 apt！**

### Hermes 当前环境判断
- 有 `pyproject.toml` ✅
- 有 `.uv` ✅
- 官方要求 `uv run hermes` ✅
- → 永远用 `uv run hermes 命令`

---

## 对话中穿插的搜索资料摘要

### uv 工具简介
- **uv**: Astral 公司开发，Rust 编写的新一代 Python 包管理器
- 比 pip 快 10-100 倍
- 可替代 pip, virtualenv, pip-tools，集包管理、Python 版本管理、虚拟环境、项目初始化于一体
- 与现有工具兼容：可处理 requirements.txt 和 pyproject.toml
- 常用命令: `uv init`, `uv add`, `uv remove`, `uv sync`, `uv lock`, `uv tree`, `uv build`, `uv run`
