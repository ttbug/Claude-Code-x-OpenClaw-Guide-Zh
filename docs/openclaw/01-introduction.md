# 01. OpenClaw 项目介绍

## 一句话说清楚

OpenClaw 是一个**开源的 AI 私人助手框架**，跑在你自己的电脑或服务器上，能连接 WhatsApp、Telegram、Discord 等 30+ 消息平台，让 AI 帮你处理消息、执行任务、管理日程。

你可以把它理解为：**一个 7×24 小时在线的 AI 员工，通过你常用的聊天软件跟你沟通。**

## 发展历史

OpenClaw 的故事堪称开源传奇：

```
2025年11月  Peter Steinberger（iOS 开发者，PSPDFKit 创始人）
            周末写了个小项目，把 AI 模型接到消息应用上
                    ↓
2025年11月  项目命名为 Clawdbot，开源发布
            3 周内从 0 涨到 123K GitHub Stars
                    ↓
2026年1月27日  Anthropic 要求商标变更
              项目更名为 Moltbot
                    ↓
2026年1月30日  社区投票，最终定名 OpenClaw
              "Open" 强调开源，"Claw" 致敬龙虾钳子 🦞
                    ↓
2026年2月    GitHub Stars 突破 224K
             成为史上增长最快的开源项目之一
             累计数百万次安装
```

## 核心架构

OpenClaw 的架构设计非常优雅，核心就三层：

```
┌─────────────────────────────────────────────────┐
│                  消息平台层                        │
│  WhatsApp │ Telegram │ Discord │ Slack │ ...     │
└──────────────────────┬──────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────┐
│              Gateway（网关守护进程）                │
│                                                   │
│  ┌─────────┐  ┌──────────┐  ┌────────────────┐  │
│  │ 路由引擎 │  │ 会话管理  │  │ WebSocket API  │  │
│  └─────────┘  └──────────┘  └────────────────┘  │
│                                                   │
│  ┌─────────┐  ┌──────────┐  ┌────────────────┐  │
│  │ 技能系统 │  │ 记忆系统  │  │ 安全沙箱       │  │
│  └─────────┘  └──────────┘  └────────────────┘  │
└──────────────────────┬──────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────┐
│                AI 模型提供商层                     │
│  OpenAI │ Anthropic │ Gemini │ Ollama │ ...     │
└─────────────────────────────────────────────────┘
```

### Gateway（网关）

Gateway 是 OpenClaw 的心脏，一个长期运行的守护进程：

- 管理所有消息平台的连接（WhatsApp 用 Baileys，Telegram 用 grammY，等等）
- 通过 WebSocket 暴露 API（默认端口 `127.0.0.1:18789`）
- 处理消息路由、会话管理、Agent 调度

### Agent Loop（代理循环）

每次收到消息，OpenClaw 执行一个完整的 Agent 循环：

```
消息接收 → 上下文组装 → 模型推理 → 工具执行 → 流式回复 → 状态持久化
```

关键特性：
- 每个会话串行执行，防止竞态条件
- 支持工具调用（读写文件、执行命令等）
- 流式输出，实时看到 AI 的回复

### 客户端

连接 Gateway 的方式有很多：

- **macOS 菜单栏应用** — 原生体验
- **CLI 命令行** — 开发者最爱
- **Web 控制面板** — 浏览器直接用
- **iOS / Android 节点** — 手机端
- **消息平台** — WhatsApp、Telegram 等

## 支持的消息平台（30+）

| 平台 | 集成方式 | 状态 |
|------|----------|------|
| WhatsApp | Baileys (Web) | ✅ 稳定 |
| Telegram | grammY Bot | ✅ 稳定 |
| Discord | discord.js | ✅ 稳定 |
| Slack | Slack API | ✅ 稳定 |
| iMessage | imsg CLI (macOS) | ✅ 稳定 |
| Signal | Signal CLI | ✅ 稳定 |
| 飞书 (Feishu) | 飞书 Bot | ✅ 稳定 |
| Google Chat | Google API | ✅ 稳定 |
| Microsoft Teams | MS Graph | ✅ 稳定 |
| Matrix | Matrix SDK | ✅ 稳定 |
| Mattermost | 插件 | ✅ 稳定 |
| IRC | IRC 协议 | ✅ 稳定 |
| LINE | LINE API | ✅ 稳定 |
| Twitch | Twitch IRC | ✅ 稳定 |
| Nostr | Nostr 协议 | ✅ 稳定 |
| Zalo | Zalo API | ✅ 稳定 |
| Synology Chat | Synology API | ✅ 稳定 |
| Nextcloud Talk | Nextcloud API | ✅ 稳定 |
| Tlon | Tlon API | ✅ 稳定 |

