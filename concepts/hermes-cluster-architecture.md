---
title: Hermes Cluster 项目架构
created: 2026-05-14
updated: 2026-05-14
type: concept
tags: [tool, agent, devops, architecture]
sources: [raw/knowledge-learning-notes.md]
confidence: medium
---

# Hermes Cluster 项目架构

多机统一管理平台，中心服务纳管多台设备的集群方案。

## 目录结构

```
hermes-cluster/
│
├── central/                          ← 中心服务（80% 代码）
│   ├── api/        路由层（薄）       ← 只做分发
│   ├── core/       业务逻辑           ← 设备/日志/目录/指令
│   ├── connector/  连接器（可插拔）    ← SSH/Docker/Hermes
│   ├── models/     数据模型           ← 统一契约
│   ├── store/      持久化             ← SQLite
│   ├── logs/       ★ 统一日志存储     ← 按设备/日期分目录
│   ├── files/      ★ 统一文件存储     ← 脚本/配置/备份/传输
│   └── frontend/   前端页面
│
├── agent/                            ← 节点代理（可选）
├── scripts/                          ← 部署运维脚本
└── docs/                             ← 文档
```

## 核心设计

- **薄 API 层**：只做请求分发，不包含业务逻辑
- **可插拔连接器**：支持 SSH/Docker/Hermes 三种连接方式，可扩展
- **统一日志/文件存储**：按设备+日期分目录集中管理
- **SQLite 持久化**：免数据库服务，部署简单

## 相关页面

- [[hermes-agent-commands]] — Hermes Agent 单机指令
- [[ubuntu-ssh-remote-setup]] — SSH 远程连接，集群节点的基础通信方式
