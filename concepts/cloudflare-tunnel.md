---
title: Cloudflare Tunnel 内网穿透
created: 2026-05-06
updated: 2026-05-06
type: concept
tags: [tool, devops, product]
sources: [raw/articles/cloudflare-tunnel-domains-doubao-conversation.md]
confidence: high
---

# Cloudflare Tunnel 内网穿透

## 定义

**Cloudflare Tunnel（原名 Argo Tunnel）** = 免费、安全、不用公网IP的内网穿透工具。本地服务不用在路由器开端口、不用公网IP，就能安全暴露到公网，**永久免费、不限流量、自动HTTPS**。

## 工作原理

- 本地安装轻量客户端 `cloudflared`
- 由**内向外**主动连到 Cloudflare 全球节点（加密隧道）
- 外网访问域名 → 走到 Cloudflare → 隧道转发到你本地服务器
- **安全、免费、不用改防火墙**

## 核心特点

- ✅ **完全免费**、不限流量、不限带宽
- ✅ **不用公网IP**、不用端口转发、不用暴露真实IP
- ✅ **自动HTTPS**（免费SSL证书）
- ✅ 自带 **DDoS防护、WAF**
- ✅ 支持 **HTTP/HTTPS/TCP/SSH** 等
- ⚠️ 国内访问：部分地区延迟偏高，适合开发/测试/个人用

## 为什么免费？（商业逻辑）

- Cloudflare 靠**企业付费**赚钱（Pro $20/月起），免费版是获客引流
- 个人隧道成本极低，边际成本几乎为零
- 核心收入是企业安全、CDN、Zero Trust
- 官方明确：**Tunnel for personal use is free forever**

## 免费版限制

- 最多 **5条并发隧道**
- 国内部分运营商（移动）延迟偏高、偶尔不稳
- 必须用 **Cloudflare 托管的域名**
- 不支持 **UDP 转发**

## 适用场景：Hermes Agent

Hermes 被飞书/微信/元宝平台回调，必须有公网可访问地址。用 Tunnel：

1. 本地 Hermes 跑在 `127.0.0.1:10086`
2. 启动隧道：把公网域名指向本地端口
3. 平台填回调地址 → 流量进隧道 → 到本地 Hermes

## 快速上手

```bash
# 1. 安装客户端
wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64
chmod +x cloudflared-linux-amd64
mv cloudflared-linux-amd64 /usr/local/bin/cloudflared

# 2. 临时隧道（暴露本地10086）
cloudflared tunnel --url http://localhost:10086

# 3. 按提示登录 Cloudflare，绑定域名
```

## 传统方案对比

| 方案 | 公网IP | 端口映射 | 费用 | 安全 |
|------|--------|----------|------|------|
| 传统端口映射 | 需要 | 需要 | 高（企业专线） | 暴露真实IP |
| Cloudflare Tunnel | 不需要 | 不需要 | 免费 | 隐藏真实IP |
| IPv6 + DDNS | 需要IPv6 | 需要 | 免费 | 需配置 |

## 相关概念

- [[domain-name-basics]]（域名管理、DNS 解析）
- 反向代理
