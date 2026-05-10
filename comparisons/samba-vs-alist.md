---
title: Samba vs Alist — 文件共享方案对比
created: 2026-05-10
updated: 2026-05-10
type: comparison
tags: [tool, devops, linux]
sources: [raw/articles/doubao-disk-reserve-samba-conversation.md]
confidence: high
---

# Samba vs Alist

一句话：**两者不冲突，互补关系，可以同时用**。

## 定位对比

| 维度 | Samba | Alist |
|:---|:---|:---|
| **核心定位** | 局域网文件共享 | 多网盘聚合 + 网页挂载管理 |
| **协议** | SMB（Windows 网上邻居） | WebDAV + 网页端 |
| **适用场景** | 电脑/电视/盒子直接读本地硬盘 | 管理各大网盘、网页看视频、手机在线浏览 |
| **存储范围** | 仅本地目录 | 本地 + 百度网盘/阿里云盘/夸克/OneDrive 等 |
| **速度** | 局域网内满速 | 受网盘/网络限制 |
| **安装复杂度** | 简单（apt install samba） | 简单（二进制/一键脚本） |

## 为什么不冲突

1. **使用场景不重叠**
   - 电脑/电视/播放器直接读本地硬盘 → **Samba** 最稳，满速，不用开网页
   - 管理各大网盘、网页看视频、手机在线浏览 → **Alist**

2. **可以互相配合**
   - 本地硬盘用 **Samba** 给局域网设备直连
   - 同一块硬盘同时挂进 **Alist**，远程/手机网页也能看
   - 一套物理硬盘，两个服务并行，互不抢端口

3. **功能边界清晰**
   - Samba：只管局域网 SMB 共享
   - Alist：只管聚合、网页、网盘、WebDAV
   - 可以同时开机自启

## 最简理解

- **Samba = 局域网邻居共享**
- **Alist = 网盘+本地的网页总管**
- 同一块硬盘，**两个服务同时用，体验拉满**

## 相关概念

- [[samba-file-sharing]] — Samba 文件共享详解
- [[linux-disk-reserved-blocks]] — Linux 磁盘 root 保留块
