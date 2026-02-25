# 06. 技能系统 (Skills)

## 什么是技能？

如果说工具 (Tools) 是 AI 的双手，那技能 (Skills) 就是教 AI 如何组合使用这些工具的教科书。

OpenClaw 内置 50+ 技能，覆盖办公、开发、生活方方面面。

## 技能 vs 工具

```
工具 (Tools) = 单个动作（读文件、发消息、搜索）
技能 (Skills) = 组合工具完成任务的知识（管理 GitHub Issue、整理 Obsidian 笔记）
```

举个例子：
- `gog` 技能教 AI 如何使用 Google Workspace 工具来管理邮件和日历
- `obsidian` 技能教 AI 如何组织和搜索 Obsidian 笔记
- `github` 技能教 AI 如何管理仓库、创建 PR、处理 Issue

## 查看已安装技能

```bash
# 列出所有技能
openclaw skills list

# 查看技能详情
openclaw skills info gog
```

## 常用技能详解

### 办公效率类

| 技能 | 功能 | 使用场景 |
|------|------|----------|
| `gog` | Google Workspace | 邮件、日历、文档管理 |
| `notion` | Notion | 页面创建、数据库查询 |
| `trello` | Trello | 看板管理、任务跟踪 |
| `slack` | Slack | 工作区消息管理 |
| `1password` | 1Password | 密码查询（只读） |

### 开发工具类

| 技能 | 功能 | 使用场景 |
|------|------|----------|
| `coding-agent` | 编程助手 | 写代码、调试、重构 |
| `github` | GitHub | 仓库管理、PR、Issue |
| `gh-issues` | GitHub Issues | Issue 专项管理 |
| `tmux` | 终端会话 | 管理终端会话 |

### 笔记知识类

| 技能 | 功能 | 使用场景 |
|------|------|----------|
| `obsidian` | Obsidian | 笔记管理、知识库 |
| `apple-notes` | Apple 备忘录 | macOS/iOS 备忘录 |
| `bear-notes` | Bear | Bear 笔记管理 |
| `nano-pdf` | PDF 处理 | PDF 阅读、摘要 |

### 多媒体类

| 技能 | 功能 | 使用场景 |
|------|------|----------|
| `voice-call` | 语音通话 | AI 语音对话 |
| `spotify-player` | Spotify | 音乐播放控制 |
| `peekaboo` | 截图 | 屏幕截图分析 |
| `camsnap` | 摄像头 | 拍照识别 |
| `video-frames` | 视频帧 | 视频内容分析 |
| `songsee` | 听歌识曲 | 识别正在播放的音乐 |

### 生活工具类

| 技能 | 功能 | 使用场景 |
|------|------|----------|
| `weather` | 天气 | 天气查询和预报 |
| `things-mac` | Things | 任务管理 (macOS) |
| `apple-reminders` | 提醒事项 | Apple 提醒事项 |
| `goplaces` | 地点 | 地点搜索和导航 |
| `healthcheck` | 健康检查 | 系统状态监控 |

### 系统管理类

| 技能 | 功能 | 使用场景 |
|------|------|----------|
| `session-logs` | 会话日志 | 查看对话历史 |
| `model-usage` | 模型统计 | API 使用量统计 |
| `summarize` | 摘要 | 长文本摘要 |
| `skill-creator` | 技能创建器 | 创建自定义技能 |

## 安装新技能

```bash
# 从 ClawHub 安装（技能市场）
openclaw skills install skill-name

# 从 GitHub 安装
openclaw skills install github:username/skill-name
```

## 创建自定义技能

这是 OpenClaw 最强大的功能之一。你可以教 AI 新的能力：

### 方法一：用 skill-creator 技能

直接告诉 AI：

```
帮我创建一个技能，用来管理我的读书笔记。
每次我说"记录读书笔记"，就创建一个新的 Markdown 文件，
包含书名、作者、日期、关键摘录和我的感想。
```

AI 会自动使用 `skill-creator` 技能帮你生成。

### 方法二：手动创建

在工作空间的 `skills/` 目录下创建：

```
~/.openclaw/workspace/skills/my-skill/
├── skill.md          # 技能描述和指令
├── tools/            # 自定义工具（可选）
│   └── my-tool.ts
└── examples/         # 使用示例（可选）
    └── example.md
```

`skill.md` 示例：

```markdown
---
name: reading-notes
description: 管理读书笔记
triggers:
  - 记录读书笔记
  - 读书笔记
---

# 读书笔记管理

当用户要求记录读书笔记时：

1. 询问书名和作者
2. 在 workspace/reading-notes/ 目录下创建 Markdown 文件
3. 文件名格式：YYYY-MM-DD-书名.md
4. 包含：书名、作者、日期、关键摘录、个人感想
5. 更新 reading-notes/INDEX.md 索引
```

### 技能的作用域

- **工作空间技能**：`~/.openclaw/workspace/skills/` — 当前 Agent 专用
- **共享技能**：`~/.openclaw/skills/` — 所有 Agent 共享

## 下一步

技能系统掌握了！去 [07. 记忆系统](07-memory.md) 了解 AI 如何记住你！
