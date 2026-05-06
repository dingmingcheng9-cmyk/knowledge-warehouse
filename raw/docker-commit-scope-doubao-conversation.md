---
source_url: https://www.doubao.com/thread/a8926a1e854a5
ingested: 2026-05-02
---

# 原始对话：docker commit 打包范围详解（豆包）

标题: 容器配置查看方法
分享者: 小蠢货
平台: 豆包 (doubao.com)

## 对话

**用户提问**: 执行 docker commit 制作镜像时，那它是将容器整个完完全全，包括环境等等，包括环境端口等等都打包成镜像文件，还是只保存一些一部分？

**AI 回答**:

### docker commit 会保存进镜像的内容
- 安装的所有软件、改的配置文件
- 容器当前的系统环境
- 容器里的用户、权限、时区
- 运行过程中产生的所有改动

一句话：**容器现在长啥样，commit 出来的镜像就长啥样。**

### docker commit 不会保存的内容
1. 端口映射 `-p`
2. 目录挂载 `-v`
3. 环境变量 `-e`
4. 开机自启 `--restart`
5. 容器名字 `--name`
6. 内存/CPU 限制等运行参数

### 为什么？
因为这些不是容器内部的东西，而是 docker 运行容器时给它加的"外部配置"：
- 端口映射：宿主机和容器之间的通道
- 挂载目录：宿硬盘和容器的链接
- 环境变量：外部传给程序的参数

这些属于**运行时配置**，不属于容器本身，所以 commit 不会带走。

### 最直观的例子
启动时：
```bash
docker run -d --name hermes -p 22048:22048 -v /www:/app -e TZ=Asia/Shanghai hermes
```

在容器里装了很多东西后执行：
```bash
docker commit hermes my-hermes-img
```

新镜像 `my-hermes-img` **包含**：
- 装的所有软件、所有修改的文件、容器内部环境

新镜像 **不包含**：
- 22048 端口映射
- /www:/app 挂载
- TZ=Asia/Shanghai 环境变量
- 容器名字
- 自启策略

### 最重要结论
**commit 只打包容器内部系统，不打包运行参数。**

所以用 commit 生成镜像后，下次用 `docker run` 启动新容器时，**端口、挂载、环境变量、自启必须重新加一遍。**

### 一句话记忆
- `docker commit` = 打包**容器内部**
- `docker run` 的参数 = 容器**外部运行配置**
