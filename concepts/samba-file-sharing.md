---
title: Samba 文件共享
created: 2026-05-10
updated: 2026-05-10
type: concept
tags: [tool, devops, linux]
sources: [raw/articles/doubao-disk-reserve-samba-conversation.md]
confidence: high
---

# Samba 文件共享

**Samba** 是一套开源软件，让 Linux/Unix 系统能像 Windows 一样做「网上邻居」共享，核心是实现 Windows 的 **SMB/CIFS 协议**。

## 一句话理解

- Windows 之间共享文件用的是 SMB 协议（网上邻居）
- Linux 默认不用 SMB，所以 Windows 不能直接访问 Linux 文件夹
- **Samba = 翻译器 + 服务器**：装在 Linux 上，让 Linux 说 SMB 语言

## 核心用途

1. **文件共享（最常用）** — 把 Linux/NAS 上的目录共享给 Windows，资源管理器输入 `\\192.168.x.x\共享名` 即可访问
2. **打印机共享** — 接在 Linux 上的打印机，Windows 可直接使用
3. **权限管理** — 用户名/密码、只读/读写、不同用户不同权限

## 关键服务

| 服务 | 作用 |
|:---|:---|
| **smbd** | 核心服务，管文件/打印共享、用户认证 |
| **nmbd** | 主机名解析，没它只能 IP 访问，不能用主机名 |

## 典型场景（家庭/小型办公）

- 一台 Linux NAS（或旧电脑装 Linux）
- 硬盘总容量比如 916GiB（系统保留 46GiB，用户可用 870GiB）
- 装 Samba 把可用空间共享给所有 Windows 电脑、手机、电视
- 看电影、存照片、备份文件，不用来回传 U 盘

## 与 [[linux-disk-reserved-blocks]] 的关系

Samba 共享出去的大小 = 文件系统分区的「用户可用空间」，**root 保留的 5% 对 Windows 客户端不可见**。

## 与 Alist 的对比

参见 [[samba-vs-alist]] 对比页面。

## Samba vs [[samba-vs-alist]]

- Samba = 局域网 SMB 共享（低速高兼容）
- Alist = 多网盘聚合 + WebDAV + 网页影音（功能更广）
- **两者不冲突，可以同时用**
