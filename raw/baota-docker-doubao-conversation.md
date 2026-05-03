---
source_url: https://www.doubao.com/thread/a4bcce9625dea
ingested: 2026-05-02
sha256: placeholder
---

# 原始对话：宝塔面板命令 + Docker 容器管理（豆包）

标题: 宝塔查看状态与登录地址命令
分享者: 小蠢货
平台: 豆包 (doubao.com)

## 对话一：宝塔面板管理命令

**用户提问**: 宝塔启动指令

**AI 回答**:

### 一、面板主服务（6.x+ 通用）

#### 1. 快捷命令（推荐）
```bash
bt 3          # 启动面板（最常用）
bt start      # 等效 bt 3
bt 1          # 重启面板
bt 2          # 停止面板
```

#### 2. 系统服务命令
```bash
# Systemd（CentOS7+/Debian9+/Ubuntu16.04+）
systemctl start bt
systemctl start bt-panel

# Init.d（旧系统）
service bt start
/etc/init.d/bt start
```

### 二、核心组件（Nginx/MySQL/PHP）
```bash
# Nginx
service nginx start

# MySQL
service mysqld start

# PHP（按版本，如7.4）
service php-fpm-74 start
```

### 三、手动兜底（服务异常时）
```bash
cd /www/server/panel
python tools.pyc start
```

---

## 对话二：Docker 容器管理进阶

### 创建容器最常用拓展参数

基础命令：`docker run -d --name 容器名 镜像名`

#### 1. 端口映射 `-p`
```bash
-p 主机端口:容器端口
# 示例
docker run -d --name hermes -p 22048:22048 hermes-image
```

#### 2. 目录挂载 `-v`
```bash
-v 主机目录:容器目录
# 示例
docker run -d --name hermes -v /www/wwwroot:/app hermes-image
```

#### 3. 环境变量 `-e`
```bash
-e KEY=VALUE
# 示例
docker run -d --name hermes -e TZ=Asia/Shanghai hermes-image
```

#### 4. 开机自启
```bash
--restart=always
```

#### 5. 资源限制
```bash
--memory 512m   # 最大内存
--cpus 1        # 最大CPU核心
```

#### 全能版示例
```bash
docker run -d \
  --name hermes \
  --restart=always \
  -p 22048:22048 \
  -v /www/wwwroot:/app \
  -e TZ=Asia/Shanghai \
  --memory 512m \
  hermes-image
```

### 制作镜像与导出

```bash
# commit - 将运行中的容器保存为本地镜像
docker commit 容器名 新镜像名

# save - 将镜像导出为.tar文件，可迁移/备份
docker save -o my-hermes.tar my-hermes:v1
```

---

## 对话三：概念讲解

**用户提问**: 什么叫目录挂载？为什么要设环境变量？commit 为什么还要 save？

**AI 回答**:

### 目录挂载
- **本质**: 把服务器上的文件夹和容器里的文件夹绑在一起，互通数据
- **比喻**: 电脑=服务器，U盘=容器，挂载=插U盘，两边文件实时同步
- **原因**: 容器删了文件还在；方便在外部直接改代码传文件

### 环境变量
- **本质**: 给程序传配置参数，不用改代码、不用进容器
- **常见设置**: 时区 `TZ=Asia/Shanghai`（必须）、数据库信息、运行模式、密钥/token

### commit vs save
- **commit**: 本机生成镜像（在电脑上做好压缩包）
- **save**: 把镜像打包成文件带走/备份（复制到U盘去别的电脑用）
- commit 只存在当前服务器，save 后可迁移
