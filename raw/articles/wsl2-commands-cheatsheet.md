---
source: "用户发送文档"
date: 2026-06-16
---

# Windows + WSL2 指令速查手册

> 适用环境：Windows 11 + WSL 2.7+
> 适用发行版：Ubuntu（WSL2 模式）
> 最后更新：2026-06-16

---

## 一、基础管理命令

### 状态查看

```cmd
wsl -l -v                  # 列出所有发行版及运行状态
wsl --status               # WSL 总体状态
wsl --version              # WSL 引擎版本（内核、WSLg、Windows）
```

输出示例：
```
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

### 启动与进入

```cmd
wsl                        # 进入默认发行版（当前目录为 Windows 工作目录）
wsl -d Ubuntu              # 进入指定发行版
wsl ~                      # 进入后直接到 Linux 家目录
wsl --cd /home/dingm       # 指定进入后的起始目录
```

### 以特定用户登录

```cmd
wsl -u root                # 以 root 进入
wsl -u dingm               # 以指定用户进入
```

### 直接在 WSL 内执行命令（不进交互 Shell）

```cmd
wsl -- ls -la              # 执行单条命令
wsl -d Ubuntu -- cat /etc/os-release  # 指定发行版执行
wsl -e bash -c "echo hello && pwd"    # 多命令
```

### 关闭 WSL

```cmd
wsl --shutdown             # 完全关闭所有发行版（释放内存）
wsl --terminate Ubuntu     # 仅关闭指定发行版
```

> **注意**：`--shutdown` 会杀掉 Vmmem 进程，彻底释放 WSL2 占用的内存。
> `--terminate` 只关指定发行版，其他发行版不受影响。

---

## 二、发行版管理

### 安装

```cmd
wsl --install                        # 安装 WSL + 默认 Ubuntu
wsl --install -d Ubuntu              # 安装指定发行版
wsl --install -d Debian              # 安装 Debian
wsl --install -d kali-linux          # 安装 Kali Linux
wsl --list --online                  # 查看可在线安装的发行版列表
```

> Windows 11 首次安装：系统会自动启用 WSL 功能并下载 Ubuntu。

### 导入/导出（备份与迁移）

```cmd
# 导出为 tar 文件（备份）
wsl --export Ubuntu D:\backup\ubuntu_backup.tar
wsl --export Ubuntu D:\backup\ubuntu_backup.tar --vhd  # 导出为 VHD 格式

# 从 tar 文件恢复（导入）
wsl --import Ubuntu D:\wsl\ubuntu\ D:\backup\ubuntu_backup.tar
wsl --import Ubuntu D:\wsl\ubuntu\ D:\backup\ubuntu_backup.tar --version 2

# 移动发行版到新位置
wsl --move Ubuntu D:\wsl\ubuntu_new\
```

> **备份要点**：导出的 tar 文件包含完整的 Linux 文件系统。
> 导入时指定 `--version 2` 确保用 WSL2 模式。
> 导入后默认用户为 root，需用 `--set-default-user` 改回普通用户。

### 删除与重装

```cmd
wsl --unregister Ubuntu    # 注销/删除发行版（所有数据丢失！）
```

### 设置默认

```cmd
wsl --set-default Ubuntu                        # 设置默认发行版
wsl --set-default-version 2                     # 设置新装的发行版默认用 WSL2
wsl --set-default-user dingm                    # 设置默认登录用户
wsl --manage Ubuntu --set-sparse true           # 开启稀疏磁盘（自动回收未用空间）
```

---

## 三、WSL 配置（.wslconfig）

### 文件位置

```
C:\Users\<用户名>\.wslconfig
```

> 全局配置，影响所有 WSL2 发行版。修改后需执行 `wsl --shutdown` 再重启生效。

### 常用配置示例

```ini
[wsl2]
memory=8GB                    # 限制 WSL2 最大内存（默认：宿主机 50%）
processors=8                  # 限制 CPU 核心数（默认：所有核心）
swap=2GB                      # 限制 swap 大小（默认：宿主机 25%）
swapFile=D:\wsl\swap.vhdx     # swap 文件存放位置
localhostForwarding=true       # localhost 转发（默认 true，Mirrored 模式下自动）
```

### 查看当前配置是否生效

```cmd
wsl --status                  # 查看当前内存/swap等限制
```

### WSL 内部配置文件（/etc/wsl.conf）

```ini
# 在 WSL 内：/etc/wsl.conf
[boot]
systemd=true                  # 启用 systemd（WSL 2.7+ 默认开启）

