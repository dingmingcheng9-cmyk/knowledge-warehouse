---
title: Hermes Agent 指令集（Ubuntu）
created: 2026-05-03
updated: 2026-05-03
type: concept
tags: [tool, llm, agent, cli, tutorial]
sources: [raw/articles/deepseek-hermes-agent-commands.md]
confidence: high
---

# Hermes Agent 指令集（Ubuntu）

Hermes Agent 是 Nous Research 开发的开源 AI Agent 框架，在 Ubuntu 终端中通过 `hermes` 命令使用。以下是常用指令速查。

## CLI 命令

### 核心管理
| 指令 | 说明 |
|------|------|
| `hermes` | 启动 TUI 交互界面 |
| `hermes setup` | 配置向导（初始化/修改全局配置） |
| `hermes doctor` | 检查依赖和配置健康 |
| `hermes status` | 查看组件状态 |

### 对话
| 指令 | 说明 |
|------|------|
| `hermes chat -q "query"` | 一次性查询，非交互模式 |
| `hermes -m model` | 指定模型 |
| `hermes --continue` | 继续上次会话 |
| `hermes --resume SESSION` | 恢复指定会话 |

### 配置
| 指令 | 说明 |
|------|------|
| `hermes config` | 查看当前配置 |
| `hermes config edit` | 编辑 config.yaml |
| `hermes config set KEY VAL` | 设置配置值 |
| `hermes config list` | 显示所有生效配置项 |
| `hermes model` | 交互式模型/提供商选择 |
| `hermes auth` | 管理 API 凭证（添加/列出/删除/策略设置） |

### 认证 & 凭证
| 指令 | 说明 |
|------|------|
| `hermes auth` | 交互式凭证向导 |
| `hermes auth list [provider]` | 列出指定提供商的凭据 |
| `hermes auth remove P INDEX` | 删除凭证 |
| `hermes auth reset PROVIDER` | 清除耗尽状态 |

### 全局选项
| 选项 | 说明 |
|------|------|
| `--version, -V` | 查看版本 |
| `--profile <name>, -p <name>` | 指定隔离配置文件 |
| `--resume <session>, -r <session>` | 恢复指定会话 |
| `--continue, -c` | 继续上次会话 |
| `--yolo` | 绕过高风险操作批准 |
| `--tui` | 强制 TUI 模式 |
| `--skills, -s SKILL` | 预加载技能 |
| `--worktree, -w` | 隔离 git worktree 模式 |

### 工具 & 技能
| 指令 | 说明 |
|------|------|
| `hermes tools list` | 列出所有工具及状态 |
| `hermes tools enable/disable NAME` | 启用/禁用工具集 |
| `hermes skills list` | 列出已安装技能 |
| `hermes skills install ID` | 安装技能 |
| `hermes skills search QUERY` | 搜索技能市场 |

### 会话管理
| 指令 | 说明 |
|------|------|
| `hermes sessions list` | 列出近期会话 |
| `hermes sessions rename ID NAME` | 重命名会话 |
| `hermes sessions delete ID` | 删除会话 |
| `hermes sessions prune` | 清理旧会话 |

### 网关（消息平台）
| 指令 | 说明 |
|------|------|
| `hermes gateway run` | 启动网关（前台） |
| `hermes gateway install` | 安装为后台服务 |
| `hermes gateway start/stop/restart` | 控制网关服务 |

### 定时任务
| 指令 | 说明 |
|------|------|
| `hermes cron list` | 列出任务 |
| `hermes cron create SCHED` | 创建定时任务 |
| `hermes cron remove ID` | 删除任务 |

### 诊断 & 维护
| 指令 | 说明 |
|------|------|
| `hermes doctor [--fix]` | 一键诊断配置和系统依赖 |
| `hermes logs` | 查看/跟踪代理、网关等日志文件 |
| `hermes update` | 更新到最新版本 |
| `hermes backup` | 备份整个 Hermes 主目录（ZIP） |
| `hermes import FILE` | 从 ZIP 恢复配置与数据 |

### 高级应用
| 指令 | 说明 |
|------|------|
| `hermes acp` | 作为 ACP 服务器运行（IDE 集成） |
| `hermes mcp` | 管理 MCP 服务器配置 |
| `hermes cron` | 定时任务调度 |
| `hermes claw` | OpenClaw 迁移助手 |
| `hermes webhook` | Webhook 订阅管理 |

### 关键文件路径
| 文件 | 路径 | 说明 |
|------|------|------|
| 主配置文件 | `~/.hermes/config.yaml` | YAML 格式，模型定义、端点等核心参数 |
| 环境变量 | `~/.hermes/.env` | API 密钥等敏感信息 |
| 提示词文件 | `~/hermes/agents/prompts.md` | 自定义 Agent 系统指令 |
| 网关配置 | `~/hermes/gateway/config/` | 外部消息平台配置 |
| 技能文件 | `~/hermes/skills/` | Agent 自我生成的自动化技能 |

> 提示：Ubuntu 下 `~/.hermes` 是隐藏目录，需按 `Ctrl+H` 显示。

### 配置向导子命令
`hermes setup` 支持精确调整子模块：
- `hermes setup model` — 仅修改 LLM 提供商和模型
- `hermes setup gateway` — 仅修改聊天平台配置
- `hermes setup terminal` — 终端后端配置
- `hermes setup tools` — 工具集管理

## 交互式斜杠命令

在会话中可直接输入以下命令（以 `/` 开头）：

### 会话控制
| 指令 | 说明 |
|------|------|
| `/new` 或 `/reset` | 新会话 |
| `/retry` | 重发最后一条消息 |
| `/undo` | 撤销上一轮对话 |
| `/title [name]` | 命名会话 |
| `/compress` | 手动压缩上下文 |

### 配置
| 指令 | 说明 |
|------|------|
| `/model [name]` | 查看或切换模型 |
| `/yolo` | 跳过危险命令确认 |
| `/voice [on/off/tts]` | 语音模式 |
| `/verbose` | 切换详细输出级别 |

### 信息查询
| 指令 | 说明 |
|------|------|
| `/help` | 显示帮助 |
| `/usage` | 查看 Token 用量 |
| `/status` | 会话信息 |
| `/platforms` 或 `/gateway` | 查看平台连接状态 |
| `/quit` 或 `/exit` | 退出 CLI |

## 进阶功能

- **子代理（Subagent）**：通过 `delegate_task` 工具派生子任务，支持并行执行
- **ACP 服务器**：`hermes acp` — IDE 集成
- **MCP 服务器**：`hermes mcp` 系列命令管理
- **定时任务**：`hermes cron` 系列
- **技能系统**：`/skill name` 加载技能，`hermes skills` 管理
- **记忆系统**：跨会话持久记忆，支持多后端（内置、Honcho、Mem0）
- **配置文件**：`~/.hermes/config.yaml`（配置）+ `~/.hermes/.env`（密钥）

> 完整文档：https://hermes-agent.nousresearch.com/docs/
> 本页内容综合自 DeepSeek 分享对话及官方 API 文档。
