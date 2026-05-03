---
source_url: https://chat.deepseek.com/share/oeztngtiyz6qnloy91
ingested: 2026-05-03
sha256: 733e22ba9f146a4b33a53cc53c59d1c9de9d024ecbcf4829cd79b1c0d6c1c73b
---

# DeepSeek 分享：Hermes Agent 指令集（完整渲染版）

以下内容由 Chromium headless 渲染后从 DOM 提取。

## 对话概览

该对话全面整理了 Ubuntu 系统中 Hermes Agent 的指令使用指南，包括：

### 1. CLI 指令速查表

| 指令分类 | 指令 | 功能说明 |
|---------|------|---------|
| **核心管理** | `hermes` | TUI 界面与 Agent 对话 |
| | `hermes setup` | 配置向导 |
| | `hermes config` | 查看/修改配置 |
| **模型与认证** | `hermes model` | 交互式切换模型提供商 |
| | `hermes auth` | 管理 API 凭证 |
| **会话与记录** | `hermes sessions` | 浏览/导出/清理会话记录 |
| | `hermes logs` | 查看日志文件 |
| **诊断与维护** | `hermes doctor` | 系统诊断 |
| | `hermes update` | 更新到最新版本 |
| | `hermes backup / import` | 备份/恢复配置与数据 |
| **高级应用** | `hermes acp` | ACP 服务器（IDE 集成） |
| | `hermes mcp` | 管理 MCP 服务器 |
| | `hermes cron` | 定时任务管理 |
| | `hermes claw` | OpenClaw 迁移助手 |

### 2. CLI 全局选项
- `--version, -V`：查看版本
- `--profile <name>, -p <name>`：指定配置文件
- `--resume <session>, -r <session>`：恢复旧会话
- `--yolo`：绕过高风险操作批准
- `--tui`：强制 TUI 模式

### 3. 交互式斜杠命令
- `/help` — 帮助文档
- `/model [provider:model]` — 热切换模型
- `/skills` — 管理自动化技能
- `/compress [focus topic]` — 手动压缩上下文
- `/new` 或 `/reset` — 新会话
- `/undo` — 撤销上一轮操作
- `/insights --days N` — 查看分析报告
- `/cron` — 管理定时任务
- `/voice` — 语音模式控制

### 4. 关键文件路径
- `~/.hermes/config.yaml` — 主配置文件
- `~/.hermes/.env` — 环境变量/密钥
- `~/hermes/agents/prompts.md` — 提示词文件
- `~/hermes/gateway/config/` — 网关配置
- `~/hermes/skills/` — 技能文件
