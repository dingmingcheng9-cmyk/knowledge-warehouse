---
title: WebDAV 协议
created: 2026-05-28
updated: 2026-05-28
type: concept
tags: [tool, tutorial, note]
sources: [raw/articles/doubao-network-protocols-conversation.md]
confidence: medium
---

# WebDAV 协议

WebDAV（Web Distributed Authoring and Versioning）= 带"读写权限"的 HTTP，专门用来通过网络管理文件（上传/下载/改文件名/建目录/锁文件）。

## 与 HTTP/HTTPS 的关系

- **底层**：TCP
- **端口**：HTTP 80 / HTTPS 443（推荐加密）
- **本质**：HTTP 的扩展
  - 普通 HTTP：只能"看"（GET）
  - WebDAV：多了 PUT/DELETE/MKCOL/COPY/MOVE/PROPFIND 等"写"动作
- **类比**：HTTP = 只能浏览网页；WebDAV = 能把网页服务器当网络硬盘用

## 核心用途

- ✅ 像本地文件夹一样挂载远程 NAS/服务器/网盘
- ✅ 跨平台：Windows、macOS、Linux、手机都能连
- ✅ 多用户协作：文件锁定（防止两人同时改一个文件）
- ✅ 私有云、照片备份、笔记同步、NAS 外网访问常用

## 协议对比

| 协议 | 端口 | 特点 |
|------|------|------|
| HTTP | 80 | 网页浏览，只读，明文 |
| HTTPS | 443 | 网页浏览，只读，加密 |
| **WebDAV** | 80/443 | 文件读写，可加密，基于 HTTP |
| SMB | 445 | Windows 局域网文件共享，不跨公网，易被攻击 |
| FTP | 21 | 老牌文件传输，明文，防火墙容易挡 |

## 选型建议

- **内网 Windows 共享** → SMB（445）
- **外网/跨平台/安全** → WebDAV over HTTPS（443）
- **旧设备/简单传文件** → FTP

## 安全要点

- ❌ 不要裸奔用 HTTP 公网暴露：账号密码明文传输
- ✅ 公网必须 HTTPS + 强密码 + 限制 IP
- ✅ 内网用 HTTP 没问题，速度快、省事

## 相关页面
- [[network-protocols-basics]] — 网络协议基础
- [[samba-file-sharing]] — Samba 文件共享
- [[samba-vs-alist]] — Samba 与 Alist 定位对比
- [[self-built-vpn-legal-boundary]] — 内网穿透法律边界