## 支持的 AI 模型提供商（29 个）

| 提供商 | 代表模型 | 特点 |
|--------|----------|------|
| OpenAI | GPT-5.3, GPT-4o | 最强通用能力 |
| Anthropic | Claude Opus 4.6, Sonnet 4.6 | 最强编程能力 |
| Google | Gemini 3.1 | 多模态强 |
| Ollama | Llama, Qwen, Mistral | 本地运行，隐私优先 |
| Moonshot/Kimi | Moonshot 视觉 | 中文优化，视觉能力 |
| Mistral | Mistral Large | 欧洲开源模型 |
| xAI | Grok | Elon Musk 的 AI |
| 通义千问 (Qwen) | Qwen-Max | 阿里巴巴，中文强 |
| 百度千帆 (Qianfan) | ERNIE | 百度，中文优化 |
| 智谱 (GLM) | GLM-4 | 清华系，中文强 |
| 小米 (Xiaomi) | MiLM | 小米 AI |
| MiniMax | abab6 | 国产多模态 |
| DeepGram | 语音识别 | 语音转文字 |
| NVIDIA | NIM | GPU 加速推理 |
| Together AI | 开源模型托管 | 便宜好用 |
| OpenRouter | 多模型路由 | 一个 Key 用所有模型 |
| Venice AI | 隐私优先 | 不记录数据 |
| HuggingFace | 开源模型 | 社区模型 |
| vLLM | 本地推理 | 高性能本地部署 |
| LiteLLM | 统一接口 | 代理层 |
| AWS Bedrock | Claude, Titan | 企业级 |
| Vercel AI Gateway | 多模型 | Vercel 生态 |
| Cloudflare AI Gateway | 多模型 | CDN 加速 |
| GitHub Copilot | Copilot | 编程辅助 |
| Z.AI | Z.AI 模型 | 新兴平台 |
| KiloCode | Kilo 模型 | 编程优化 |

## 50+ 内置技能一览

OpenClaw 的技能系统是它的杀手锏。技能 = 教 AI 如何组合工具完成任务：

| 技能 | 功能 |
|------|------|
| `gog` | Google Workspace（邮件、日历、文档） |
| `obsidian` | Obsidian 笔记管理 |
| `github` / `gh-issues` | GitHub 仓库和 Issue 管理 |
| `discord` | Discord 服务器管理 |
| `slack` | Slack 工作区管理 |
| `notion` | Notion 页面管理 |
| `trello` | Trello 看板管理 |
| `spotify-player` | Spotify 音乐控制 |
| `weather` | 天气查询 |
| `coding-agent` | 编程助手 |
| `voice-call` | 语音通话 |
| `summarize` | 内容摘要 |
| `skill-creator` | 创建自定义技能 |
| `1password` | 密码管理 |
| `apple-notes` | Apple 备忘录 |
| `apple-reminders` | Apple 提醒事项 |
| `bear-notes` | Bear 笔记 |
| `things-mac` | Things 任务管理 |
| `tmux` | 终端会话管理 |
| `peekaboo` | 屏幕截图 |
| `camsnap` | 摄像头拍照 |
| `video-frames` | 视频帧提取 |
| `nano-pdf` | PDF 处理 |
| `healthcheck` | 系统健康检查 |
| `session-logs` | 会话日志 |
| `model-usage` | 模型使用统计 |
| ... | 还有更多！ |

## 下一步

准备好了吗？去 [02. 环境安装](02-installation.md) 开始安装 OpenClaw！
