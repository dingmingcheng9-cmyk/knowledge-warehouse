---
title: 宝塔面板常用命令速查
created: 2026-05-02
updated: 2026-05-02
type: concept
tags: [tool, devops, note]
sources: [raw/baota-docker-doubao-conversation.md, raw/knowledge-learning-notes.md]
---

# 宝塔面板 (Baota/BT Panel) 常用命令

> 宝塔 Linux 面板的 CLI 命令速查。涵盖面板服务管理、核心组件启停、手动兜底操作。

## 面板主服务（6.x+ 通用）

### 快捷命令（推荐）
```bash
bt 3          # 启动面板（最常用）
bt start      # 等效 bt 3
bt 1          # 重启面板
bt 2          # 停止面板
bt 4          # 重载面板
bt 9          # 查看面板登录信息
bt 5          # 修改面板密码
bt 6          # 修改面板端口
```

### Systemd 服务命令
```bash
systemctl start bt
systemctl start bt-panel    # 部分版本用这个
systemctl restart bt-panel
systemctl stop bt-panel
systemctl status bt-panel
```

### Init.d 兼容（旧系统）
```bash
service bt start
/etc/init.d/bt start
```

## 核心组件启停

```bash
# Nginx
service nginx start | stop | restart | reload

# MySQL
service mysqld start | stop | restart

# PHP (按版本号)
service php-fpm-74 start | stop | restart
# 其他版本: php-fpm-72, php-fpm-80, php-fpm-81 ...

# Apache
service httpd start | stop | restart

# FTP
service pure-ftpd start | stop | restart
```

## 手动兜底（面板异常时）

当 `bt` 命令或 systemd 不可用时，直接进面板目录执行：

```bash
cd /www/server/panel
python tools.pyc start     # 启动
python tools.pyc restart   # 重启
python tools.pyc stop      # 停止
```

## 查看面板信息

```bash
/etc/init.d/bt default    # 查看默认登录地址、用户名、密码
bt 9                       # 查看当前面板登录信息
bt 14                      # 查看面板当前状态
```

## 面板端口管理

```bash
bt status       # 检查运行情况
bt default      # 查看登录信息（密码仅首次产生）

# 查看/设置面板端口
cat /www/server/panel/data/port.pl              # 查看当前端口
echo "22048" > /www/server/panel/data/port.pl   # 设置端口为 22048
bt 1            # 改完端口后重启面板

# 防火墙放行端口（Ubuntu）
ufw allow 22048/tcp
ufw status
```

面板默认安全入口为随机字符串（如 `e9d6ce01`），每次重启面板都会变。  
示例访问地址：`https://外网IP:22048/e9d6ce01`

## 相关概念

- [[docker-container-basics]] — Docker 容器常用配置参数
- [[proxy-setup-sing-box]]

## 资料来源

- [豆包分享对话：宝塔查看状态与登录地址命令](https://www.doubao.com/thread/a4bcce9625dea)
