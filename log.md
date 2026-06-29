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

## [2026-05-07] ingest | 用户的「后端开发学习笔记」
- 来源: 用户自写的整理笔记（从豆包对话提炼）
- raw: raw/articles/user-backend-study-notes.md
- 更新: concepts/fastapi-basics.md（补充 HTTP 餐厅模型、状态码、CRUD 四句话、路由可视化、更完整 API 示例）
- 更新: concepts/sqlalchemy-orm-basics.md（补充持久化说明、nullable/index 字段约束、自动建表、创建/删除用户、CRUD 对照表、MySQL 换用指引）
- 更新: index.md

## [2026-05-08] ingest | 豆包对话：source 命令 + JWT 认证原理
- 来源: https://www.doubao.com/thread/wc75fec6254908835
- raw: raw/doubao-source-jwt-conversation.md（44轮对话，涵盖 source 命令、密码哈希、JWT 认证全流程）
- 来源: 用户 CLI 对话备份（后端学习第4课 JWT 认证实战）
- raw: raw/user-backend-jwt-conversation.md（JWT 认证 FastAPI 实现 + bcrypt 兼容性问题）
- 新建: concepts/shell-source-command.md（Linux source 命令详解）
- 新建: concepts/jwt-authentication.md（密码哈希/bcrypt/盐、JWT 签名/验签、FastAPI JWT 实现）
- 更新: concepts/fastapi-basics.md（补充 JWT 相关来源 + 添加 jwt-authentication 链接）
- 更新: index.md

## [2026-05-08] ingest | 豆包对话：CPU线程与多任务 + 句柄与系统资源管理
- 来源: https://www.doubao.com/thread/ace6be76a457c
- raw: raw/doubao-cpu-threads-handles-conversation.md（13轮对话，CPU线程/超线程/多任务/句柄/Windows vs Linux）
- 新建: concepts/cpu-threads-and-processes.md（CPU线程/进程/核心/超线程/时间片调度）
- 新建: concepts/os-handles-and-resource-management.md（句柄概念/句柄泄漏/Windows vs Linux资源管理对比）
- 更新: index.md

## [2026-05-09] ingest | 后端学习第5课 — PUT 更新接口（完整 CRUD）
- 来源: https://www.doubao.com/thread/w4a20f3697fb0bb2c
- raw: raw/articles/user-backend-put-update.md
- 更新: concepts/fastapi-basics.md（新增「PUT 更新接口」章节、完整 CRUD 一览表、测试命令）
- 更新: concepts/sqlalchemy-orm-basics.md（新增「更新用户」章节、CRUD 对照表填上 Update 行）
- 更新: index.md

## [2026-05-10] ingest | 解释磁盘空间预留 + Samba 与 Alist 对比
- 来源: https://www.doubao.com/thread/w67e3206676d9de73
- raw: raw/articles/doubao-disk-reserve-samba-conversation.md
- 新建: concepts/linux-disk-reserved-blocks.md — Linux 5% root 保留块机制
- 新建: concepts/samba-file-sharing.md — Samba 文件共享介绍
- 新建: comparisons/samba-vs-alist.md — Samba 与 Alist 对比
- 更新: index.md
- 新建: concepts/software-architecture-download.md — 软件下载页架构与格式：x64/arm64/x86、.msi/.exe
- 更新: index.md（总页数 23）

## [2026-05-10] create | Hermes Agent 技能总览
- raw: raw/articles/hermes-agent-skills-list.md
- 新建: concepts/hermes-agent-skills.md — 全部 90+ 内置技能分类整理
- 更新: index.md

## [2026-05-11] ingest | 豆包对话：浙江财经大学东方学院投资 + 通信专利SEP商业模式
- 来源: https://www.doubao.com/thread/a094d8f8b8552
- 标题: 浙江财经大学东方学院总投资建设金额
- raw: raw/articles/college-investment-patent-business-doubao-conversation.md
- 新建: concepts/independent-college-economics.md（独立学院投资与盈利模式）
- 新建: concepts/sep-patent-business-model.md（标准必要专利SEP商业模式）
- 更新: index.md
- 新建: concepts/sep-patent-business-model.md（标准必要专利SEP商业模式）
- 更新: index.md

## [2026-05-11] ingest | 豆包对话：互联网现金奶牛 + CUDA垄断 + 闻泰ST分析
- 来源: https://www.doubao.com/thread/ab354da279854
- 标题: 腾讯、字节、阿里现金奶牛对比
- raw: raw/articles/tech-cash-cows-cuda-wingtech-doubao-conversation.md
- 新建: comparisons/china-internet-cash-cows.md（中国互联网五大现金奶牛对比）
- 新建: concepts/china-cloud-market.md（中国云市场格局）
- 新建: concepts/cuda-ecosystem-monopoly.md（CUDA生态与垄断破局）
- 新建: entities/linus-torvalds.md（林纳斯·托瓦兹）
- 新建: queries/wingtech-st-analysis.md（闻泰科技ST分析）
- 更新: index.md

