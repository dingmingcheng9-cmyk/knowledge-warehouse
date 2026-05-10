---
title: Hermes Agent 技能总览
created: 2026-05-10
updated: 2026-05-10
type: concept
tags: [tool, tutorial, note]
sources: [raw/articles/hermes-agent-skills-list.md]
confidence: high
---

# Hermes Agent 技能总览

Hermes Agent 内置了 90+ 个技能（Skills），按功能领域分类。技能是可复用的工作流模板，用 `skill_view(name)` 加载后即可按步骤执行复杂任务。

> 所有技能列表可通过 `skills_list` 命令查看，用 `skill_view(name)` 加载具体技能内容。

---

## 自治 AI 代理

| 技能 | 说明 |
|:---|:---|
| `claude-code` | 用 Claude Code CLI 代理写代码（功能/PR） |
| `codex` | 用 OpenAI Codex CLI 代理写代码（功能/PR） |
| `hermes-agent` | Hermes Agent 自身的配置/扩展/故障排除 |
| `opencode` | 用 OpenCode CLI 代理写代码/审查 PR |

## 创意

| 技能 | 说明 |
|:---|:---|
| `architecture-diagram` | SVG 架构/云/基础设施图（暗色主题 HTML） |
| `ascii-art` | ASCII 艺术字 / pyfiglet / cowsay / boxes |
| `ascii-video` | 视频/音频转彩色 ASCII MP4/GIF |
| `baoyu-comic` | 知识漫画（教育/传记/教程） |
| `baoyu-infographic` | 信息图（21种布局 × 21种风格） |
| `claude-design` | 一次性 HTML 设计作品（落地页/演示/原型） |
| `comfyui` | 用 ComfyUI 生成图像/视频/音频 |
| `design-md` | 编写/验证 Google DESIGN.md 令牌规范文件 |
| `excalidraw` | Excalidraw 手绘风 JSON 图（架构/流程/时序图） |
| `humanizer` | 去 AI 味，添加真实人声 |
| `ideation` | 通过创意约束生成项目点子 |
| `manim-video` | Manim CE 动画（3Blue1Brown 风格数学/算法视频） |
| `p5js` | p5.js 创意编程 / 生成艺术 / 3D |
| `pixel-art` | 像素艺术（NES/Game Boy/PICO-8 色盘） |
| `popular-web-designs` | 54 套真实设计系统的 HTML/CSS 实现 |
| `pretext` | 浏览器趣味 Demo 构建 |
| `sketch` | 一次性 HTML 线框图（2-3种设计变体对比） |
| `songwriting-and-ai-music` | 写歌技巧 + Suno AI 音乐提示词 |
| `touchdesigner-mcp` | 通过 twozero MCP 控制 TouchDesigner |

## 数据科学

| 技能 | 说明 |
|:---|:---|
| `jupyter-live-kernel` | Jupyter 实时内核交互式 Python |

## DevOps

| 技能 | 说明 |
|:---|:---|
| `baota-panel` | 宝塔 Linux 面板排障 |
| `docker-container-backup` | Docker 容器全量备份（多硬盘 + 周定时） |
| `docker-container-lifecycle` | Docker 容器全生命周期管理 |
| `kanban-orchestrator` | 任务分解 + 专家分工 |
| `kanban-worker` | Kanban 工作器陷阱/示例 |
| `nas-raidrive-mapping` | Alist WebDAV + Samba 映射 NAS 到 Windows |
| `storage-management` | 磁盘检测/分区/格式化/挂载/监控 |
| `ubuntu-proxy-singbox` | Ubuntu sing-box anytls 代理客户端配置 |
| `webhook-subscriptions` | Webhook 订阅事件驱动执行 |

## 内测

| 技能 | 说明 |
|:---|:---|
| `dogfood` | Web 应用探索性 QA（找 bug + 出报告） |

## 邮件

| 技能 | 说明 |
|:---|:---|
| `himalaya` | Himalaya CLI 终端邮件（IMAP/SMTP） |

## 游戏

| 技能 | 说明 |
|:---|:---|
| `minecraft-modpack-server` | 搭建模组 Minecraft 服务器（CurseForge/Modrinth） |
| `pokemon-player` | 模拟器玩宝可梦（Headless + RAM 读取） |

## GitHub

| 技能 | 说明 |
|:---|:---|
| `codebase-inspection` | pygount 代码统计（行数/语言/比例） |
| `github-auth` | GitHub 认证设置（HTTPS/SSH/gh CLI） |
| `github-code-review` | 审查 PR（差异对比 + 行内评论） |
| `github-issues` | 创建/分类/标记/分配 Issue |
| `github-pr-workflow` | PR 生命周期（分支/提交/CI/合并） |
| `github-repo-management` | 克隆/创建/复刻仓库 + 发布管理 |

## Hermes Agent 自身

| 技能 | 说明 |
|:---|:---|
| `memory-organization` | 定期整理 Hermes Agent 持久记忆 |

## MCP

| 技能 | 说明 |
|:---|:---|
| `native-mcp` | MCP 客户端（stdio/HTTP 连接服务器注册工具） |

