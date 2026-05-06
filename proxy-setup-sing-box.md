# 代理配置文档 (sing-box + anytls)

> 环境: Debian 13 (trixie) x86_64
> 配置日期: 2026-05-02

---

## 1. 代理软件信息

- **软件**: sing-box (通用代理客户端，支持 VLESS / anytls / Shadowsocks / Trojan 等)
- **版本**: 1.13.11 (2026-04-22 发布)
- **官方源码**: https://github.com/SagerNet/sing-box
- **国内加速下载**: https://gh-proxy.com/https://github.com/SagerNet/sing-box/releases/download/v1.13.11/sing-box-1.13.11-linux-amd64.tar.gz

*其他可用国内镜像*:
- `https://gh.api.99988866.xyz/` (可能失效)
- `https://github.moeyy.xyz/` (可能失效)

---

## 2. 文件位置

| 文件 | 路径 |
|------|------|
| 主程序 | `/usr/local/bin/sing-box` |
| 配置文件 | `/opt/sing-box/config.json` |
| 日志文件 | `/opt/sing-box/sing-box.log` |
| 全局代理配置 | `/etc/environment` |

---

## 3. 代理信息

来自订阅链接 `https://b.youtulink3.top/dy/a2c56e60127d0a782d345ebb0ce076df`

- **协议**: anytls
- **服务器**: `oldsd.youtu2.top:34102`
- **密码 (UUID)**: `390a8def-c87a-4f83-b6a1-ff9d986080aa`
- **SNI 伪装**: `s.dydyyd.com`
- **TLS 验证**: 开启 (insecure: false)
- **剩余流量**: 14.3 GB (截至配置时)

### 订阅链接原始内容 (Base64 解码后)
```
anytls://390a8def-c87a-4f83-b6a1-ff9d986080aa@oldsd.youtu2.top:34102?sni=s.dydyyd.com&insecure=0#%E5%89%A9%E4%BD%99%E6%B5%81%E9%87%8F%EF%BC%9A14.3%20G
```

---

## 4. 配置文件详解

`/opt/sing-box/config.json` 结构:

```json
{
  "log": {
    "level": "warn",         // 日志级别: debug / info / warn / error
    "output": "/opt/sing-box/sing-box.log"
  },
  "inbounds": [
    {
      "type": "mixed",          // SOCKS5 + HTTP 混合代理
      "tag": "mixed-in",
      "listen": "127.0.0.1",    // 只监听本地
      "listen_port": 1080       // 代理端口
    }
  ],
  "outbounds": [
    {
      "type": "anytls",         // 代理协议
      "tag": "proxy",
      "server": "oldsd.youtu2.top",
      "server_port": 34102,
      "password": "390a8def-c87a-4f83-b6a1-ff9d986080aa",
      "tls": {
        "enabled": true,
        "server_name": "s.dydyyd.com",  // SNI 伪装域名
        "insecure": false                // 是否跳过证书验证
      }
    },
    {
      "type": "direct",         // 直连出口（备用）
      "tag": "direct"
    }
  ]
}
```

---

## 5. 操作方法

### 启动代理
```bash
sing-box run -c /opt/sing-box/config.json
```

### 停止代理
```bash
kill $(pgrep sing-box)
# 或
pkill sing-box
```

### 检查代理是否运行
```bash
ps aux | grep sing-box        # 进程是否存在
ss -tlnp | grep 1080           # 端口是否监听
curl -s --max-time 10 "https://www.baidu.com"  # 测试能否上网
```

### 测试代理连通性
```bash
# 走代理访问 (如果环境变量已设可以直接 curl)
curl -sI --proxy "socks5://127.0.0.1:1080" "https://www.baidu.com"

# 走代理访问 GitHub
curl -sI --proxy "socks5://127.0.0.1:1080" "https://github.com"
```

---

## 6. 变更代理配置（换节点/订阅）

### 修改节点信息
编辑 `/opt/sing-box/config.json`，修改 `outbounds[0]` 中的以下字段：
- `server` — 服务器地址
- `server_port` — 端口
- `password` — 密码/UUID
- `tls.server_name` — SNI 伪装域名

改完后重启 sing-box:
```bash
pkill sing-box
sing-box run -c /opt/sing-box/config.json &
```

### 使用新的订阅链接
如果订阅链接变了，获取新节点信息:
```bash
curl -s "https://新订阅链接" | base64 -d
```
返回结果类似 `anytls://xxx@server:port?sni=xxx&insecure=0#名称`
据此更新 config.json 中的对应字段。

---

## 7. 更新 sing-box 版本

```bash
# 获取最新版本号
VERSION=$(curl -s "https://api.github.com/repos/SagerNet/sing-box/releases/latest" | grep '"tag_name"' | cut -d'"' -f4 | sed 's/^v//')

# 通过镜像下载新版
wget -O /tmp/sing-box-new.tar.gz \
  "https://gh-proxy.com/https://github.com/SagerNet/sing-box/releases/download/v${VERSION}/sing-box-${VERSION}-linux-amd64.tar.gz"

# 解压替换
tar xzf /tmp/sing-box-new.tar.gz -C /tmp/
cp /tmp/sing-box-${VERSION}-linux-amd64/sing-box /usr/local/bin/sing-box

# 重启服务
pkill sing-box
sing-box run -c /opt/sing-box/config.json &
```

---

## 8. 全局代理环境变量

写入 `/etc/environment`:

```
http_proxy=socks5://127.0.0.1:1080
https_proxy=socks5://127.0.0.1:1080
all_proxy=socks5://127.0.0.1:1080
HTTP_PROXY=socks5://127.0.0.1:1080
HTTPS_PROXY=socks5://127.0.0.1:1080
ALL_PROXY=socks5://127.0.0.1:1080
NO_PROXY=localhost,127.0.0.1,::1
no_proxy=localhost,127.0.0.1,::1
```

当前会话生效:
```bash
export http_proxy="socks5://127.0.0.1:1080"
export https_proxy="socks5://127.0.0.1:1080"
export all_proxy="socks5://127.0.0.1:1080"
```

取消代理:
```bash
unset http_proxy https_proxy all_proxy HTTP_PROXY HTTPS_PROXY ALL_PROXY
```
