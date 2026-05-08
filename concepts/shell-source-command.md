---
title: Shell source 命令
created: 2026-05-08
updated: 2026-05-08
type: concept
tags: [tool, tutorial, devops]
sources: [raw/doubao-source-jwt-conversation.md]
confidence: high
---

# Shell source 命令

> `source` 命令用于在当前 Shell 进程中执行脚本文件，不创建子进程。常用于刷新环境变量和激活虚拟环境。

## 作用

**在当前终端进程**里执行脚本文件，不新建子进程。脚本里改环境变量、切换目录，会直接影响当前终端。

### 语法

```bash
source 文件名
# 简写（常用）
. 文件名
```

## 核心特点

1. **不开启新进程** — 脚本里的环境变更直接影响当前 Shell
2. **对比 `bash 文件名` / `./文件名`** — 会新建子进程，子进程里的修改退出后消失
3. 常用于：
   - 刷新环境变量：`source ~/.bashrc`
   - 激活虚拟环境：`source venv/bin/activate`

## `source venv/bin/activate` 逐句拆解

| 部分 | 含义 |
|------|------|
| `source` | 在当前终端进程执行，不新建子进程 |
| `venv/` | 虚拟环境文件夹（独立的 Python 解释器和包） |
| `bin/` | binary 目录（可执行文件和激活脚本） |
| `activate` | 激活脚本（修改环境变量，让 Python 指向该环境） |

**激活后发生的事：**
- `PATH` 被修改：虚拟环境的 `bin/` 优先于系统路径
- `which python` → 指向虚拟环境里的 python
- `pip install` → 包装进虚拟环境，不影响系统

### 激活 vs 不激活对比

| | 不激活 | 激活后 |
|---|---|---|
| `python` | 系统默认 Python | 虚拟环境里的 Python |
| `pip install` | 装到系统全局 | 装到虚拟环境 |
| 项目隔离 | ❌ 互相污染 | ✅ 每个项目独立 |

### 脚本中慎用 source

在脚本中执行 `source venv/bin/activate` 后，pip 安装仍在原环境。正确的做法是在**虚拟环境激活后的命令行**中操作，或者直接使用 `venv/bin/pip install` 无需激活也能安装到虚拟环境。

## 相关页面

- [[python-environment-management]] — Python 虚拟环境与依赖管理
- [[fastapi-basics]] — FastAPI 后端框架基础（用到虚拟环境）
