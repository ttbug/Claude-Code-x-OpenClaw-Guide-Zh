# 02. 环境安装

## 前置要求

只需要一个东西：**Node.js 22 或更高版本**。

检查你的 Node 版本：

```bash
node --version
# 需要 v22.0.0 或更高
```

没有 Node.js？往下看各平台安装方法。

## macOS 安装

### 方法一：一键安装（推荐）

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

这个脚本会自动：
- 检测你的系统环境
- 安装 Node.js 22+（如果没有）
- 全局安装 OpenClaw
- 配置 PATH 环境变量

### 方法二：手动安装

```bash
# 先装 Node.js（用 Homebrew）
brew install node@22

# 全局安装 OpenClaw
npm install -g openclaw@latest

# 验证安装
openclaw --version
```

## Linux 安装

### 方法一：一键安装（推荐）

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

### 方法二：手动安装

```bash
# Ubuntu / Debian
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs

# CentOS / RHEL
curl -fsSL https://rpm.nodesource.com/setup_22.x | sudo bash -
sudo yum install -y nodejs

# 安装 OpenClaw
npm install -g openclaw@latest
```

### 方法三：Nix

```bash
# 使用 Nix flake
nix run github:openclaw/openclaw
```

## Windows 安装

### 方法一：PowerShell 一键安装（推荐）

以管理员身份打开 PowerShell：

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

### 方法二：WSL2（推荐开发者使用）

Windows 上跑 OpenClaw 最稳的方式是用 WSL2：

```powershell
# 1. 安装 WSL2
wsl --install

# 2. 进入 WSL
wsl

# 3. 在 WSL 中一键安装
curl -fsSL https://openclaw.ai/install.sh | bash
```

### 方法三：手动安装

1. 从 [nodejs.org](https://nodejs.org/) 下载 Node.js 22+ 安装包
2. 安装完成后打开终端：

```bash
npm install -g openclaw@latest
openclaw --version
```

## Docker 安装（可选）

Docker 适合想要隔离环境或在 VPS 上部署的用户：

```bash
# 克隆仓库
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# 一键 Docker 安装
./docker-setup.sh
```

`docker-setup.sh` 会自动：
- 构建 Gateway 镜像
- 运行引导向导
- 启动 Gateway（Docker Compose）
- 生成 Gateway Token 写入 `.env`

启动后访问 `http://127.0.0.1:18789/` 即可。

### Docker 手动流程

```bash
# 构建镜像
docker build -t openclaw:local -f Dockerfile .

# 运行引导向导
docker compose run --rm openclaw-cli onboard

# 启动网关
docker compose up -d openclaw-gateway
```

### ClawDock 快捷命令（可选）

安装 Shell 辅助工具，简化日常 Docker 管理：

```bash
mkdir -p ~/.clawdock && curl -sL https://raw.githubusercontent.com/openclaw/openclaw/main/scripts/shell-helpers/clawdock-helpers.sh -o ~/.clawdock/clawdock-helpers.sh

# 添加到 shell 配置
echo 'source ~/.clawdock/clawdock-helpers.sh' >> ~/.zshrc && source ~/.zshrc

# 使用快捷命令
clawdock-start    # 启动
clawdock-stop     # 停止
clawdock-dashboard  # 打开面板
clawdock-help     # 查看所有命令
```

## VPS 部署

最便宜的方案：Hetzner，€3.49/月起。

```bash
# SSH 连接你的 VPS
ssh root@your-vps-ip

# 一键安装
curl -fsSL https://openclaw.ai/install.sh | bash

# 运行引导
openclaw onboard --install-daemon

# 检查状态
openclaw gateway status
```

远程访问建议用 SSH 隧道或 Tailscale：

```bash
# 本地机器上建 SSH 隧道
ssh -L 18789:127.0.0.1:18789 root@your-vps-ip

# 然后本地浏览器访问
# http://127.0.0.1:18789/
```

## 验证安装

不管用哪种方式安装，最后都要验证：

```bash
# 检查版本
openclaw --version

# 检查网关状态
openclaw gateway status

# 打开控制面板
openclaw dashboard
```

如果控制面板能正常加载，恭喜你，安装成功！🎉

## 常见安装问题

### Node.js 版本太低

```bash
# 检查版本
node --version

# 如果低于 22，升级
# macOS
brew upgrade node

# Linux (nvm)
nvm install 22
nvm use 22
```

### 权限问题（Linux/macOS）

```bash
# 不要用 sudo 安装 npm 全局包！
# 配置 npm 全局目录
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

# 重新安装
npm install -g openclaw@latest
```

### Windows 路径问题

确保 Node.js 和 npm 在 PATH 中：

```powershell
# 检查
$env:PATH -split ';' | Select-String 'node'

# 如果没有，手动添加
[Environment]::SetEnvironmentVariable("PATH", "$env:PATH;C:\Program Files\nodejs", "User")
```

## 下一步

安装完成！去 [03. 快速开始](03-quickstart.md) 跑起你的第一个 AI 对话！
