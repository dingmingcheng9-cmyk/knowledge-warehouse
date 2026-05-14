---
source_url: （本地笔记）
ingested: 2026-05-14
---

# 知识学习笔记（综合）

> 用户自整理的系统运维学习笔记：Hermes 安装、Docker 管理、SSH 配置、宝塔面板、网络测试等。

## 测试电脑能否访问国外网络

```bash
curl -I www.google.com
```
返回 `HTTP/1.1 200 OK` → 可以访问  
超时/connection refused → 访问不了

## Hermes 安装

### Windows 系统安装（需先装 Git）

**沙箱版：**
```powershell
# 国内镜像
irm https://res1.hermesagent.org.cn/install.ps1 | iex
# 官方
irm https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.ps1 | iex
```

**原生版：**
```powershell
irm https://res1.hermesagent.org.cn/install.ps1 | iex
```

### Ubuntu 系统安装

```bash
# 1. 安装依赖
apt update && apt install -y git curl wget

# 2. 验证
git --version && curl --version | head -n 1 && wget --version | head -n 1

# 3. 修改 pip 镜像为清华源/阿里源
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
# 或
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/

# 4. 一键安装
curl -fsSL https://res1.hermesagent.org.cn/install.sh | bash        # 国内镜像
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash  # 官方
```

## Docker 容器中运行 Hermes

启动后使用 uv 来运行：
```bash
uv run hermes                  # 终端交互式对话
uv run hermes gateway          # 启动网关
```

容器内查询：
```bash
netstat -tlnp     # 查询开放端口
mount             # 查看挂载目录
env               # 查看环境变量
```

容器外查询：
```bash
docker port 容器名                             # 查看端口映射
docker inspect 容器名 | grep Mounts -A 20      # 查看挂载
docker inspect 容器名                          # 查看完整配置
```

## Ubuntu 开启 SSH 远程连接

```bash
# 1. 安装 SSH 服务
sudo apt update
sudo apt install openssh-server -y

# 2. 启动并设置开机自启
sudo systemctl start ssh
sudo systemctl enable ssh

# 3. 检查运行状态
sudo systemctl status ssh

# 4. 放行防火墙
sudo ufw allow 22/tcp
```

从 Windows 连接：`ssh root@192.168.31.187`

## IP 地址查询

| 系统 | 命令 | 说明 |
|------|------|------|
| Windows | `ipconfig` | 查看本机所有IP |
| Windows | `curl ifconfig.me` | 查看外网/公网IP |
| Ubuntu | `ip a` | 查看内网网卡IP |
| Ubuntu | `curl ifconfig.me` | 查看外网/公网IP |
| Ubuntu | `ifconfig` | 查看网卡信息 |

## 宝塔面板端口管理

```bash
bt status       # 检查运行情况
bt default      # 查看登录信息（密码仅首次产生）

# 查看/设置面板端口
cat /www/server/panel/data/port.pl
echo "22048" > /www/server/panel/data/port.pl  # 设置端口
bt 1            # 改完端口重启面板

# 防火墙放行端口
ufw allow 22048/tcp
ufw status
```

外网访问：`https://223.94.207.172:22048/e9d6ce01`  
内网访问：`https://192.168.31.187:22048/e9d6ce01`

## hermes-cluster 项目结构

```
hermes-cluster/
├── central/                          ← 中心服务（80% 代码）
│   ├── api/        路由层（薄）       ← 只做分发
│   ├── core/       业务逻辑           ← 设备/日志/目录/指令
│   ├── connector/  连接器（可插拔）    ← SSH/Docker/Hermes
│   ├── models/     数据模型           ← 统一契约
│   ├── store/      持久化             ← SQLite
│   ├── logs/       ★ 统一日志存储     ← 按设备/日期分目录
│   ├── files/      ★ 统一文件存储     ← 脚本/配置/备份/传输
│   └── frontend/   前端页面
├── agent/                            ← 节点代理（可选）
├── scripts/                          ← 部署运维脚本
└── docs/                             ← 文档
```
