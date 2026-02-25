# 04. AI 模型配置

## 概述

OpenClaw 支持 29 个 AI 模型提供商，从云端大模型到本地开源模型，总有一款适合你。

配置模型最简单的方式：

```bash
openclaw onboard
```

引导向导会帮你配置。但如果你想手动配置或切换模型，往下看。

## 云端模型（按推荐度排序）

### OpenAI

```bash
# 设置 API Key
openclaw config set providers.openai.apiKey "sk-proj-xxxxx"

# 或用环境变量
export OPENAI_API_KEY="sk-proj-xxxxx"
```

支持的模型：GPT-5.3、GPT-5.3 Codex、GPT-4o、GPT-4o-mini 等。

获取 API Key：[platform.openai.com](https://platform.openai.com/)

### Anthropic (Claude)

```bash
openclaw config set providers.anthropic.apiKey "sk-ant-xxxxx"

# 或环境变量
export ANTHROPIC_API_KEY="sk-ant-xxxxx"
```

支持的模型：Claude Opus 4.6、Claude Sonnet 4.6、Claude Haiku 等。

获取 API Key：[console.anthropic.com](https://console.anthropic.com/)

### Google Gemini

```bash
openclaw config set providers.google.apiKey "AIzaSy-xxxxx"

# 或环境变量
export GOOGLE_API_KEY="AIzaSy-xxxxx"
```

支持的模型：Gemini 3.1、Gemini 2.5 Pro 等。

### Moonshot / Kimi（中国用户推荐）

```bash
openclaw config set providers.moonshot.apiKey "sk-xxxxx"
```

v2026.2.23 新增视觉和视频能力，中文理解能力强。

### 通义千问 (Qwen)

```bash
openclaw config set providers.qwen.apiKey "sk-xxxxx"
```

阿里巴巴出品，中文能力优秀，性价比高。

### MistralAI

```bash
openclaw config set providers.mistral.apiKey "xxxxx"
```

v2026.2.22 新增集成，支持聊天、记忆和语音功能。

## 本地模型（隐私优先）

### Ollama（强烈推荐）

Ollama 让你在本地跑开源模型，数据完全不出你的电脑：

```bash
# 1. 安装 Ollama
curl -fsSL https://ollama.com/install.sh | sh

# 2. 下载模型
ollama pull llama3.1
ollama pull qwen2.5

# 3. 配置 OpenClaw 使用 Ollama
openclaw config set providers.ollama.baseUrl "http://127.0.0.1:11434"
```

推荐本地模型：
- `llama3.1` — Meta 的开源模型，综合能力强
- `qwen2.5` — 通义千问，中文最强开源模型
- `mistral` — Mistral 7B，轻量高效
- `codellama` — 编程专用

### vLLM

高性能本地推理引擎，适合有 GPU 的用户：

```bash
openclaw config set providers.vllm.baseUrl "http://127.0.0.1:8000"
```

## 多模型路由

### OpenRouter（一个 Key 用所有模型）

```bash
openclaw config set providers.openrouter.apiKey "sk-or-xxxxx"
```

OpenRouter 是模型路由器，一个 API Key 就能访问 OpenAI、Anthropic、Google 等所有模型。适合想要灵活切换模型的用户。

获取 API Key：[openrouter.ai](https://openrouter.ai/)

## 模型故障转移

OpenClaw 支持自动模型故障转移。当主模型不可用时，自动切换到备用模型：

```json
{
  "agents": {
    "defaults": {
      "model": "anthropic:claude-sonnet-4-6",
      "modelFallback": [
        "openai:gpt-4o",
        "ollama:llama3.1"
      ]
    }
  }
}
```

## 订阅认证（Claude Max / ChatGPT Plus）

如果你有 Claude Max 或 ChatGPT Plus 订阅，可以通过 OAuth 直接使用：

```bash
# 配置订阅认证
openclaw config set providers.anthropic.auth "subscription"
```

## 预算控制

担心 API 费用？设置使用限制：

```json
{
  "agents": {
    "defaults": {
      "usageTracking": {
        "enabled": true,
        "dailyLimit": 5.00,
        "monthlyLimit": 50.00,
        "currency": "USD"
      }
    }
  }
}
```

## 模型选择建议

| 场景 | 推荐模型 | 理由 |
|------|----------|------|
| 日常对话 | GPT-4o-mini / Haiku | 便宜快速 |
| 复杂推理 | Claude Opus 4.6 | 最强推理 |
| 编程辅助 | Claude Sonnet 4.6 | 编程最强 |
| 中文场景 | Qwen-Max / Moonshot | 中文优化 |
| 隐私优先 | Ollama + Llama3.1 | 数据不出本地 |
| 多模态 | Gemini 3.1 | 图片视频理解 |
| 预算有限 | OpenRouter | 按需选模型 |

## 下一步

模型配好了！去 [05. 消息平台集成](05-channels.md) 连接你的聊天软件！
