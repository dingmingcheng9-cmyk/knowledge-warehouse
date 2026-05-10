---
title: Linux 磁盘 root 保留块（5% reserved blocks）
created: 2026-05-10
updated: 2026-05-10
type: concept
tags: [tool, devops, linux]
sources: [raw/articles/doubao-disk-reserve-samba-conversation.md]
confidence: high
---

# Linux 磁盘 root 保留块

Linux 文件系统（ext4/xfs 等）默认给 **root 管理员预留 5%** 的磁盘空间，防止普通用户把磁盘塞满导致系统崩溃。

## 核心机制

```
总磁盘容量 = 用户可用空间 + root 保留空间（5%）
```

例：磁盘总容量 916 GiB，root 保留 46 GiB（5%），用户可用 870 GiB。

### 保留块的作用
- **防崩溃**：普通用户把磁盘写满后，系统仍能正常运行，root 可以登录清理
- **防碎片化**：保留块给文件系统碎片整理留出移动空间（传统文件系统更敏感）
- **系统服务保护**：关键系统服务在磁盘满时仍能写入日志

### 查看方式
```bash
# 查看保留块比例（默认 5%）
tune2fs -l /dev/sdX | grep "Reserved block count"
# 或者
dumpe2fs -h /dev/sdX | grep "Reserved"

# 查看保留块占总容量的百分比
df -h /  # 显示的总量 = 用户可用（不含保留块）
```

## 修改保留比例

NAS/数据盘一般没必要留 5%，可以调小：

```bash
# 改为 1%（需要 root）
tune2fs -m 1 /dev/sdX
# -m 1 表示 1%
```

> ⚠️ **根分区（/）建议保留至少 1-2%**，数据分区可以设为 0%。

## 与 [[samba-file-sharing]] 的关系

- Samba 共享出去的空间 = 用户可用空间（扣除了 root 保留块）
- Windows 客户端看不到 root 保留的 46 GiB
- 两者是不同层级的概念：保留块是本地文件系统机制，Samba 是在这之上做的共享服务

## 相关概念
- [[samba-file-sharing]] — Samba 文件共享
- [[linux-proc-filesystem]] — Linux /proc 文件系统
