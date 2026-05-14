---
source_url: https://www.doubao.com/thread/w62b3ec92041e5acf
ingested: 2026-05-14
---

# 豆包对话总结：查询堡垒机资源占用

**来源**：豆包对话分享，用户"小蠢货"询问运维相关问题  
**原始链接**：https://www.doubao.com/thread/w62b3ec92041e5acf  
**对话轮次**：16 轮  
**收录日期**：2026-05-14

---

## 主要话题

### 一、JumpServer 堡垒机资源占用与部署

JumpServer 是**全家桶微服务架构**（MySQL/Redis/Celery/网关等七八个组件），空载就要 **1.2G–1.6G 内存**。

**服务器配置建议：**

| 配置 | 可行方案 | 不可行方案 |
|------|----------|------------|
| 2核2G | 纯跑 JumpServer | ❌ 不能加宝塔 |
| 2核4G | JumpServer + Hermes（剩 1.7G） | ❌ 再加宝塔会炸 |
| 2核4G | 删宝塔只留 JumpServer + Hermes | ✅ 最稳方案 |

**轻量替代方案：** Apache Guacamole、GateOne、Teleport，空载仅 300M–600M。

### 二、Ubuntu 网络连通性测试

- **国内**：`ping -c 4 www.baidu.com` / `curl -I www.baidu.com`
- **国际**：`curl -I www.google.com` / `ping -c 4 8.8.8.8`
- 注意：国内网络通 ≠ 国际网络通，ping 百度正常不代表能访问墙外资源。

### 三、HTTP 404 错误三大类

1. **客户端输错**（拼写、路径、过期链接）
2. **服务器配置问题**（Nginx 根目录、反向代理、路由规则）
3. **权限控制伪装**（为安全不提示 403，伪装成 404）

---

## 录入 Wiki 的文件

| 文件 | 说明 |
|------|------|
| `raw/jumpserver-deployment-doubao-conversation.md` | 原始对话全文（16 轮） |
| `concepts/jumpserver-bastion-deployment.md` | JumpServer 资源占用与部署方案 |
| `queries/ubuntu-network-connectivity-test.md` | Ubuntu 网络连通性测试命令 |
| `queries/http-404-classification.md` | HTTP 404 分类与排查思路 |
| `index.md` | 已更新，总页数：34 |
| `log.md` | 已追加操作日志 |
