---
tags:
  - wsl
  - windows
  - commands
  - cheatsheet
---

# WSL2 指令速查

> 适用环境：Windows 11 + WSL 2.7+
> 适用发行版：Ubuntu（WSL2 模式）

## 基础管理

```cmd
wsl -l -v                    # 列出所有发行版及状态
wsl --status                 # WSL 总体状态
wsl --version                # WSL 引擎版本
wsl                          # 进入默认发行版
wsl -d Ubuntu                # 进入指定发行版
wsl -u root                  # 以 root 进入
wsl --shutdown               # 完全关闭所有发行版
wsl --terminate Ubuntu       # 仅关闭指定发行版
```

## 发行版管理

```cmd
wsl --install                # 安装 WSL + 默认 Ubuntu
wsl --list --online          # 查看可在线安装的发行版
wsl --export Ubuntu file.tar # 导出备份
wsl --import Ubuntu dir file # 导入恢复
wsl --unregister Ubuntu      # 删除发行版
wsl --set-default-version 2  # 新装默认 WSL2
wsl --set-default Ubuntu     # 设置默认发行版
wsl --move Ubuntu new_path   # 移动发行版位置
wsl --manage Ubuntu --set-sparse true  # 开启稀疏磁盘
```

## 配置

- **全局配置**：`C:\Users\<用户名>\.wslconfig`
  - `memory=8GB` — 限制内存
  - `processors=8` — 限制 CPU 核心
  - `swap=2GB` — 限制 swap
- **内部配置**：`/etc/wsl.conf`
  - `systemd=true` — 启用 systemd
  - `default=dingm` — 默认用户
  - `generateResolvConf=false` — 关闭自动 DNS

> 修改配置后需 `wsl --shutdown` 再重启生效。

## 文件互操作

| 方向 | 路径 |
|------|------|
| Windows → WSL | `\\wsl.localhost\Ubuntu\` |
| WSL → Windows | `/mnt/c/` |
| 路径转换 | `wslpath -w /home/...`（Linux→Win）|
| 路径转换 | `wslpath -u "C:\..."`（Win→Linux）|
| VS Code | `code .`（WSL 内执行）|
| 调用 Windows exe | `notepad.exe file`, `explorer.exe .` |

## 网络排查

```bash
ip addr && ip route          # 查看 IP 和路由
ping -c 4 8.8.8.8           # 测试外网
nslookup baidu.com           # 测试 DNS
```

> Mirrored 模式（默认）无需端口转发。详见 [[wsl-mirrored-network]]

## 故障处理

| 问题 | 解决 |
|------|------|
| 无法启动 | `dism.exe` 启用 VirtualMachinePlatform + 重启 |
| 内存过高 | `wsl --shutdown` 或 `.wslconfig` 限制 memory |
| 网络不通 | `wsl --shutdown` 后重启 |
| 卡死无响应 | `wsl --shutdown` 或杀 Vmmem 进程 |

---

> **参考**: [WSL 官方文档](https://learn.microsoft.com/zh-cn/windows/wsl/) · [命令参考](https://learn.microsoft.com/zh-cn/windows/wsl/basic-commands)
> 完整版见: `raw/articles/wsl2-commands-cheatsheet.md`