## [2026-05-11] ingest | 豆包对话 + 教学备份：JWT哈希机制 + 环境变量配置分离
- 来源: https://www.doubao.com/thread/w73148d3000cccd8e
- 标题: 解释 JWT 哈希原理
- raw: raw/articles/jwt-hash-env-doubao-conversation.md（豆包对话）
- raw: raw/articles/l8-env-config-teaching-backup.md（本地教学备份）
- 更新: concepts/jwt-authentication.md（添加 JWT 签名内部机制详解章节）
- 更新: concepts/fastapi-basics.md（添加环境变量配置分离 L8 章节）
- 更新: index.md

## [2026-05-14] ingest | 豆包对话：JumpServer 堡垒机部署方案 & Ubuntu 网络测试 & HTTP 404 分类
- 来源: https://www.doubao.com/thread/w62b3ec92041e5acf
- 标题: 查询堡垒机资源占用
- raw: raw/jumpserver-deployment-doubao-conversation.md
- 新建: concepts/jumpserver-bastion-deployment.md（JumpServer 资源占用与部署方案）
- 新建: queries/ubuntu-network-connectivity-test.md（Ubuntu 外网/国际网络连通性测试）
- 新建: queries/http-404-classification.md（HTTP 404 错误分类与排查思路）
- 更新: index.md（+3 pages，共 34 页）

## [2026-05-14] ingest | 本地学习笔记综合整理
- 来源: 本地笔记（知识学习.txt）
- raw: raw/knowledge-learning-notes.md
- 新建: concepts/ubuntu-ssh-remote-setup.md（Ubuntu SSH 远程连接配置）
- 新建: concepts/hermes-cluster-architecture.md（Hermes Cluster 项目架构）
- 新建: queries/ip-address-query.md（IP 地址查询方法）
- 更新: concepts/hermes-agent-commands.md（添加 Windows/Ubuntu/Docker 安装指南）
- 更新: concepts/docker-container-basics.md（添加容器内外查询命令）
- 更新: concepts/baota-panel-commands.md（添加面板端口管理）
- 更新: index.md（+4 pages，共 38 页）

## [2026-05-16] ingest | 豆包对话：自建 VPN/内网穿透技术教程与法律边界
- 来源: https://www.doubao.com/thread/af35dea467ac0
- raw: raw/vpn-internet-penetration-legal-doubao-conversation.md
- 新建: concepts/self-built-vpn-legal-boundary.md
- 更新: index.md


## [2026-05-16] ingest | 豆包对话：论文代写与 AI 去水印网站的法律风险
- 来源: https://www.doubao.com/thread/a881d000ab2d6
- raw: raw/thesis-watermark-legal-doubao-conversation.md
- 新建: concepts/thesis-writing-fraud-legal-risk.md
- 新建: concepts/ai-watermark-removal-legal-risk.md
- 更新: index.md


## [2026-05-17] ingest | Hermes Agent 架构探索对话（会话导出）
- raw: raw/hermes-agent-architecture-exploration.md
- concept: concepts/hermes-agent-architecture.md
- 内容：会话隔离/记忆系统/Profiles/多智能体协作架构的深入探讨

## [2026-05-28] ingest | 豆包对话：网络协议全面科普（HTTP/HTTPS/TCP/UDP/端口/WebDAV/SSH/NAT/代理）
- 来源: https://www.doubao.com/thread/a27115d5f40f4
- raw: raw/articles/doubao-network-protocols-conversation.md
- 新建: concepts/network-protocols-basics.md（TCP/UDP/HTTP/HTTPS 协议基础与端口速查）
- 新建: concepts/webdav-protocol.md（WebDAV 协议详解与对比）
- 新建: concepts/ssh-tunnel.md（SSH 三种隧道模式与 FRP 对比）
- 新建: concepts/proxy-types.md（HTTP/SOCKS5 代理类型对比）
- 新建: concepts/nat-port-forwarding.md（NAT vs 端口映射，FRP XTCP P2P）
- 更新: index.md（+5 页，总页数 47）

## [2026-06-16] ingest | WSL 镜像网络模式笔记合并
- 来源: 用户发送文档合并（WSL镜像网络模式笔记.md + WSL2状态与网络分析_20260616.md）
- raw: raw/articles/wsl-mirrored-network-notes.md, raw/articles/wsl2-status-network-20260616.md
- 新建: concepts/wsl-mirrored-network.md（两篇合并：WSL NAT vs Mirrored 模式对比、网络拓扑、Plan 9 文件系统、故障处理）
- 更新: index.md（+1 页，总页数 48）

## [2026-06-16] ingest | WSL2 指令速查手册
- 来源: 用户发送文档（WSL2指令速查手册_20260616.md）
- raw: raw/articles/wsl2-commands-cheatsheet.md
- 新建: concepts/wsl2-commands-cheatsheet.md（WSL2 指令速查手册，包含基础管理/发行版管理/配置/文件互操作/网络/故障处理等 9 大类）
- 更新: index.md（+1 页，总页数 49）

## [2026-06-21] ingest | DeepSeek 共享对话：常见 Windows 命令和 Linux 命令
- 来源: https://chat.deepseek.com/share/9itr8yzwo2jwvq4naf
- raw: raw/articles/deepseek-windows-linux-commands.md
- 新建: concepts/windows-linux-commands-comparison.md（Windows CMD 和 Linux Shell 18 类 100+ 命令速查，含快速对比表）
- 更新: index.md（+1 页，总页数 50）
