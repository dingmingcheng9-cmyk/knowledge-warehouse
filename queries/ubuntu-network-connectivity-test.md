---
title: Ubuntu 外网/国际网络连通性测试命令
created: 2026-05-14
updated: 2026-05-14
type: query
tags: [tool, devops, note]
sources: [raw/jumpserver-deployment-doubao-conversation.md]
confidence: high
---

# Ubuntu 外网/国际网络连通性测试命令

服务器运维场景下，快速判断一台 Ubuntu 机器的外网连通性。

## 测试国内网络

```bash
# 最通用
ping -c 4 www.baidu.com

# 测试 DNS 解析 + HTTP 连通
curl -I www.baidu.com

# 纯网络层（不涉及 DNS）
ping -c 4 223.5.5.5
```

### 判断口诀
| 结果 | 结论 |
|------|------|
| ping 百度通 | 外网正常 |
| ping 百度不通，ping 223.5.5.5 通 | DNS 坏了 |
| 两个都不通 | 完全断网 |

## 测试国际网络（墙外）

```bash
# 最直接：测试 Google 连通性
curl -I www.google.com

# 备用：测试 Google DNS
ping -c 4 8.8.8.8
```

### 判断标准
- 返回 `HTTP/1.1 200 OK` → ✅ 能访问国际网络
- 超时/connection refused → ❌ 仅国内网

> ⚠️ 国内网络正常 ≠ 国际网络正常。ping 百度和 ping 223.5.5.5 都通，只证明国内没问题，**完全不能代表**能访问墙外资源。

## 相关页面

- [[cloudflare-tunnel]] — 内网穿透方案，需要服务端有国际网络连通
- [[docker-container-basics]] — 容器内网络连通性测试同理
