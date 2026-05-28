---
title: 代理类型：HTTP vs SOCKS5
created: 2026-05-28
updated: 2026-05-28
type: concept
tags: [tool, tutorial, note, devops]
sources: [raw/articles/doubao-network-protocols-conversation.md]
confidence: medium
---

# 代理类型：HTTP vs SOCKS5

代理节点类型，用来翻墙、换 IP、隐藏本机地址。

## HTTP 代理
- **功能**：只负责网页、浏览器、APP 网络
- **端口**：一般 8080
- **优点**：延迟低、最简单
- **缺点**：不支持游戏、软件全局、聊天工具，加密弱

## SOCKS5 代理（最常用）
- **功能**：全能全局代理，浏览器、游戏、模拟器、所有软件都能用
- **端口**：一般 1080
- **优点**：加密更好、隐藏 IP 更彻底、稳定性最强
- **日常使用**：优先选 SOCKS5

**简单区别**：HTTP 只能上网页临时用，SOCKS5 全部软件通用主流首选。

> 还有 **HTTPS 代理** = 加密版 HTTP，介于两者中间。

## 与 SMB 的关系

SMB 协议走的是裸 TCP 连接，它不认识 HTTP 代理，也不会主动走 SOCKS5 代理。如果电脑开启了代理环境，SMB 连接会被卡住。

**解决方案**：
1. 临时关闭代理（最快验证）
2. 使用全局 TCP 代理工具：Clash for Windows「全局模式」/ ClashX Pro「增强模式」
3. 换用 WebDAV、SFTP 等替代方案

## 相关页面
- [[network-protocols-basics]] — 网络协议基础
- [[ssh-tunnel]] — SSH 隧道（内置 SOCKS5 代理功能）
- [[self-built-vpn-legal-boundary]] — 内网穿透法律边界
