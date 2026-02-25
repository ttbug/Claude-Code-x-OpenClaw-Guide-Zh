# 10. 安全最佳实践

## 为什么安全很重要？

OpenClaw 是一个能读写文件、执行命令、发送消息的 AI Agent。如果配置不当，可能会：

- 泄露你的 API Key 和个人数据
- 被恶意消息触发危险操作
- 暴露你的文件系统

OpenClaw 在 2026 年初曾爆出 CVE 安全漏洞，社区对安全问题高度重视。以下是你必须知道的安全配置。

## Gateway Token（必须设置）

```bash
# 设置 Gateway Token
openclaw config set gateway.token "your-strong-random-token"

# 或通过环境变量
export OPENCLAW_GATEWAY_TOKEN="your-strong-random-token"
```

没有 Token 的 Gateway 任何人都能连接。这是最基本的安全措施。

## TLS 1.3（v2026.2.1 强制）

从 v2026.2.1 开始，OpenClaw 强制使用 TLS 1.3 作为最低标准。如果你的环境不支持，需要升级。

## 配对系统（DM 安全）

默认情况下，陌生人发来的消息需要你手动批准：

```bash
# 查看待配对
openclaw pairing list

# 批准
openclaw pairing approve <contact>

# 拒绝
openclaw pairing reject <contact>
```

配置自动配对（不推荐）：

```json
{
  "channels": {
    "pairing": {
      "autoApprove": false,
      "requireApproval": true
    }
  }
}
```

## 系统提示词护栏（v2026.2.1）

防止提示词注入攻击：

```json
{
  "agents": {
    "defaults": {
      "systemPrompt": {
        "guardrails": true,
        "maxLength": 10000
      }
    }
  }
}
```

## 沙箱隔离

让 Agent 的工具执行在 Docker 容器中运行，防止 AI 操作主机文件系统：

```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "enabled": true,
        "type": "docker",
        "networkAccess": false,
        "readOnlyPaths": ["/etc", "/usr"],
        "writablePaths": ["~/.openclaw/workspace"]
      }
    }
  }
}
```

## API Key 安全

```bash
# 使用环境变量，不要写在配置文件里
export OPENAI_API_KEY="sk-proj-xxxxx"
export ANTHROPIC_API_KEY="sk-ant-xxxxx"

# 检查配置文件中是否有明文 Key
grep -r "sk-" ~/.openclaw/openclaw.json
```

如果必须写在配置文件中，确保文件权限正确：

```bash
chmod 600 ~/.openclaw/openclaw.json
```

## 网络安全

### 绑定地址

Gateway 默认绑定 `127.0.0.1`（仅本地访问）。不要改成 `0.0.0.0` 除非你知道自己在做什么：

```json
{
  "gateway": {
    "host": "127.0.0.1",
    "port": 18789
  }
}
```

### 防火墙

如果在 VPS 上运行：

```bash
# UFW（Ubuntu）
ufw allow ssh
ufw deny 18789
ufw enable

# 通过 SSH 隧道或 Tailscale 访问，不要直接暴露端口
```

## 安全检查清单

每次部署前过一遍：

- [ ] Gateway Token 已设置
- [ ] API Key 使用环境变量，不在配置文件中明文存储
- [ ] 配对系统已启用（不自动批准陌生人）
- [ ] Gateway 绑定 127.0.0.1（不对外暴露）
- [ ] 配置文件权限 600
- [ ] 沙箱已启用（生产环境）
- [ ] TLS 1.3 已启用
- [ ] 系统提示词护栏已启用
- [ ] 定期更新到最新版本

## 安全更新

OpenClaw 更新非常频繁，安全修复通常在 24 小时内发布：

```bash
# 检查更新
openclaw update check

# 更新到最新版
npm update -g openclaw@latest

# Docker 更新
cd openclaw && git pull && docker compose build && docker compose up -d
```

## 下一步

安全加固完成！去 [11. 常见问题](11-faq.md) 看看其他人踩过的坑！
