---
title: Windows 与 Linux 常见命令速查对比
created: 2026-06-21
updated: 2026-06-21
type: concept
tags: [tool, devops, note, comparison]
sources: [raw/articles/deepseek-windows-linux-commands.md]
---

# Windows 与 Linux 常见命令速查对比

> Windows CMD 和 Linux Shell 命令的按功能分类速查手册，含快速对比表。适合从 Windows 转到 Linux 的用户或日常双系统开发者参考。

## Windows 常见命令

Windows 的命令行环境主要有两种：**CMD（命令提示符）** 和 **PowerShell**（功能更强大，支持对象化操作）。

### 一、文件与目录操作

| 命令 | 功能说明 |
|------|---------|
| `dir` | 查看当前目录下的文件和文件夹列表 |
| `cd` | 切换当前目录，`cd ..` 返回上级，`cd /d D:` 跨盘符切换 |
| `md` / `mkdir` | 创建新目录 |
| `rd` / `rmdir` | 删除目录（默认只删空目录，`/s` 递归删除） |
| `copy` | 复制文件 |
| `xcopy` | 高级复制（可复制目录结构），推荐用 `Robocopy` 替代 |
| `move` | 移动文件或重命名 |
| `del` | 删除文件（危险操作，慎用） |
| `rename` / `ren` | 重命名文件或文件夹 |
| `type` | 查看文本文件内容 |
| `echo` | 打印文本或写入文件 |
| `cls` | 清屏 |

### 二、网络诊断命令

| 命令 | 功能说明 |
|------|---------|
| `ipconfig` | 查看IP地址、网关、DNS等信息 |
| `ipconfig /all` | 查看完整网卡详细信息 |
| `ping` | 测试网络连通性 |
| `tracert` | 路由追踪 |
| `nslookup` | DNS解析查询 |
| `netstat -ano` | 查看端口占用及对应进程PID |
| `arp -a` | 查看ARP缓存 |
| `route print` | 查看本地路由表 |
| `netsh` | 网络配置工具，可配置防火墙、网络接口等 |

### 三、系统管理命令

| 命令 | 功能说明 |
|------|---------|
| `tasklist` | 查看当前运行的所有进程 |
| `taskkill /pid xxx /f` | 强制终止指定进程 |
| `systeminfo` | 查看系统详细信息（版本、内存等） |
| `taskmgr` | 打开任务管理器 |
| `services.msc` | 打开服务管理 |
| `eventvwr` | 查看系统日志 |
| `regedit` | 注册表编辑器（⚠️高危操作） |
| `msconfig` | 系统启动项和启动服务管理 |
| `shutdown /s /t 0` | 立即关机 |
| `shutdown /r /t 0` | 立即重启 |

### 四、磁盘管理命令

| 命令 | 功能说明 |
|------|---------|
| `chkdsk` | 检查并修复磁盘错误 |
| `sfc /scannow` | 系统文件检查，修复损坏的系统文件 |
| `diskpart` | 磁盘分区管理工具 |
| `format` | 格式化磁盘 |
| `vol` | 显示磁盘卷标 |

### 五、文件搜索与权限

| 命令 | 功能说明 |
|------|---------|
| `find` | 在文件中查找包含特定字符串的行 |
| `findstr` | 高级文件内容搜索，支持正则表达式 |
| `attrib` | 查看/修改文件属性 |
| `takeown` | 取得文件所有权 |
| `icacls` | 修改文件访问权限 |

---

## Linux 常见命令

### 一、文件与目录管理

| 命令 | 功能说明 |
|------|---------|
| `ls` | 列出目录内容（`ls -la` 最常用） |
| `cd` | 切换当前工作目录 |
| `pwd` | 显示当前所在路径 |
| `mkdir` | 创建新目录（`mkdir -p` 递归创建） |
| `rm` | 删除文件或目录（`rm -rf` ⚠️危险） |
| `rmdir` | 删除空目录 |
| `cp` | 复制文件或目录（`cp -r` 递归复制） |
| `mv` | 移动或重命名文件 |
| `touch` | 创建空文件或改变文件时间戳 |
| `ln` | 创建硬链接与软链接 |
| `find` | 在目录中查找文件 |
| `file` | 显示文件类型 |

