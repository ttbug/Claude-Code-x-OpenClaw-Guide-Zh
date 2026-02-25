# 05. 消息平台集成

## 概述

OpenClaw 的核心卖点之一：一个 Gateway 连接所有消息平台。你在 WhatsApp 上给 AI 发消息，它能帮你在 Telegram 上回复别人。

## WhatsApp（最常用）

WhatsApp 集成基于 Baileys（WhatsApp Web 协议），不需要官方 API，免费。

### 配置步骤

```bash
# 运行引导向导中选择 WhatsApp
openclaw onboard

# 或手动配置
openclaw channels add whatsapp
```

配置后会显示一个二维码，用手机 WhatsApp 扫码配对。

### 注意事项

- 一个 Gateway 只能连接一个 WhatsApp 账号
- 需要保持 Gateway 运行，否则 WhatsApp 会断开
- 建议用专门的手机号，别用主号（避免被封）

## Telegram

Telegram 集成基于 grammY Bot 框架，需要创建一个 Telegram Bot。

### 配置步骤

```bash
# 1. 在 Telegram 中找 @BotFather
# 2. 发送 /newbot 创建机器人
# 3. 获取 Bot Token

# 4. 配置 OpenClaw
openclaw config set channels.telegram.botToken "123456:ABC-xxxxx"

# 或通过引导向导
openclaw channels add telegram
```

### 群组使用

Telegram Bot 支持群组，通过 @提及 激活：

```
@your_bot_name 帮我查一下明天的天气
```

## Discord

Discord 集成基于 discord.js，支持频道消息和语音流。

### 配置步骤

```bash
# 1. 去 Discord Developer Portal 创建应用
# https://discord.com/developers/applications

# 2. 创建 Bot，获取 Token
# 3. 邀请 Bot 到你的服务器

# 4. 配置 OpenClaw
openclaw config set channels.discord.botToken "xxxxx"
openclaw config set channels.discord.guildId "xxxxx"
```

### v2026.2.21 新功能：语音流

Discord 现在支持语音频道流式传输，AI 可以在语音频道中实时对话：

```json
{
  "channels": {
    "discord": {
      "voice": {
        "enabled": true,
        "autoJoin": true
      }
    }
  }
}
```

## Slack

```bash
# 1. 创建 Slack App
# https://api.slack.com/apps

# 2. 配置 Bot Token
openclaw config set channels.slack.botToken "xoxb-xxxxx"
openclaw config set channels.slack.appToken "xapp-xxxxx"
```

## 飞书 (Feishu)

对中国用户特别友好：

```bash
openclaw channels add feishu

# 需要配置
# - App ID
# - App Secret
# - Verification Token
```

## Signal

```bash
# 需要先安装 signal-cli
# https://github.com/AsamK/signal-cli

openclaw channels add signal
```

## iMessage（仅 macOS）

```bash
# 需要 macOS 系统
# 使用本地 imsg CLI 工具
openclaw channels add imessage
```

## 其他平台

| 平台 | 配置命令 |
|------|----------|
| Google Chat | `openclaw channels add googlechat` |
| Microsoft Teams | `openclaw channels add msteams` |
| Matrix | `openclaw channels add matrix` |
| Mattermost | 通过插件安装 |
| IRC | `openclaw channels add irc` |
| LINE | `openclaw channels add line` |
| Twitch | `openclaw channels add twitch` |

## 消息路由

当你连接了多个平台，OpenClaw 会智能路由消息：

- **私聊** — 合并到同一个 `main` 会话
- **群组** — 每个群组独立会话
- **@提及** — 群组中通过 @提及 激活 AI

### 配置路由规则

```json
{
  "channels": {
    "routing": {
      "directMessages": "shared",
      "groupMessages": "isolated",
      "mentionRequired": true
    }
  }
}
```

## 配对与安全

首次收到陌生人消息时，OpenClaw 会要求配对确认：

```bash
# 查看待配对列表
openclaw pairing list

# 批准配对
openclaw pairing approve +86138xxxx0000

# 拒绝配对
openclaw pairing reject +86138xxxx0000
```

## 广播消息

向多个联系人发送同一条消息：

```bash
openclaw message broadcast --group "team" --message "今天下午3点开会"
```

## 下一步

平台连好了！去 [06. 技能系统](06-skills.md) 解锁 AI 的超能力！
