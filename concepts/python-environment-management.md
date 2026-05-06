---
title: Python 环境管理方式对比
created: 2026-05-02
updated: 2026-05-02
type: concept
tags: [tool, devops, note, comparison]
sources: [raw/hermes-uv-env-doubao-conversation.md]
---

# Python 环境管理方式对比

> uv / pip / venv / 系统 Python 的区别与适用场景。

## 四种运行方式对比

| 方式 | 命令 | 适用场景 | 说明 |
|------|------|----------|------|
| **uv** | `uv run 程序` | 现代 Python 项目 | 自动准备环境 + 运行。有 `pyproject.toml` 和 `.uv/` 目录的项目用此方式 |
| **系统 Python** | `python3 脚本.py` | 临时测试、系统脚本 | 裸跑，不加载项目环境，缺依赖就报错 |
| **./ 直接执行** | `./脚本` | 已激活环境 | 文件需有 `#!/usr/bin/env python` shebang，且环境已手动激活 |
| **source venv** | `source venv/bin/activate` | 传统 Python 项目 | 先激活虚拟环境，再运行命令 |

## 如何判断该用什么方式

### 终身通用判断口诀

> **看文档、看文件，有 uv 用 uv，没有用 pip，系统工具用 apt！**

### 判断流程

1. **命令前带 `uv`** → 用 `uv run`（现代项目，有 `pyproject.toml`）
2. **系统工具**（curl, ping, vim, git）→ 用 `apt install`
3. **传统 Python 工具** → 用 `pip install`
4. **全局命令**（docker, ls, cd）→ 直接运行
5. **上述都不是** → 看项目 `README.md` 或官方文档

### Hermes Agent 示例

```bash
# 正确
uv run hermes chat        # 启动聊天
uv run hermes gateway      # 启动网关

# 错误
python3 hermes             # 缺依赖，报错
./hermes                   # 环境未激活
```

## uv 工具简介

- **开发者**: Astral 公司
- **语言**: Rust
- **定位**: 新一代 Python 包管理器，替代 pip + virtualenv + pip-tools
- **特点**: 比 pip 快 10-100 倍，统一项目管理
- **兼容**: 可处理 `requirements.txt` 和 `pyproject.toml`

### uv 常用命令
```bash
uv init               # 创建新项目
uv add 包名           # 添加依赖
uv remove 包名        # 移除依赖
uv sync               # 同步依赖到环境
uv lock               # 创建锁文件
uv tree               # 查看依赖树
uv run 程序           # 运行项目程序
uv build              # 构建分发包
```

## 相关概念

- [[docker-container-basics]] — Docker 容器运行方式对比
- [[baota-panel-commands]] — 宝塔面板管理
- [[proxy-setup-sing-box]]

## 资料来源

- [豆包分享：Hermes 运行方式 & uv 环境管理](https://www.doubao.com/thread/ad70e7e69eb4d)
