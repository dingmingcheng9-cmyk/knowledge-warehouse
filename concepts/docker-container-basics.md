---
title: Docker 容器常用配置参数
created: 2026-05-02
updated: 2026-05-02
type: concept
tags: [tool, devops, note]
sources: [raw/baota-docker-doubao-conversation.md]
---

# Docker 容器常用配置参数

> `docker run` 创建容器时最常用的扩展参数速查：端口映射、目录挂载、环境变量、开机自启。

## 基础命令

```bash
docker run -d --name 容器名 镜像名
```

## 五个最常用参数

### 1. 端口映射 `-p`

把服务器端口与容器端口打通：

```bash
-p 主机端口:容器端口
# 例: docker run -d --name myapp -p 8080:80 nginx
```

### 2. 目录挂载 `-v`

**本质**：把服务器上的文件夹和容器里的文件夹绑在一起，互通数据。

- 容器删了，挂载目录下的文件还在
- 方便直接在外部修改代码/上传文件
- 类似"U盘插电脑"——两边实时同步

```bash
-v 服务器目录:容器目录
# 例: docker run -d --name myapp -v /www/wwwroot:/app nginx
```

### 3. 环境变量 `-e`

给程序传配置参数，不用改代码、不用进容器：

```bash
-e KEY=VALUE
```

**最常见设置**：
- `-e TZ=Asia/Shanghai` — **必须**，不然容器时间是 UTC，日志/定时任务全乱
- `-e DB_HOST=127.0.0.1` — 数据库地址
- `-e MODE=prod` — 运行模式
- `-e API_KEY=xxxx` — 密钥/token

### 4. 开机自启

```bash
--restart=always
```

服务器重启后容器自动启动。

### 5. 资源限制

```bash
--memory 512m   # 最大内存
--cpus 1        # 最大 CPU 核心
```

## 全能版示例

```bash
docker run -d \
  --name myapp \
  --restart=always \
  -p 8080:80 \
  -v /www/wwwroot:/app \
  -e TZ=Asia/Shanghai \
  --memory 512m \
  my-image
```

## 制作镜像与导出

| 命令 | 作用 | 类比 |
|------|------|------|
| `docker commit 容器名 新镜像名` | 容器→本地镜像（仅当前服务器） | 在电脑上做好压缩包 |
| `docker save -o file.tar 镜像名` | 镜像→.tar文件（可迁移备份） | 复制到 U 盘带走 |

**关键区别**：commit 只存本地，save 生成文件后可以拷贝到其他服务器离线部署。

## 常用速查

```bash
docker images          # 查看所有镜像
docker ps -a           # 查看所有容器
docker start 容器名    # 启动已停止容器
docker exec -it 容器名 /bin/bash  # 进入容器
docker stop 容器名     # 停止容器
docker rm 容器名       # 删除容器
docker rmi 镜像名      # 删除镜像
```

## 相关概念

- [[baota-panel-commands]] — 宝塔面板管理命令
- [[proxy-setup-sing-box]]

## 重要补充：docker commit 打包范围

`docker commit 容器名 新镜像名` 可以理解为打包容器的"内部快照"。

### ✅ commit 会保存
- 安装的所有软件、修改的配置文件
- 容器当前的系统环境、用户、权限、时区
- 运行过程中产生的所有改动
- **容器现在长啥样，commit 出来的镜像就长啥样**

### ❌ commit 不会保存（运行时参数）
| 参数 | 原因 |
|------|------|
| 端口映射 `-p` | 宿主机与容器的通道，不属于容器内部 |
| 目录挂载 `-v` | 宿硬盘和容器的链接 |
| 环境变量 `-e` | 外部传入程序的参数 |
| 开机自启 `--restart` | 运行配置 |
| 容器名 `--name` | 运行配置 |
| 资源限制 `--memory/--cpus` | 运行配置 |

**结论**: commit 只打包容器内部系统。下次 `docker run` 时，端口、挂载、环境变量、自启必须重新加一遍。

## 资料来源

- [豆包分享对话：宝塔查看状态与登录地址命令](https://www.doubao.com/thread/a4bcce9625dea)
- [豆包分享对话：容器配置查看方法 / docker commit 打包范围](https://www.doubao.com/thread/a8926a1e854a5)
