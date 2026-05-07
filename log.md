# Wiki Log

> 所有操作的时间线记录。只追加不删除。
> 格式: `## [YYYY-MM-DD] 操作 | 标题`
> 操作类型: ingest(录入), update(更新), query(问答), lint(检查), create(创建), archive(归档), delete(删除)

## [2026-05-02] create | 知识库初始化
- 领域: AI/ML 学习笔记 + 泛科技杂记
- 已创建: SCHEMA.md, index.md, log.md
- 目录结构: raw/, entities/, concepts/, comparisons/, queries/

## [2026-05-02] ingest | 豆包对话：宝塔面板命令 + Docker容器管理
- 来源: https://www.doubao.com/thread/a4bcce9625dea
- raw: raw/baota-docker-doubao-conversation.md
- 新建: concepts/baota-panel-commands.md, concepts/docker-container-basics.md
- 更新: index.md

## [2026-05-02] ingest | 豆包对话：Hermes 运行方式 & uv 环境管理
- 来源: https://www.doubao.com/thread/ad70e7e69eb4d
- raw: raw/hermes-uv-env-doubao-conversation.md
- 新建: concepts/python-environment-management.md
- 更新: index.md

## [2026-05-02] ingest | 豆包对话：docker commit 打包范围详解
- 来源: https://www.doubao.com/thread/a8926a1e854a5
- raw: raw/docker-commit-scope-doubao-conversation.md
- 更新: concepts/docker-container-basics.md（补充 commit 打包范围章节）
- 更新: index.md

## [2026-05-02] ingest | 豆包对话：时间认知与选择 — 曝光效应与第一印象
- 来源: https://www.doubao.com/thread/ab2122250c8af
- raw: raw/mere-exposure-effect-doubao-conversation.md
- 新建: concepts/mere-exposure-effect.md
- 更新: index.md

## [2026-05-03] update | DeepSeek 分享：Hermes Agent 指令集（补全完整内容）
- 使用 chromium headless 浏览器成功渲染 SPA 页面，提取完整对话
- raw: raw/articles/deepseek-hermes-agent-commands.md（更新为完整版）
- 更新: concepts/hermes-agent-commands.md（补充全局选项、认证凭证、诊断维护、高级应用、文件路径等章节）
- 更新: index.md

## [2026-05-03] ingest | DeepSeek 分享：Hermes Agent 指令集
- 来源: https://chat.deepseek.com/share/oeztngtiyz6qnloy91
- raw: raw/articles/deepseek-hermes-agent-commands.md
- 新建: concepts/hermes-agent-commands.md
- 更新: index.md

## [2026-05-02] update | 补充第二个主题——时间认知与选择
- raw: raw/mere-exposure-effect-doubao-conversation.md（追加主题二）
- 新建: concepts/time-exclusivity-and-efficiency.md
- 更新: index.md

## [2026-05-06] ingest | 豆包对话：HDD vs SSD vs 玻璃存储技术对比
- 来源: https://www.doubao.com/thread/aa4b7875589fe
- raw: raw/hdd-ssd-glass-doubao-conversation.md
- 新建: comparisons/hdd-vs-ssd-vs-glass-storage.md
- 更新: index.md

## [2026-05-06] ingest | 豆包对话：Cloudflare Tunnel + 域名基础 + ICP 备案
- 来源: https://www.doubao.com/thread/a0219e5411382
- 30轮问答，涵盖 Cloudflare Tunnel 内网穿透、公网IP现状、域名vs商标、TLD、DNS、ICANN/CNNIC、注册续费、ICP备案
- raw: raw/articles/cloudflare-tunnel-domains-doubao-conversation.md
- 新建: concepts/cloudflare-tunnel.md
- 新建: concepts/domain-name-basics.md
- 新建: concepts/china-icp-beian.md
- 新建: concepts/china-home-broadband-public-ip.md
- 更新: index.md

## [2026-05-07] ingest | 豆包对话：FastAPI 环境搭建 + ORM 原理 + /proc 文件系统
- 来源: https://www.doubao.com/thread/w4ca1d3395d9f845c
- 来源: https://www.doubao.com/thread/wb7331b7dfc9ae6df
- raw: raw/articles/fastapi-backend-doubao-conversation.md（FastAPI 入门教学，34 轮对话）
- raw: raw/articles/proc-orm-doubao-conversation.md（/proc + ORM 详解，8 轮对话）
- 新建: concepts/fastapi-basics.md
- 新建: concepts/sqlalchemy-orm-basics.md
- 新建: concepts/linux-proc-filesystem.md
- 更新: index.md
