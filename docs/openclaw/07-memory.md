# 07. 记忆系统

## 核心概念

OpenClaw 的记忆系统非常简洁：**纯 Markdown 文件**。

没有复杂的向量数据库，没有花哨的 RAG 管道。AI 的记忆就是写在磁盘上的 Markdown 文件，模型只"记住"写到磁盘上的内容。

## 记忆文件结构

```
~/.openclaw/workspace/
├── MEMORY.md              # 长期记忆（精选的重要信息）
└── memory/
    ├── 2026-02-25.md      # 今天的日志
    ├── 2026-02-24.md      # 昨天的日志
    └── ...                # 更早的日志
```

### 两层记忆

| 层级 | 文件 | 用途 | 加载时机 |
|------|------|------|----------|
| 长期记忆 | `MEMORY.md` | 精选的持久信息（偏好、决策、重要事实） | 主会话启动时 |
| 每日日志 | `memory/YYYY-MM-DD.md` | 当天的笔记和上下文 | 每次会话加载今天+昨天 |

## 记忆工具

AI 有两个记忆工具：

- `memory_search` — 语义搜索，从所有记忆中找相关内容
- `memory_get` — 精确读取某个记忆文件的特定内容

## 什么时候写记忆？

- **决策和偏好** → 写入 `MEMORY.md`
  - "我喜欢用 Claude 而不是 GPT"
  - "项目用 TypeScript + React"
  - "每周一上午 10 点开周会"

- **日常笔记** → 写入 `memory/YYYY-MM-DD.md`
  - "今天和老李讨论了新功能"
  - "发现了一个 bug，明天修"

- **用户说"记住这个"** → 立刻写入记忆文件

## 主动让 AI 记住

直接告诉 AI：

```
记住：我的 GitHub 用户名是 laojin-dev
记住：项目部署在 Hetzner 的 VPS 上，IP 是 xxx.xxx.xxx.xxx
记住：我不喜欢在代码里用分号
```

AI 会自动把这些写入 `MEMORY.md`。

## 自动记忆刷新（防丢失）

OpenClaw 有一个聪明的机制：当会话快要被压缩（compaction）时，会自动触发一个隐藏的 Agent 轮次，提醒 AI 把重要信息写入记忆文件。

配置：

```json
{
  "agents": {
    "defaults": {
      "compaction": {
        "reserveTokensFloor": 20000,
        "memoryFlush": {
          "enabled": true,
          "softThresholdTokens": 4000,
          "systemPrompt": "Session nearing compaction. Store durable memories now.",
          "prompt": "Write any lasting notes to memory/YYYY-MM-DD.md; reply with NO_REPLY if nothing to store."
        }
      }
    }
  }
}
```

这意味着：即使对话很长，重要信息也不会丢失。

## 记忆插件

默认使用 `memory-core` 插件。如果不需要记忆功能：

```json
{
  "plugins": {
    "slots": {
      "memory": "none"
    }
  }
}
```

## 多语言记忆（v2026.2.22 新功能）

v2026.2.22 新增多语言记忆支持：

- 西班牙语
- 葡萄牙语
- 日语
- 韩语
- 阿拉伯语

AI 可以用这些语言写入和搜索记忆。

## 记忆最佳实践

1. **定期整理 MEMORY.md** — 删除过时信息，保持精简
2. **让 AI 主动记录** — 重要对话后说"把今天讨论的要点记下来"
3. **分类存储** — 在 MEMORY.md 中用标题分类（工作、项目、偏好等）
4. **不要存敏感信息** — 密码、API Key 等不要写入记忆文件

## 下一步

记忆系统搞定！去 [08. 多 Agent 路由](08-multi-agent.md) 学习如何运行多个 AI 助手！