### 二、文本查看与处理

| 命令 | 功能说明 |
|------|---------|
| `cat` | 查看文件内容（合并输出） |
| `more` / `less` | 分页查看文件内容 |
| `head` / `tail` | 查看文件开头/结尾内容 |
| `grep` | 在文件中搜索指定字符串 |
| `sed` | 流编辑器，用于文本替换和处理 |
| `awk` | 模式扫描和文本处理语言 |
| `wc` | 统计文件的行数、单词数、字符数 |
| `diff` | 比较两个文件的不同 |
| `cut` | 从文本中提取一段并输出 |
| `uniq` | 去除重复行 |
| `tr` | 替换或删除字符 |
| `vim` / `vi` | 纯文本编辑器 |

### 三、系统管理与监控

| 命令 | 功能说明 |
|------|---------|
| `ps` | 显示当前运行的进程（`ps aux` 最常用） |
| `top` | 实时显示系统资源使用情况 |
| `kill` | 终止指定进程 |
| `df` | 查看磁盘空间使用情况（`df -h`） |
| `free` | 查看内存使用情况（`free -h`） |
| `uptime` | 显示系统运行时间及负载 |
| `lsof` | 查看进程打开的文件 |
| `history` | 显示命令历史记录 |

### 四、网络管理

| 命令 | 功能说明 |
|------|---------|
| `ping` | 测试网络连接 |
| `ifconfig` | 配置和显示网络接口信息（旧式，`ip addr` 替代） |
| `netstat` | 显示网络连接、路由表等信息 |
| `ssh` | 远程登录到其他主机 |
| `scp` | 在本地和远程之间安全传输文件 |
| `curl` | 文件传输工具 |

### 五、权限与用户管理

| 命令 | 功能说明 |
|------|---------|
| `chmod` | 修改文件或目录的权限 |
| `chown` | 改变文件或目录的所有者 |
| `chgrp` | 更改文件用户组 |
| `useradd` / `userdel` | 添加/删除用户 |
| `passwd` | 更改用户密码 |
| `su` | 切换用户身份 |
| `sudo` | 以超级用户权限执行命令 |

### 六、压缩与备份

| 命令 | 功能说明 |
|------|---------|
| `tar` | 打包和解包文件 |
| `gzip` / `gunzip` | 压缩/解压缩文件 |
| `zip` / `unzip` | 创建/解压ZIP文件 |

### 七、软件包管理

| 命令 | 功能说明 |
|------|---------|
| `apt` / `apt-get` | Debian/Ubuntu 包管理工具 |
| `yum` | CentOS/Fedora 包管理工具 |
| `rpm` | RPM包管理工具 |

### 八、帮助命令

| 命令 | 功能说明 |
|------|---------|
| `man` | 查看命令手册页 |
| `info` | 查看程序的信息页 |
| `whatis` | 显示一行命令的简要描述 |

---

## 快速对比：Windows vs Linux

| 功能 | Windows (CMD) | Linux |
|------|--------------|-------|
| 列出目录 | `dir` | `ls` |
| 切换目录 | `cd` | `cd` |
| 复制文件 | `copy` | `cp` |
| 移动/重命名 | `move` | `mv` |
| 删除文件 | `del` | `rm` |
| 创建目录 | `md` / `mkdir` | `mkdir` |
| 清屏 | `cls` | `clear` |
| 查看IP | `ipconfig` | `ifconfig` / `ip addr` |
| 网络测试 | `ping` | `ping` |
| 路由追踪 | `tracert` | `traceroute` |
| 查看进程 | `tasklist` | `ps` |
| 终止进程 | `taskkill` | `kill` |
| 查看帮助 | `命令 /?` | `man 命令` |

> 💡 在 Windows 中查看命令帮助可用 `命令 /?`；在 Linux 中可用 `man 命令` 或 `命令 --help`。

## 关联页面

- [[ip-address-query]] — IP 地址查询方法（Windows/Linux 都有涉及）
- [[wsl2-commands-cheatsheet]] — WSL2 指令速查手册
- [[network-protocols-basics]] — 网络协议基础
- [[shell-source-command]] — Shell source 命令详解
