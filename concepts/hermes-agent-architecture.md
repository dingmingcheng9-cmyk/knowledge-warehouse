---
title: Hermes Agent 架构探秘：Profiles/会话/记忆系统
created: 2026-05-17
updated: 2026-05-17
type: concept
tags: [hermes-agent, architecture, technical]
sources: [raw/hermes-agent-architecture-exploration.md]
confidence: high
---

# Hermes Agent 架构探秘

> 基于用户与 Hermes Agent 的深度对话，梳理 Hermes 的核心架构设计。

## 一、会话隔离机制

每个会话（Telegram/Discord/终端/WebUI）都是**独立隔离的上下文**：

```
用户 → 会话 A ⟷ Hermes Agent（独立上下文）
用户 → 会话 B ⟷ Hermes Agent（独立上下文）
```

- 每个会话有独立的聊天历史、工具调用记录
- 会话之间**不会互相干扰或"乱套"**
- 但可以**访问同一台机器上的文件系统**

## 二、记忆系统（Memory）

记忆是**跨会话共享**的：

| 方面 | 说明 |
|------|------|
| 存储目标 | `memory`（环境事实/工作流）和 `user`（用户画像） |
| 共享范围 | 该 Hermes 实例上的**所有会话**共用 |
| 写入 | 通过 `memory` 工具（add/replace/remove） |
| 查询 | 每次对话自动注入到系统提示中 |

**重要规则：** 不保存任务进度、临时状态、已完成的 TODO 类信息（7天内会过时）。依赖 `session_search` 回溯历史对话。

## 三、Profiles 系统（多智能体实例）

> [30]-[68] 消息深入探讨了 Profiles。

Profile = **一个完整的 Hermes Agent 实例**，拥有独立配置与记忆。

### Profile vs 会话

| | 会话（Session） | Profile（用户配置） |
|---|---|---|
| 性质 | 一次对话 | 一个独立智能体实例 |
| 上下文 | 独立（仅当前对话） | 独立（该实例的全部对话） |
| 记忆 | 跨会话共享同一记忆池 | **每个 Profile 有自己的记忆池** |
| 配置 | 继承当前 Profile | 独立模型/网关/工具集 |
| 生命周期 | 临时 | 持久 |

### 创建 Profile 的意义

```bash
hermes profile create --name "助手A" --model deepseek-v4-flash
```

1. 创建了一个**新的独立 Agent 实例**
2. 有**自己独立的记忆和配置**
3. 可绑定**不同的通信平台**
4. 可在同一个 Hermes 进程中**并行运行**

### 内存占用

Profile 本身很轻量——本质是配置文件 + 记忆库（JSON 文件）。**运行时内存消耗主要取决于模型推理和工具调用**，Profile 管理本身开销极小。

## 四、多智能体协作架构

用户提出了一个有深度的架构方案：

```
                  ┌─────────────────────┐
                  │   协调智能体（中间人）   │
                  │  (Orchestrator Profile)│
                  └─────┬───────┬─────────┘
                        │       │
              ┌─────────▼─┐   ┌─▼─────────┐
              │ 智能体 A   │   │ 智能体 B   │
              │(Profile A) │   │(Profile B) │
              │ 记忆独立    │   │ 记忆独立    │
              └───────────┘   └───────────┘
```

- **群聊模式**：用户@具体智能体，智能体响应
- **智能体间互相@**：当前版本似乎还不支持自动触发，需要用户手动@
- **中间协调智能体**：一个负责调度/转发的 orchestrator Profile，可协调多个专业子智能体

## 五、用户部署环境

- **平台**：hermes-web-ui（V0.5.28，EKKOLearnAI/hermes-web-ui）
- **服务器**：阿里云 ECS
- **连接模式**：Bridge 模式
- **模型**：DeepSeek（deepseek-v4-flash）
- **语言**：中文

## 六、对话中的中断问题

用户在对话过程中经历了多次"断线"（[72]-[89]），原因可能是：
- 长时间会话 token 超限
- 后端进程意外重启
- WebSocket 连接超时

---

## 相关链接

- [[hermes-agent-learning-path]]（Hermes Agent 学习路径）
