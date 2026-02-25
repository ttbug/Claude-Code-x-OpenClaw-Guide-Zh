# 09. Docker 部署

## 什么时候用 Docker？

- 想要隔离的、一次性的 Gateway 环境
- 在没有本地安装条件的主机上运行
- VPS 远程部署
- Agent 沙箱隔离（工具执行在容器中）

不需要 Docker 的情况：在自己电脑上开发，直接用本地安装更快。

## 快速部署

### 一键 Docker 安装

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw
./docker-setup.sh
```

脚本自动完成：
1. 构建 Gateway 镜像
2. 运行引导向导
3. 打印可选的模型提供商配置提示
4. 通过 Docker Compose 启动 Gateway
5. 生成 Gateway Token 写入 `.env`

完成后访问 `http://127.0.0.1:18789/`，在设置中粘贴 Token。

### 手动 Docker 部署

```bash
# 构建镜像
docker build -t openclaw:local -f Dockerfile .

# 运行引导向导
docker compose run --rm openclaw-cli onboard

# 启动网关
docker compose up -d openclaw-gateway
```

## Docker Compose 配置

`docker-compose.yml` 关键配置：

```yaml
services:
  openclaw-gateway:
    image: openclaw:local
    ports:
      - "18789:18789"
    volumes:
      - ~/.openclaw:/home/node/.openclaw
      - ~/.openclaw/workspace:/home/node/.openclaw/workspace
    environment:
      - OPENCLAW_GATEWAY_TOKEN=${OPENCLAW_GATEWAY_TOKEN}
    restart: unless-stopped
```

### 可选环境变量

```bash
# 额外 apt 包（构建时安装）
OPENCLAW_DOCKER_APT_PACKAGES="ffmpeg imagemagick"

# 额外挂载
OPENCLAW_EXTRA_MOUNTS="/path/to/data:/data"

# 持久化 home 目录
OPENCLAW_HOME_VOLUME=openclaw-home
```

## VPS 部署实战

### Hetzner（最便宜，€3.49/月起）

```bash
# 1. 创建 VPS（推荐 CX22: 2 vCPU, 4GB RAM）
# 2. SSH 连接
ssh root@your-vps-ip

# 3. 安装 Docker
curl -fsSL https://get.docker.com | sh

# 4. 克隆并部署
git clone https://github.com/openclaw/openclaw.git
cd openclaw
./docker-setup.sh
```

### 远程访问

Gateway 默认绑定 `127.0.0.1`，外部无法直接访问。推荐方案：

#### 方案一：SSH 隧道（简单安全）

```bash
# 在本地机器上
ssh -L 18789:127.0.0.1:18789 root@your-vps-ip

# 然后本地浏览器访问
# http://127.0.0.1:18789/
```

#### 方案二：Tailscale（推荐长期使用）

```bash
# VPS 上安装 Tailscale
curl -fsSL https://tailscale.com/install.sh | sh
tailscale up

# 本地也安装 Tailscale
# 然后通过 Tailscale IP 访问
# http://100.x.x.x:18789/
```

#### 方案三：反向代理 + HTTPS

```bash
# 安装 Caddy
apt install -y caddy

# 配置 /etc/caddy/Caddyfile
# openclaw.yourdomain.com {
#     reverse_proxy 127.0.0.1:18789
# }

systemctl restart caddy
```

## Agent 沙箱（Docker 隔离）

Agent 沙箱和 Gateway 容器化是两回事：

- **Gateway 容器化** — 整个 OpenClaw 跑在 Docker 里
- **Agent 沙箱** — Gateway 跑在主机上，但 Agent 的工具执行在 Docker 容器中

Agent 沙箱配置：

```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "enabled": true,
        "image": "openclaw:sandbox"
      }
    }
  }
}
```

沙箱镜像：
- `Dockerfile.sandbox` — 基础沙箱
- `Dockerfile.sandbox-browser` — 带浏览器的沙箱
- `Dockerfile.sandbox-common` — 通用沙箱

## Fly.io 部署

```bash
# 安装 flyctl
curl -L https://fly.io/install.sh | sh

# 部署
fly launch
fly deploy
```

## Render 部署

仓库中已包含 `render.yaml`，直接在 Render 面板中导入 GitHub 仓库即可。

## 常见 Docker 问题

### 镜像构建失败

```bash
# 清理缓存重新构建
docker build --no-cache -t openclaw:local -f Dockerfile .
```

### 容器无法启动

```bash
# 查看日志
docker compose logs openclaw-gateway

# 检查端口占用
lsof -i :18789
```

### 数据持久化

确保挂载了正确的目录：

```bash
# 检查挂载
docker inspect openclaw-gateway | grep -A 5 Mounts
```

## 下一步

部署搞定！去 [10. 安全最佳实践](10-security.md) 加固你的 AI 助手！
