# 08. 多 Agent 路由

## 什么是多 Agent？

一个 OpenClaw Gateway 可以同时运行多个独立的 AI 助手（Agent），每个 Agent 有自己的：

- **工作空间** — 独立的文件和记忆
- **状态目录** — 独立的认证和配置
- **会话存储** — 独立的对话历史
- **技能** — 独立的技能集

## 使用场景

- **工作 Agent** — 处理邮件、日程、项目管理
- **编程 Agent** — 专注写代码、管理 GitHub
- **社交 Agent** — 管理社交媒体消息
- **家庭 Agent** — 智能家居、购物清单

## 目录结构

```
~/.openclaw/
├── openclaw.json              # 主配置
├── agents/
│   ├── main/                  # 默认 Agent
│   │   ├── agent/
│   │   │   └── auth-profiles.json
│   │   └── sessions/
│   ├── coding/                # 编程 Agent
│   │   ├── agent/
│   │   │   └── auth-profiles.json
│   │   └── sessions/
│   └── social/                # 社交 Agent
│       ├── agent/
│       │   └── auth-profiles.json
│       └── sessions/
├── workspace/                 # main Agent 的工作空间
├── workspace-coding/          # coding Agent 的工作空间
└── workspace-social/          # social Agent 的工作空间
```

## 快速开始

### 创建新 Agent

```bash
# 使用向导创建
openclaw agents add coding
openclaw agents add social

# 每个 Agent 会自动创建：
# - 独立的工作空间（SOUL.md, AGENTS.md, USER.md）
# - 独立的 agentDir 和会话存储
```

### 配置消息路由

通过 bindings 把消息平台的消息路由到不同 Agent：

```json
{
  "agents": {
    "list": [
      {
        "agentId": "main",
        "workspace": "~/.openclaw/workspace"
      },
      {
        "agentId": "coding",
        "workspace": "~/.openclaw/workspace-coding",
        "bindings": [
          {
            "channel": "discord",
            "guildId": "123456",
            "channelId": "789012"
          }
        ]
      },
      {
        "agentId": "social",
        "workspace": "~/.openclaw/workspace-social",
        "bindings": [
          {
            "channel": "telegram",
            "chatId": "-100xxxxx"
          }
        ]
      }
    ]
  }
}
```

### 查看 Agent 列表

```bash
openclaw agents list --bindings
```

## Agent 人格设定

每个 Agent 的工作空间里有一个 `SOUL.md` 文件，定义 AI 的人格：

```markdown
# coding Agent

你是一个专业的编程助手。

## 性格
- 严谨、精确
- 代码优先，少说废话
- 遵循最佳实践

## 专长
- TypeScript / Python / Go
- 系统架构设计
- 代码审查

## 规则
- 所有代码必须有类型注解
- 遵循 SOLID 原则
- 不写没有测试的代码
```

## 认证隔离

每个 Agent 的认证是独立的：

```
~/.openclaw/agents/<agentId>/agent/auth-profiles.json
```

主 Agent 的凭证不会自动共享给其他 Agent。如果需要共享，手动复制 `auth-profiles.json`。

## 会话路由规则

- **私聊** — 默认路由到 `main` Agent
- **群组** — 根据 bindings 配置路由
- **未匹配** — 回退到默认 Agent

## 沙箱隔离

每个 Agent 的工作空间是默认工作目录，但不是硬沙箱。如果需要严格隔离：

```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "enabled": true,
        "type": "docker"
      }
    }
  }
}
```

启用 Docker 沙箱后，Agent 的工具执行（文件操作、命令执行）会在 Docker 容器中运行。

## 下一步

多 Agent 搞定！去 [09. Docker 部署](09-docker.md) 学习容器化部署！
