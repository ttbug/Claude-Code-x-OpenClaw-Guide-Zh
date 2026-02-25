# 11. 常见问题 (FAQ)

## 安装问题

### Q: `openclaw` 命令找不到

```bash
# 检查是否安装成功
npm list -g openclaw

# 检查 PATH
echo $PATH | tr ':' '\n' | grep npm

# 修复：添加 npm 全局目录到 PATH
export PATH="$(npm config get prefix)/bin:$PATH"
```

### Q: Node.js 版本不对

```bash
# 检查版本
node --version

# 需要 22+，用 nvm 管理版本
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
nvm install 22
nvm use 22
```

### Q: Docker 安装失败

```bash
# 检查 Docker 是否运行
docker info

# 清理重试
docker system prune -f
./docker-setup.sh
```

## 连接问题

### Q: WhatsApp 扫码后断开

- 确保 Gateway 持续运行（用 `--install-daemon` 安装为服务）
- 检查网络连接
- 尝试重新配对：`openclaw channels reconnect whatsapp`

### Q: Telegram Bot 不响应

- 检查 Bot Token 是否正确
- 确保 Bot 已被添加到群组
- 群组中需要 @提及 Bot 才会响应
- 检查 Gateway 日志：`openclaw gateway logs`

### Q: 控制面板打不开

```bash
# 检查 Gateway 是否运行
openclaw gateway status

# 检查端口
lsof -i :18789  # macOS/Linux
netstat -an | findstr 18789  # Windows

# 重启 Gateway
openclaw gateway restart
```

## 模型问题

### Q: API 调用报错 401

- API Key 过期或无效
- 检查余额是否充足
- 重新设置 Key：`openclaw config set providers.openai.apiKey "new-key"`

### Q: 回复很慢

- 检查网络到 API 服务器的延迟
- 尝试切换模型（小模型更快）
- 启用模型故障转移
- 本地模型（Ollama）延迟最低

### Q: Ollama 本地模型连不上

```bash
# 确保 Ollama 在运行
ollama serve

# 检查端口
curl http://127.0.0.1:11434/api/tags

# 配置 OpenClaw
openclaw config set providers.ollama.baseUrl "http://127.0.0.1:11434"
```

## 记忆问题

### Q: AI 不记得之前说的话

- 检查记忆文件是否存在：`ls ~/.openclaw/workspace/memory/`
- 主动让 AI 记录："记住这个"
- 检查 `memoryFlush` 是否启用
- 长对话后记忆可能被压缩，重要信息要写入 `MEMORY.md`

### Q: 记忆文件太大

- 定期清理旧的每日日志
- 精简 `MEMORY.md`，删除过时信息
- 设置自动清理：

```json
{
  "agents": {
    "defaults": {
      "memory": {
        "retentionDays": 30
      }
    }
  }
}
```

## 性能问题

### Q: Gateway 占用内存高

- 检查连接的平台数量，减少不必要的连接
- 重启 Gateway：`openclaw gateway restart`
- 检查是否有大量未处理的会话

### Q: 磁盘空间不足

```bash
# 检查 OpenClaw 占用
du -sh ~/.openclaw/

# 清理会话日志
openclaw sessions prune --older-than 30d

# 清理 Docker（如果使用）
docker system prune -f
```

## 更新问题

### Q: 更新后配置丢失

- OpenClaw 的配置在 `~/.openclaw/` 目录，更新不会覆盖
- 如果用 Docker，确保挂载了配置目录
- 建议更新前备份：`cp -r ~/.openclaw ~/.openclaw.backup`

### Q: 更新后功能异常

```bash
# 检查版本
openclaw --version

# 查看 changelog
# https://github.com/openclaw/openclaw/releases

# 回滚到上一版本
npm install -g openclaw@previous-version
```

## 中国用户特别提示

### Q: 安装脚本下载慢

```bash
# 使用镜像
npm config set registry https://registry.npmmirror.com
npm install -g openclaw@latest
```

### Q: API 访问不了

- OpenAI / Anthropic / Google 需要代理
- 推荐使用 OpenRouter（一个 Key 用所有模型）
- 或使用国产模型：通义千问、Moonshot/Kimi、智谱 GLM
- Ollama 本地模型完全不需要网络

### Q: WhatsApp 在中国能用吗？

- WhatsApp 在中国需要代理
- 建议用 Telegram 或飞书替代
- 飞书 (Feishu) 是 OpenClaw 原生支持的中国平台

## 获取帮助

- **GitHub Issues**: [github.com/openclaw/openclaw/issues](https://github.com/openclaw/openclaw/issues)
- **Discord 社区**: 加入 OpenClaw Discord 服务器
- **官方文档**: [docs.openclaw.ai](https://docs.openclaw.ai)

---

> 🦞 教程到这里就结束了！如果这份教程帮到了你，给个 Star 支持一下！
