---
title: Ubuntu SSH 远程连接配置
created: 2026-05-14
updated: 2026-05-14
type: concept
tags: [tool, devops, note]
sources: [raw/knowledge-learning-notes.md]
confidence: high
---

# Ubuntu SSH 远程连接配置

从 Windows/Mac 远程连接 Ubuntu 服务器的完整设置流程。

## 服务端配置（Ubuntu）

```bash
# 1. 安装 SSH 服务
sudo apt update
sudo apt install openssh-server -y

# 2. 启动并设置开机自启
sudo systemctl start ssh
sudo systemctl enable ssh

# 3. 检查运行状态
sudo systemctl status ssh

# 4. 放行防火墙（UFW）
sudo ufw allow 22/tcp

# 5. 查看 Ubuntu IP（后面连接要用）
ip a
```

## 客户端连接（Windows）

```bash
# Win + R → 输入 cmd 或 wt（Windows Terminal）
ssh root@192.168.31.187
```

## 验证步骤

| 步骤 | 命令 | 预期结果 |
|------|------|----------|
| 检查服务状态 | `sudo systemctl status ssh` | 显示 active (running) |
| 检查监听端口 | `ss -tlnp \| grep :22` | 显示 LISTEN |
| 防火墙放行 | `sudo ufw status` | 22/tcp ALLOW |
| 从外部连接 | `ssh user@IP` | 登录成功 |

## 相关页面

- [[ip-address-query]] — IP 地址查询方法
- [[cloudflare-tunnel]] — 无需公网 IP 的内网穿透替代方案
- [[docker-container-basics]] — 容器内 SSH 配置不同