## 媒体

| 技能 | 说明 |
|:---|:---|
| `gif-search` | 从 Tenor 搜索/下载 GIF |
| `heartmula` | 歌词 + 标签生成 Suno 风格歌曲 |
| `songsee` | 音频频谱图/特征可视化 |
| `spotify` | Spotify 播放/搜索/队列/管理 |
| `youtube-content` | YouTube 转录 → 摘要/帖子/博客 |

## MLOps

| 技能 | 说明 |
|:---|:---|
| `huggingface-hub` | HuggingFace hf CLI 搜索/下载/上传模型/数据集 |
| `evaluating-llms-harness` | lm-eval-harness LLM 基准测试（MMLU/GSM8K 等） |
| `weights-and-biases` | W&B ML 实验追踪/模型注册/仪表板 |
| `llama-cpp` | 本地 GGUF 推理 + HF Hub 模型发现 |
| `obliteratus` | 移除 LLM 审查/拒答（diff-in-means） |
| `outlines` | 结构化 JSON/正则/Pydantic 输出生成 |
| `serving-llms-vllm` | vLLM 高性能 LLM 服务 |
| `audiocraft-audio-generation` | MusicGen 文生音乐 / AudioGen 文生音效 |
| `segment-anything-model` | SAM 零样本图像分割 |
| `dspy` | 声明式 LM 程序，自动优化提示词 |
| `axolotl` | YAML 配置 LLM 微调（LoRA/DPO/GRPO） |
| `fine-tuning-with-trl` | TRL 框架（SFT/DPO/PPO/GRPO/奖励模型） |
| `unsloth` | 2-5 倍速 LoRA/QLoRA 微调，更低显存 |

## 笔记

| 技能 | 说明 |
|:---|:---|
| `obsidian` | 读取/搜索/创建 Obsidian 笔记 |

## 生产力

| 技能 | 说明 |
|:---|:---|
| `airtable` | Airtable REST API（CRUD/筛选/Upsert） |
| `google-workspace` | Gmail/日历/Drive/文档/表格（gws CLI） |
| `linear` | Linear Issue 管理（GraphQL） |
| `maps` | 地理编码/POI/路线/时区（OpenStreetMap/OSRM） |
| `nano-pdf` | PDF 文字/标题编辑（自然语言提示） |
| `notion` | Notion API（页面/数据库/块/搜索） |
| `ocr-and-documents` | PDF/扫描件文字提取（pymupdf/marker-pdf） |
| `powerpoint` | 创建/读取/编辑 .pptx 演示文稿 |

## 红队

| 技能 | 说明 |
|:---|:---|
| `godmode` | 越狱 LLM（Parseltongue/GODMODE/ULTRAPLINIAN） |

## 研究

| 技能 | 说明 |
|:---|:---|
| `arxiv` | arXiv 论文搜索（关键词/作者/分类/ID） |
| `blogwatcher` | RSS/Atom 订阅监控 |
| `llm-wiki` | Karpathy 风格 LLM 知识库构建与维护 |
| `polymarket` | Polymarket 预测市场查询 |

## 智能家居

| 技能 | 说明 |
|:---|:---|
| `openhue` | 控制 Philips Hue 灯光/场景/房间 |

## 社交媒体

| 技能 | 说明 |
|:---|:---|
| `xurl` | X/Twitter 发帖/搜索/私信/媒体/API |

## 软件开发

| 技能 | 说明 |
|:---|:---|
| `debugging-hermes-tui-commands` | 调试 Hermes TUI 斜杠命令 |
| `hermes-agent-skill-authoring` | 编写 SKILL.md 技能文件 |
| `node-inspect-debugger` | Node.js --inspect + Chrome DevTools 调试 |
| `plan` | 计划模式：写 Markdown 计划，不执行 |
| `python-debugpy` | Python pdb + debugpy 远程调试 |
| `requesting-code-review` | 提交前审查（安全/质量/自动修复） |
| `spike` | 验证想法的一次性实验 |
| `subagent-driven-development` | 用子代理执行计划（两阶段审查） |
| `systematic-debugging` | 4 阶段根因调试法 |
| `test-driven-development` | TDD 流程（RED-GREEN-REFACTOR） |
| `writing-plans` | 编写实现计划 |

## 元宝

| 技能 | 说明 |
|:---|:---|
| `yuanbao` | 元宝企业微信 @提及用户/查询信息/群管理 |

---

## 使用方式

```bash
# 列出所有可用技能
skills_list

# 按分类筛选
skills_list category="devops"

# 加载技能内容
skill_view(name="llama-cpp")

# 加载技能内的辅助文件
skill_view(name="llama-cpp", file_path="references/api.md")
```

技能会在相关任务触发时自动加载（通过系统提示中的 `available_skills` 匹配），也可手动加载。

> 共 **90+** 个技能，持续增长中。新技能通过 `skill_manage(action='create')` 添加。技能有 Bug 或缺失步骤时，用 `skill_manage(action='patch')` 修复。
