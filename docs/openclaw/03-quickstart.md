# 03. 快速开始

## 5 分钟跑起第一个对话

安装完 OpenClaw 后，只需要 3 步就能开始聊天。

### 第一步：运行引导向导

```bash
openclaw onboard --install-daemon
```

向导会引导你完成：
- 选择 AI 模型提供商（OpenAI / Anthropic / Ollama 等）
- 输入 API Key
- 配置 Gateway 设置
- 可选：连接消息平台

跟着提示一步步来就行，不懂的直接回车用默认值。

### 第二步：检查 Gateway 状态

```bash
openclaw gateway status
```

看到 `running` 就说明网关已经启动了。

### 第三步：打开控制面板

```bash
openclaw dashboard
```

浏览器会自动打开 `http://127.0.0.1:18789/`，这就是你的 AI 助手控制面板。

直接在输入框里打字，开始和 AI 对话！

## 发送第一条测试消息

如果你已经配置了消息平台（比如 WhatsApp），可以用命令行发送测试消息：

```bash
openclaw message send --target +86138xxxx0000 --message "你好，我是你的 AI 助手！"
```

## 控制面板功能

打开 `http://127.0.0.1:18789/` 后，你会看到：

- **聊天界面** — 直接和 AI 对话
- **设置面板** — 配置模型、平台、技能
- **会话管理** — 查看所有对话历史
- **系统状态** — 监控 Gateway 运行状态

## 前台运行（调试用）

如果想看到详细日志，可以在前台运行 Gateway：

```bash
openclaw gateway --port 18789
```

这样所有日志都会直接输出到终端，方便排查问题。

## 环境变量

如果需要自定义配置路径：

```bash
# 设置 OpenClaw 主目录
export OPENCLAW_HOME=/path/to/your/home

# 设置状态目录
export OPENCLAW_STATE_DIR=/path/to/state

# 设置配置文件路径
export OPENCLAW_CONFIG_PATH=/path/to/openclaw.json
```

默认路径：
- 配置文件：`~/.openclaw/openclaw.json`
- 工作空间：`~/.openclaw/workspace`
- 状态目录：`~/.openclaw/`

## 目录结构

安装完成后，OpenClaw 的文件结构：

```
~/.openclaw/
├── openclaw.json          # 主配置文件
├── workspace/             # 工作空间（AI 的文件操作区域）
│   ├── MEMORY.md          # 长期记忆
│   ├── memory/            # 每日记忆日志
│   │   └── 2026-02-25.md
│   ├── SOUL.md            # AI 人格设定
│   ├── AGENTS.md          # Agent 配置
│   └── USER.md            # 用户信息
├── agents/                # 多 Agent 配置
│   └── main/
│       ├── agent/
│       │   └── auth-profiles.json
│       └── sessions/      # 会话存储
├── skills/                # 共享技能目录
└── extensions/            # 扩展插件
```

## 下一步

对话跑起来了！去 [04. AI 模型配置](04-model-setup.md) 学习如何接入更多 AI 模型！