[user]
default=dingm                 # 默认登录用户

[network]
generateResolvConf = false    # 关闭自动 DNS 生成（自定义 DNS 时需要）
hostname = my-wsl             # 自定义主机名
```

> 修改 `wsl.conf` 后执行 `wsl --terminate Ubuntu`（或 `wsl --shutdown`）再重进生效。

---

## 四、文件系统互操作

### Windows → WSL（访问 WSL 文件）

```
\\wsl.localhost\Ubuntu\       # 文件资源管理器直达 WSL 根目录
\\wsl.localhost\Ubuntu\home\dingm\  # WSL 家目录
```

> 支持在 Windows 应用（VS Code、Explorer）中直接打开/编辑。

### WSL → Windows（访问 Windows 文件）

```
/mnt/c/Users/dingm/           # WSL 里访问 Windows C 盘
/mnt/d/                       # WSL 里访问 D 盘
```

> 也可用 `/mnt/c/` 访问 Windows 全部盘符。

### wslpath 路径转换（WSL 内部用）

```bash
# Linux 路径 → Windows 路径
wslpath -w /home/dingm/file.txt
# 输出：\\wsl.localhost\Ubuntu\home\dingm\file.txt

# Windows 路径 → Linux 路径
wslpath -u "C:\Users\dingm"
# 输出：/mnt/c/Users/dingm
```

### VS Code 集成

```bash
# 在 WSL 内直接打开 VS Code
code .                        # 打开当前目录
code /home/dingm/myproject    # 打开指定项目

# 自动安装 "Remote - WSL" 扩展，体验接近原生 Linux 开发
```

### Windows 程序调用（WSL 内执行 Windows exe）

```bash
notepad.exe ~/file.txt        # 用 Windows 记事本打开 WSL 文件
explorer.exe .                # 用资源管理器打开当前目录
ipconfig.exe                  # 查看 Windows 网络配置
```

---

## 五、网络相关

### 查看 WSL 内部网络

```bash
# 在 WSL 内执行
ip addr                       # 查看 IP 地址
ip route                      # 查看路由表
ip neigh                      # 查看 ARP 表
cat /etc/resolv.conf          # 查看 DNS 配置
ping -c 4 8.8.8.8            # 测试外网连通性
```

### WSL 网络模式

WSL2 支持两种网络模式（2.7+ 默认 Mirrored）：

| 模式 | 特点 | 适用场景 |
|------|------|---------|
| **Mirrored**（镜像模式） | WSL 与宿主机共享 IP，端口直通，无需转发 | 开发、局域网服务暴露 |
| **NAT**（传统模式） | WSL 用独立内网 IP，需端口转发 | 旧版兼容 |

> 你的 WSL 2.7.8.0 默认使用 Mirrored 模式。

### Windows 侧端口转发（仅 NAT 模式需要）

```cmd
# 将 Windows 1212 端口转发到 WSL 的 8080
netsh interface portproxy add v4tov4 listenport=1212 connectport=8080 connectaddress=::1

# 查看转发规则
netsh interface portproxy show all

# 删除转发规则
netsh interface portproxy delete v4tov4 listenport=1212
```

> Mirrored 模式下**不需要**端口转发，WSL 服务直接暴露在宿主机 IP 上。

### DNS 配置

```bash
# WSL 默认自动生成 resolv.conf
# 若要自定义 DNS，在 /etc/wsl.conf 中添加：
# [network]
# generateResolvConf = false
# 然后手动编辑 /etc/resolv.conf：
sudo bash -c 'echo "nameserver 223.5.5.5" > /etc/resolv.conf'
sudo bash -c 'echo "nameserver 114.114.114.114" >> /etc/resolv.conf'
```

---

## 六、磁盘与存储

### 查看 WSL2 vhdx 文件位置

```
C:\Users\<用户名>\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu_79rhkp1fndgsc\LocalState\ext4.vhdx
```

> 这是 WSL2 的虚拟磁盘文件，可能非常大。

### 压缩/回收 vhdx 空间

```cmd
wsl --shutdown                          # 先关闭 WSL
diskpart                                # 启动 diskpart 工具
  select vdisk file="C:\Users\dingm\AppData\Local\Packages\..."\LocalState\ext4.vhdx
  attach vdisk readonly
  compact vdisk
  detach vdisk
  exit
