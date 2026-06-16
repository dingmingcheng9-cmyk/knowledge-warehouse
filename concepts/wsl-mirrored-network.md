---
tags:
  - wsl
  - windows
  - networking
  - mirrored-network
  - Plan9
---

# WSL 镜像网络模式

## 概述

WSL2 从较新版本开始支持 **Mirrored Network（镜像网络）模式**，取代了传统的 NAT 网络模式。在此模式下，WSL 与 Windows 宿主机共享同一张物理网卡、同一个 IP 地址和 DNS 配置，大幅简化网络管理。

## 环境

| 项目 | 值 |
|------|-----|
| Windows | 11 Build 26200+ (10.0.26200.8655) |
| WSL 版本 | 2.7.3.0 ~ 2.7.8.0 |
| 内核 | 6.6.114.1 ~ 6.18.33.1-1 |
| WSL 默认配置 | ✅ **默认已是镜像模式**，无需手动 `.wslconfig` 配置 |

## Mirrored 模式 vs 传统 NAT 模式

| 对比项 | 传统 NAT 模式 | Mirrored 镜像模式 |
|--------|--------------|-------------------|
| WSL IP | 独立内网 IP（172.x.x.x / 10.110.x.x） | **与宿主机共享 IP** |
| 虚拟交换机 | 有 vEthernet (WSL) 适配器 | **没有** |
| 默认路由 | 需经过 Windows NAT 转发 | 与 Windows 相同 |
| DNS | 独立配置 | 自动继承 Windows DNS |
| 端口空间 | **独立**，不会冲突 | **共享**，需避免端口冲突 |
| 宿主机访问 WSL 服务 | 需 `netsh portproxy` 端口转发 | 直接用 localhost 访问 |
| 局域网访问 WSL 服务 | 需额外端口映射配置 | 直接用宿主机 IP:端口 访问 |
| 换网络（WiFi/网线/VPN 切换） | 可能断连需重启 WSL | 自动跟随 Windows |

## 实际网络拓扑

```
局域网 (192.168.31.0/24)
├── 路由器 Xiaomi_F355_0CAA (.1)
├── Windows 主机 (.79) ─── 以太网卡
│   └── WSL2 Ubuntu ─── 共享 .79 IP (enP48276p0s0)
│       ├── DNS: 8.8.8.8 / 1.1.1.1
│       └── 网关: 192.168.31.1
└── NAS (192.168.31.187)
```

### 网络详情（实测）

| 项目 | 值 |
|------|-----|
| WSL IP | 192.168.31.79/24（与宿主机相同） |
| 网关 | 192.168.31.1 |
| 外网连通 | ✅ 41ms to 8.8.8.8，0% 丢包 |
| 防火墙组 | 无独立 WSL 防火墙规则 |
| 网络类别 | Public（公用网络） |
| Docker | docker0 (172.17.0.0/16) 当前 linkdown |

### 硬件资源（WSL 可见）

| 项目 | 值 |
|------|-----|
| CPU | i5-13500H（WSL 可见 16 核） |
| 内存 | 15Gi 总量，已用 766Mi（约5%），可用 ~14Gi |
| Swap | 4.0Gi，使用 0B |
| 磁盘 | /dev/sdd 1007G，已用 9.2G（1%），可用 947G |

### WSL 配置

- **`.wslconfig`** ❌ 不存在，使用默认配置
- **`/etc/wsl.conf`**：`systemd=true`，默认用户 `dingm`
- DNS 自动生成：8.8.8.8 / 1.1.1.1

## Mirrored 模式的实际好处

1. **局域网直接访问**：WSL 中启动的服务（如 `python -m http.server`），局域网其他设备直接用 `宿主机IP:端口` 即可访问
2. **代理无缝共享**：Windows 上的代理（Clash/v2ray），WSL 里走 `localhost:端口` 就能用
3. **NAS 直连**：局域网设备从 WSL 网段直连，不绕 NAT
4. **自动跟随网络变化**：换 WiFi / 插网线 / VPN 切换，WSL 自动跟随，无需手动处理
5. **无需端口转发**：相比 NAT 模式省去了 `netsh portproxy` 的繁琐配置

## 注意事项

- ⚠️ **端口冲突**：镜像模式下 WSL 与 Windows 共享端口空间，两边别用同一个端口即可
- ✅ **无需手动开启**：WSL 2.7.3+ 默认已是镜像模式，无需在 `.wslconfig` 中配置

## 故障记录（2026-05-13）

- **问题**：WSL 启动后无默认路由，无法访问外网 API
- **表象**：`ping 8.8.8.8` → `Network is unreachable`
- **修复**：直接 `wsl --shutdown` 后重启即可
- **误操作教训**：尝试加 `.wslconfig` 配置镜像模式反而导致网卡消失
- **结论**：WSL 2.7.3 默认已是镜像模式，无需额外配置

## WSL 文件系统访问（Plan 9）

### `\\wsl.localhost\Ubuntu`

WSL2 Ubuntu 的文件系统在 Windows 侧的映射入口，通过 **Plan 9 文件共享协议** 暴露：

```
\\wsl.localhost\Ubuntu
  ├── home/
  │   └── dingm/     ← WSL 用户目录
  ├── etc/
  ├── usr/
  └── mnt/
      └── c/         ← 从 WSL 看 Windows C: 盘
```

### 从 Windows 侧访问 WSL 文件的三种方式

| 方式 | 命令/代码 | 适用场景 |
|------|----------|---------|
| Python 直读 | `open(r"\\wsl.localhost\Ubuntu\...")` | 读文件，最快 |
| wsl.exe 远程执行 | `wsl.exe -d Ubuntu -- <命令>` | 在 WSL 内部跑命令 |
| git-bash 绕 cmd | `cmd.exe /c "wsl -l -v"` | Terminal 工具内使用 |

### 跨文件系统写操作风险

| 操作 | 风险 | 说明 |
|------|------|------|
| 从 Windows 创建新文件写 → WSL | 🔴 高风险 | 权限/属主可能错乱 |
| 从 Windows 修改已有文件 | 🟡 低风险 | Plan 9 驱动会尽量继承原文件属性 |
| **最佳实践** | ✅ | 修改/创建走 `wsl.exe` 钻进 WSL 内部执行 |

## WSL 状态管理

| 操作 | 命令/方法 |
|------|----------|
| 查看状态 | `wsl -l -v`（查看 STATE 列） |
| 精简状态 | `wsl --status` |
| 资源管理器检查 | 地址栏输 `\\wsl.localhost\`，能打开=运行中 |
| 任务管理器确认 | 查看 Vmmem / VmmemWSL 进程是否存在 |
| 启动 WSL | `wsl -d Ubuntu` |
| 完全关闭 | `wsl --shutdown` |

## 相关链接

- [WSL 官方文档 - 镜像网络模式](https://learn.microsoft.com/en-us/windows/wsl/networking)
- [network-protocols-basics](network-protocols-basics.md) — 基础网络协议知识
- [ssh-tunnel](ssh-tunnel.md) — SSH 隧道
