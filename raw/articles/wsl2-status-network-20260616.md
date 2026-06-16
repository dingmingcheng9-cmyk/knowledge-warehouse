---
source: "用户发送文档合并"
date: 2026-06-16
files:
  - "WSL2状态与网络分析_20260616.md"
---

# WSL2 状态与网络分析

> 日期：2026-06-16
> 环境：Windows 11 家庭版 (10.0.26200) / WSL 2.7.8.0 / Ubuntu

---

## 一、WSL2 基本信息

| 项目 | 值 |
|------|-----|
| 发行版 | Ubuntu（默认） |
| 状态 | Running |
| WSL 版本 | 2.7.8.0 |
| WSLg | 1.0.73.2 |
| 内核 | 6.18.33.1-1 |
| Windows 版本 | 10.0.26200.8655 |
| 负载 | 刚启动3分钟，负载极低 (0.07) |

### 硬件资源

| 项目 | 值 |
|------|-----|
| CPU | i5-13500H (WSL 可见 16 核) |
| 内存 | 15Gi 总量，已用 766Mi（约5%），可用 ~14Gi |
| Swap | 4.0Gi，使用 0B |
| 磁盘 | /dev/sdd 1007G，已用 9.2G（1%），可用 947G |

### 配置

- **`.wslconfig`** ❌ 不存在，全部使用 WSL 默认配置
- **`/etc/wsl.conf`**：已启用 `systemd=true`，默认用户 `dingm`
- DNS 自动生成，走 8.8.8.8 / 1.1.1.1

---

## 二、网络架构（核心发现：Mirrored 镜像模式）

### 与传统 NAT 模式对比

| 对比项 | 传统 NAT 模式（旧） | 本机 Mirrored 模式 |
|--------|--------------------|--------------------|
| WSL IP | 独立内网 IP (172.x.x.x) | **与宿主机共享 IP** |
| 虚拟交换机 | 有 vEthernet (WSL) 适配器 | **没有** |
| 端口映射 | 需要 `netsh portproxy` 配置 | **不需要**，直接通 |
| 宿主机访问 WSL | 需 localhost 转发 | 直接用 localhost |

### 实际网络拓朴

```
局域网 (192.168.31.0/24)
├── 路由器 Xiaomi_F355_0CAA (.1)
├── Windows 主机 (.79) ─── 以太网卡
│   └── WSL2 Ubuntu ─── 共享 .79 IP (enP48276p0s0)
│       ├── DNS: 8.8.8.8 / 1.1.1.1
│       └── 网关: 192.168.31.1
└── NAS (192.168.31.187)
```

### 网络详情

| 项目 | 值 |
|------|-----|
| WSL IP | 192.168.31.79/24（与宿主机相同） |
| 网关 | 192.168.31.1 |
| 外网连通 | ✅ 41ms to 8.8.8.8，0% 丢包 |
| 防火墙组 | 无独立 WSL 防火墙规则 |
| 网络类别 | Public（公用网络） |
| Docker | docker0 (172.17.0.0/16) 当前 linkdown |

### Mirrored 模式带来的实际好处

- WSL 里启动的服务（如 `python -m http.server`），局域网其他设备直接用 `192.168.31.79:端口` 访问
- Windows 上的代理（clash/v2ray），WSL 里走 `localhost:端口` 就能用
- NAS (192.168.31.187) 从 WSL 网段直连，不绕 NAT

---

## 三、WSL 文件系统访问方式

### `\\wsl.localhost\Ubuntu` 是什么

这是 WSL2 Ubuntu 的文件系统在 Windows 侧的映射入口，通过 **Plan 9 文件共享协议** 暴露。

```
\\wsl.localhost\Ubuntu
  ├── home/
  │   └── dingm/     ← WSL 用户目录
  ├── etc/
  ├── usr/
  └── mnt/
      └── c/         ← 从 WSL 看 Windows C: 盘
```

### 三种访问方式（从 Windows 侧）

| 方式 | 命令/代码 | 适用场景 |
|------|----------|---------|
| Python 直读 | `open(r"\\wsl.localhost\Ubuntu\...")` | 读文件，最快 |
| wsl.exe 远程执行 | `wsl.exe -d Ubuntu -- <命令>` | 在 WSL 内部跑命令 |
| git-bash 绕 cmd | `cmd.exe /c "wsl -l -v"` | Terminal 工具内使用 |

### 跨文件系统写操作风险

- **创建新文件**（从 Windows 写 → WSL）：🔴 有风险，权限/属主可能错乱
- **改已有文件**：🟡 低风险，Plan 9 驱动会尽量继承原文件属性
- **最佳实践**：修改/创建走 `wsl.exe` 钻进 WSL 内部执行，避免跨文件系统问题

---

## 四、WSL 状态查看方法

| 方式 | 命令/操作 |
|------|----------|
| 命令行 | `wsl -l -v` 查看 STATE 列 |
| 命令行精简 | `wsl --status` |
| 资源管理器 | 地址栏输 `\\wsl.localhost\`，能打开=运行中 |
| 任务管理器 | 查看 Vmmem / VmmemWSL 进程是否存在 |
| 启动 WSL | `wsl -d Ubuntu` |
| 完全关闭 | `wsl --shutdown` |

---

## 五、杂项

- 用户曾考虑安装 Ubuntu Desktop 系统，已评估 4 种方案（VirtualBox / VMware / WSL 桌面 / 双系统），暂不执行
- 主机硬件：Lenovo 小新 83AQ, i5-13500H, 32GB DDR5, RTX 5060 Ti 16GB
- 主机另有 VMware VMnet1/VMnet8 虚拟网卡残留（VMware 已装过）
