---
source: "用户发送文档合并"
date: 2026-06-16
files:
  - "WSL镜像网络模式笔记.md"
  - "WSL2状态与网络分析_20260616.md"
---

# WSL 镜像网络模式笔记

> 原文件：WSL镜像网络模式笔记.md

## 环境
- Windows 11 Build 26200.8246
- WSL 2.7.3.0
- WSL2 Ubuntu 26.04 (Kernel 6.6.114.1)
- WSL 默认已是镜像网络模式，**不需要**在 `.wslconfig` 中手动开启

## 镜像模式 vs NAT 模式

| 特性 | NAT 模式 | 镜像模式 |
|------|---------|---------|
| IP | WSL 有独立虚拟 IP（如 10.110.x.x） | 共享 Windows 主机 IP |
| 默认路由 | 需经过 Windows NAT 转发 | 与 Windows 相同 |
| DNS | 独立配置 | 与 Windows 相同（自动） |
| 端口空间 | **独立**，不会冲突 | **共享**，可能冲突 |
| 访问 WSL 服务 | 需端口转发 | 可直接 localhost 访问 |
| 换网络 | WSL 可能断连需重启 | 自动跟随 Windows |

## 核心要点
1. 镜像模式下 WSL 和 Windows 共享同一张物理网卡、同一个 IP、同一个 DNS
2. 虽然 IP 相同，但端口空间可能冲突——两边别用同一个端口即可
3. 换 WiFi / 插网线 / VPN 切换，WSL 自动跟随，无需手动处理
4. 相比 NAT 模式少了很多路由/DNS问题，使用更省心

## 2026-05-13 故障记录
- 问题：WSL 启动后无默认路由，无法访问外网 API
- 表象：`ping 8.8.8.8` → `Network is unreachable`
- 修复方式：直接 `wsl --shutdown` 后重启即可
- 误操作：尝试加 `.wslconfig` 配置镜像模式反而导致网卡消失
- 教训：WSL 2.7.3 默认已是镜像模式，无需额外配置