```

### 磁盘空间查看

```bash
# WSL 内
df -h                     # 查看磁盘使用情况
du -sh ~/*                # 查看家目录各文件夹大小
du -sh /var/log           # 查看日志占用
```

---

## 七、故障排查

### WSL 无法启动 / 报错 0x800701bc / 0x80370102

```cmd
# 确保虚拟化已启用（BIOS 中开启 VT-x/AMD-V）
# 确保 WSL 功能已启用：
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
# 重启后：
wsl --set-default-version 2
```

### Vmmem 占用内存过高

```cmd
# 方案1：立即释放
wsl --shutdown

# 方案2：长期限制（创建 .wslconfig）
# memory=4GB  # 限制为 4GB

# 方案3：开启稀疏磁盘自动回收
wsl --manage Ubuntu --set-sparse true
```

### WSL 网络不通

```bash
# 排查步骤：
# 1) WSL 内 ping 局域网
ping 192.168.31.1

# 2) Ping 外网
ping 8.8.8.8

# 3) DNS 解析
nslookup baidu.com

# 4) 查看路由表
ip route

# 5) 检查 Windows 代理设置
# 如果 Windows 有代理（clash/v2ray），WSL 可能需要配置代理：
export http_proxy=http://localhost:7890
export https_proxy=http://localhost:7890
```

### 重置 WSL 网络栈

```cmd
wsl --shutdown
# 网络问题通常重启 WSL 即可解决
wsl -d Ubuntu
```

### WSL 卡死 / 无响应

```cmd
# 强制关闭
wsl --shutdown

# 如果关不掉，任务管理器杀 Vmmem 进程
# 或重启 Windows
```

---

## 八、实用技巧

### 从 WSL 打开 Windows 资源管理器

```bash
explorer.exe .              # 打开当前目录
explorer.exe /mnt/c/Users   # 打开 Windows 文件夹
```

### 从 WSL 用 Windows 浏览器打开 URL

```bash
powershell.exe Start-Process "https://google.com"
cmd.exe /c start https://google.com
```

### 设置 WSL 默认用户为 root

```cmd
wsl --set-default-user root     # 从 Windows 侧设置
# 或者在 WSL 内 /etc/wsl.conf 设置：
# [user]
# default=root
```

### 彻底重装 WSL 发行版

```cmd
# 先备份重要数据
wsl --export Ubuntu D:\backup\ubuntu_backup.tar

# 删除
wsl --unregister Ubuntu

# 从备份恢复
wsl --import Ubuntu D:\wsl\ubuntu\ D:\backup\ubuntu_backup.tar --version 2
wsl --set-default-user dingm
```

### 查看运行中的 WSL 进程占用

```bash
# WSL 内
ps aux --forest             # 进程树
top / htop                 # 实时资源监控
free -h                    # 内存使用
```

```cmd
# Windows 侧
tasklist /fi "IMAGENAME eq vmmem*"   # 查看 Vmmem 进程
```

### WSL + Docker 建议配置

```bash
# 为避免 Docker 耗尽内存，建议 .wslconfig 限制：
# memory=8GB
# 或者 Docker Desktop 设置中限制资源
```

---

## 九、常用简写速查

| 命令 | 含义 |
|------|------|
| `wsl -l -v` | 列出发行版（含状态） |
| `wsl -d Ubuntu` | 进入 Ubuntu |
| `wsl -u root` | 以 root 进入 |
| `wsl --shutdown` | 关闭所有 |
| `wsl --export Ubuntu file.tar` | 导出备份 |
| `wsl --import Ubuntu dir file.tar` | 导入恢复 |
| `wsl --unregister Ubuntu` | 删除发行版 |
| `wsl --set-default-version 2` | 新装默认 WSL2 |

---

> **参考链接**
> - [WSL 官方文档](https://learn.microsoft.com/zh-cn/windows/wsl/)
> - [WSL 命令参考](https://learn.microsoft.com/zh-cn/windows/wsl/basic-commands)
> - [WSL 配置 (.wslconfig)](https://learn.microsoft.com/zh-cn/windows/wsl/wsl-config)
