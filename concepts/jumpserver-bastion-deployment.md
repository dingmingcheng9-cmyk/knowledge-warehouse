---
title: JumpServer 堡垒机资源占用与部署方案
created: 2026-05-14
updated: 2026-05-14
type: concept
tags: [devops, tool, product]
sources: [raw/jumpserver-deployment-doubao-conversation.md]
confidence: medium
---

# JumpServer 堡垒机资源占用与部署方案

JumpServer 是最流行的开源堡垒机（Bastion Host / Jump Box），提供 Web SSH、权限管控、操作录屏等功能。但其**全家桶微服务架构**导致资源占用较高，部署时需要合理规划。

## 资源占用基准

| 场景 | CPU | 内存 |
|------|-----|------|
| 空载（无人登录） | 1%–5% | 1.2G–1.6G（主要被 MySQL/Redis 吃掉） |
| 10–20 并发会话 | 10%–25% | 2G–2.5G |

JumpServer 包含：前端、后端、MySQL、Redis、Celery 任务、网关、录屏服务 ≈ **七八个微服务组件**。这不是功能强，而是架构臃肿。

## 服务器配置建议

### 2核2G（最低配置）
- ✅ **只跑 JumpServer**：空载 1.2G–1.6G，够用但很紧
- ❌ **JumpServer + 宝塔**：内存直接吃满 → OOM → 卡死重启
- ❌ **JumpServer + Hermes + 宝塔**：不可能

### 2核4G（推荐配置）
- ✅ **JumpServer 单独跑**：轻轻松松，剩余 2G+
- ✅ **JumpServer + Hermes**：系统 500M + JumpServer 1.3G + Hermes 500M = 2.28G，还剩 1.7G 富余
- ❌ **JumpServer + 宝塔 + Hermes**：JumpServer 1.2G + 宝塔 800M + Hermes 500M + 系统 ≈ 超 4G，扛不住

## 轻量级替代方案

JumpServer 太重，以下替代品空载只需 **300M–600M 内存**：

1. **Apache Guacamole** — Apache 官方项目，纯 HTML5 远程桌面/SSH 网关
2. **GateOne** — Web SSH 终端，极简架构
3. **Teleport（社区版）** — Gravitational 出品，开源 SSH 基础设施

## 推荐部署架构

### 方案 A（一机精简）
**阿里云 2核4G 一台**
- 卸载宝塔
- 只装：JumpServer + Hermes
- 用途：统一管控入口

### 方案 B（双机拆分，最稳）
- **机器 A：2核2G** → 纯跑 JumpServer 堡垒机
- **机器 B：2核4G+** → 宝塔 + Hermes + 容器业务

### 方案 C（极致省资源）
- **2核2G 一台** → 放弃 JumpServer，改用 Guacamole/Teleport（300-600M 空载）
- 省下的资源还可同时跑 Hermes

## 相关页面

- [[baota-panel-commands]] — 宝塔面板命令速查，JumpServer 环境不应同时安装宝塔
- [[hermes-agent-commands]] — Hermes Agent 指令集，常与堡垒机同机部署需规划资源
- [[docker-container-basics]] — Docker 容器管理，JumpServer 多通过容器部署
