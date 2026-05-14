---
title: IP 地址查询方法（Windows/Linux）
created: 2026-05-14
updated: 2026-05-14
type: query
tags: [tool, devops, note]
sources: [raw/knowledge-learning-notes.md]
confidence: high
---

# IP 地址查询方法（Windows/Linux）

## 内网 IP

| 系统 | 命令 | 说明 |
|------|------|------|
| Windows | `ipconfig` | 查看本机所有网卡 IP |
| Linux | `ip a` | 查看内网网卡 IP |
| Linux | `ifconfig` | 查看网卡信息（需安装 net-tools） |

## 外网/公网 IP

各系统通用，用外部服务查询：

```bash
curl ifconfig.me
```

也可访问 [ifconfig.me](https://ifconfig.me) 浏览器查看。

## 常用场景

```bash
# Ubuntu 查看 IP 后用于 SSH 连接
ip a
# 输出示例：inet 192.168.31.187/24

# Windows 查看 IP 后用于局域网访问
ipconfig
# 输出示例：IPv4 地址 . . . . . . . : 192.168.1.100
```

## 相关页面

- [[ubuntu-ssh-remote-setup]] — SSH 远程连接需要知道目标 IP
- [[cloudflare-tunnel]] — 无公网 IP 时的替代方案
