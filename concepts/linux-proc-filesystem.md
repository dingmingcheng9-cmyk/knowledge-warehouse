---
title: Linux /proc 虚拟文件系统
created: 2026-05-07
updated: 2026-05-07
type: concept
tags: [tool, devops, tutorial]
sources: [raw/articles/proc-orm-doubao-conversation.md]
confidence: high
---

# Linux /proc 虚拟文件系统

> `/proc`（procfs）是 Linux 内核提供的**虚拟/伪文件系统**，全程在内存中，不占磁盘空间，用于实时暴露内核与进程状态。

## 核心用途

### 1. 查看进程信息

每个运行中的进程在 `/proc/{PID}/` 下有一个目录：

```bash
ls /proc/$$/fd          # 查看当前进程打开的文件句柄
cat /proc/1234/cmdline  # 查看 PID 1234 的启动命令
cat /proc/1234/maps     # 查看进程的内存映射
```

### 2. 查看硬件信息

```bash
cat /proc/cpuinfo   # CPU 详细信息（型号、核心数、频率）
cat /proc/meminfo   # 内存使用情况（总量、已用、可用）
cat /proc/diskstats # 磁盘 I/O 统计
```

### 3. 查看内核参数

```bash
cat /proc/sys/net/ipv4/ip_forward     # IP 转发是否开启（0=关，1=开）
echo 1 > /proc/sys/net/ipv4/ip_forward # 动态开启 IP 转发（临时生效）
```

> `/proc/sys/` 下的参数可动态修改，**重启后失效**。要永久生效需写入 `/etc/sysctl.conf`。

### 4. 查看系统状态

```bash
cat /proc/loadavg   # 系统负载（1/5/15 分钟平均值）
cat /proc/uptime    # 系统已运行时间
cat /proc/net/      # 网络状态（接口、连接、路由）
```

## 工作原理

- **完全在内存中**：不占用磁盘空间，由内核动态生成
- **实时反映状态**：读取时即时生成数据，始终反映当前系统状态
- **虚拟文件系统**：这些"文件"不是磁盘上的真实文件，而是内核数据结构的接口

## 相关页面

- [[docker-container-basics]] — Docker 容器运行时原理
