# OpenClaw 常见问题解答 (FAQ)

> 整理自官方文档、社区讨论和用户实战经验

## 📌 部署相关问题

### Q1: OpenClaw 支持哪些部署方式？

**A:** OpenClaw 支持三种主要部署方式：

| 部署方式 | 优势 | 适用场景 | 难度 |
|---------|------|---------|------|
| **阿里云部署** | 7×24 小时稳定运行、多用户共享、公网访问 | 团队协作、长期自动化任务 | ⭐⭐ 中等 |
| **本地部署** | 数据私有化、零服务器成本、低延迟 | 个人办公、敏感数据处理 | ⭐⭐ 中等 |
| **MaxClaw 云端** | 零门槛、免配置、内置工具 | 小白用户、快速上手 | ⭐ 极低 |

**推荐阅读：** [阿里云部署指南](./deployment/aliyun-quickstart.md) | [本地部署指南](./deployment/local-setup.md)

---

### Q2: 部署 OpenClaw 需要什么配置？

**A:** 最低配置要求：

**阿里云轻量应用服务器：**
- 内存：2GiB 及以上（推荐 4GiB）
- CPU: 2vCPU
- 存储：40GB+
- 系统：Alibaba Cloud Linux 3 / Ubuntu 20.04+
- **费用参考：** 68 元/年起（新人专享）

**本地部署：**
- 系统：Windows 10+ / macOS 12+ / Linux (Ubuntu 20.04+)
- Node.js: 18.0.0+
- npm: 8.0.0+
- 内存：建议 4GB+

---

### Q3: 如何配置 API 密钥？

**A:** OpenClaw 支持多种模型 API：

**最新版本**: 2026.2.23（2026 年 3 月发布）

**安全提示**: 此版本包含关键安全补丁，修复了 WebSocket 劫持漏洞 (CVE-2026-25253)，强烈建议更新！

**推荐的 Coding Plan 套餐（成本可控）：**
- 支持模型：qwen3.5-plus、kimi-k2.5、MiniMax-M2.5-highspeed、glm-5
- 计费方式：固定月费，超出限额不计费
- 配置方法：
```bash
openclaw config set llm.apiKey "你的 Coding Plan API Key"
```

**按 Token 计费：**
```bash
openclaw config set llm.apiKey "你的按量付费 API Key"
openclaw config set llm.model "qwen3.5-plus"
```

**注意：** API Key 对应地域需与服务器地域匹配，否则会导致模型调用失败。

---

## 🔌 频道集成问题

### Q4: 如何集成飞书（Feishu）？

**A:** 两种方式：

**方式 1：使用 OpenClaw 插件（推荐，镜像版本 2026.2.9+）**

1. **创建飞书应用**
   - 访问 [飞书开放平台](https://open.feishu.cn/app)
   - 创建企业自建应用，复制 App ID 和 App Secret
   
2. **配置权限**
   ```json
   {
     "scopes": {
       "tenant": [
         "im:message",
         "im:message:send_as_bot",
         "im:chat",
         "contact:user.employee_id:readonly"
       ]
     }
   }
   ```

3. **在 OpenClaw 配置**
   ```bash
   openclaw config set channels.feishu.appId "cli_xxx"
   openclaw config set channels.feishu.appSecret "xxx"
   ```

4. **配对机器人**
   - 在飞书中@机器人发送消息，获取配对码
   - 在 WebUI 输入：`openclaw pairing approve feishu <配对码>`

**方式 2：使用 AppFlow 集成**

详细步骤见：[飞书集成完整教程](./channels/feishu-integration.md)

---

### Q5: 飞书 API 调用次数耗尽怎么办？

**A:** 这是常见问题！飞书普通账号每月限额 10000 次 API 调用。

**原因：** 官方插件默认每分钟调用一次 `/open-apis/bot/v3/info` 接口，24 小时调用 1440 次，几天就会耗尽。

**解决方案：**

1. **优化轮询频率**
   ```bash
   openclaw config set channels.feishu.pollInterval 300  # 改为 5 分钟一次
   ```

2. **使用长连接接收事件**
   - 在飞书开放平台选择"使用长连接接收事件"
   - 减少主动轮询次数

3. **升级飞书企业认证**
   - 企业认证后 API 限额更高

4. **使用 Webhook 模式**
   - 配置事件订阅，仅在收到消息时触发

**详细教程：** [飞书 API 优化指南](./channels/feishu-api-optimization.md)

---

### Q6: 为什么 OpenClaw 连接不了浏览器？

**A:** OpenClaw 使用 CDP (Chrome DevTools Protocol) 连接浏览器。

**解决步骤：**

1. **安装 OpenClaw Browser Relay 插件**
   - 在 Chrome 商店安装插件
   - 在要操作的网页上点击插件按钮
   - 按钮显示"ON"表示 CDP 已开启

2. **配置远程调试（非本地部署）**
   ```bash
   # 使用特殊参数启动 Chrome
   chrome --remote-debugging-port=18792 --remote-debugging-address=0.0.0.0
   ```

3. **检查端口监听**
   ```bash
   lsof -i :18792
   ```

4. **使用 socat 端口转发（如需要）**
   ```bash
   socat TCP-LISTEN:18792,fork,reuseaddr TCP:127.0.0.1:18792
   ```

---

### Q6.5: 如何配置企业微信/钉钉/微信公众号集成？

**A:** OpenClaw CN 社区版支持多种中国企业级平台集成：

**最新更新 (2026.2.23)**: OpenClaw 现在支持 **10+ 消息平台** simultaneously!

**支持的平台**:
- **国际平台**: WhatsApp, Telegram, Discord, Slack, Signal, iMessage
- **国内平台**: 企业微信，飞书，钉钉，微信公众号，微信小程序
- **其他**: Google Chat, Microsoft Teams, Matrix, Zalo, WebChat

**企业微信集成：**
1. 登录 [企业微信开放平台](https://work.weixin.qq.com/)
2. 创建自建应用，获取 AgentId 和 Secret
3. 配置接收消息服务器 URL
4. 在 OpenClaw 配置：
```bash
openclaw config set channels.wechatwork.agentId "your-agent-id"
openclaw config set channels.wechatwork.secret "your-secret"
openclaw config set channels.wechatwork.corpId "your-corp-id"
```

**钉钉集成：**
1. 登录 [钉钉开放平台](https://open.dingtalk.com/)
2. 创建机器人应用，获取 AppKey 和 AppSecret
3. 配置消息接收地址
4. 在 OpenClaw 配置：
```bash
openclaw config set channels.dingtalk.appKey "your-appkey"
openclaw config set channels.dingtalk.appSecret "your-appsecret"
```

**微信公众号集成：**
1. 登录 [微信公众平台](https://mp.weixin.qq.com/)
2. 配置服务器配置（URL、Token、EncodingAESKey）
3. 在 OpenClaw 配置：
```bash
openclaw config set channels.wechat-official.token "your-token"
openclaw config set channels.wechat-official.aesKey "your-aes-key"
```

**微信小程序集成：**
- 在小程序中接入客服消息功能
- 通过云开发或后端对接 OpenClaw
- 详见：[微信小程序集成指南](./channels/wechat-miniprogram.md)

**注意：** 以上功能正在积极开发中，部分功能可能需要等待后续版本发布。

---

### Q6.6: 如何更新 OpenClaw 到最新版本？

**A:** 最新版本是 **2026.2.23**（2026 年 3 月发布），包含关键安全补丁！

**重要安全更新**:
- 🔒 修复 WebSocket 劫持漏洞 (CVE-2026-25253)
- 🔒 增强 MCP (Model Context Protocol) 风险缓解
- 🔒 改进 prompt injection 保护
- 🔒 安全加固文档和最佳实践

**更新步骤**:

1. **检查当前版本**
   ```bash
   openclaw --version
   ```

2. **使用 npm 更新**
   ```bash
   npm update -g openclaw@latest
   ```
   
   **或使用安装脚本**:
   ```bash
   curl -fsSL https://openclaw.ai/install.sh | bash
   ```

3. **重启 daemon**
   ```bash
   openclaw daemon restart
   ```

4. **验证更新**
   ```bash
   openclaw --version
   openclaw gateway status
   ```

**新增功能 (2026.2.23)**:
- ✅ SecretRef 支持扩展到 64 个目标
- ✅ PDF 分析工具（原生支持 Anthropic 和 Google）
- ✅ MiniMax-M2.5-highspeed 模型支持
- ✅ Telegram streaming 默认开启
- ✅ CLI config 验证 (`openclaw config validate`)
- ✅ Ollama embeddings 支持
- ✅ Zalo Personal plugin 重构

**保持更新**:
- Watch GitHub repo: [github.com/openclaw/openclaw](https://github.com/openclaw/openclaw)
- 关注创作者: [@steipete](https://x.com/steipete) on X/Twitter
- 加入社区讨论: GitHub Discussions

---

## 🤖 自动化与技能

### Q7: Cron 和 Heartbeat 有什么区别？

**A:** 

| 特性 | Cron | Heartbeat |
|------|------|-----------|
| **执行时机** | 精确时间（如每天 9 点） | 周期性检查（如每 30 分钟） |
| **适用场景** | 定时任务、一次性提醒 | 定期检查、批量任务 |
| **会话类型** | 支持主会话和隔离会话 | 仅主会话 |
| **配置复杂度** | 需要 cron 表达式 | 简单配置即可 |

**使用建议：**
- 需要精确时间 → 用 Cron
- 多个检查可以批量 → 用 Heartbeat
- 一次性提醒 → Cron (`--at` 参数)

**详细教程：** [Cron vs Heartbeat](./automation/cron-vs-heartbeat.md)

---

### Q8: 如何安装和管理技能插件？

**A:** 

**安装技能：**
```bash
# 从 ClawHub 安装
openclaw skills install https://clawhub.ai/username/skill-name

# 本地安装
cd skill-directory
openclaw skills install .

# 从 ZIP 安装
wget https://clawhub.ai/user/skill/archive/v1.0.0.zip -O skill.zip
unzip skill.zip
cd skill
openclaw skills install .
```

**配置技能：**
```bash
openclaw config set skills.<skill-name>.env.API_KEY "your-key"
openclaw config set skills.<skill-name>.env.BASE_URL "http://localhost:8080"
openclaw gateway restart
```

**管理技能：**
```bash
# 列出已安装技能
openclaw skills list

# 卸载技能
openclaw skills uninstall <skill-name>

# 更新技能
openclaw skills update <skill-name>
```

**热门技能推荐：**
- 📊 ezBookkeeping - 自动记账
- 📧 Gmail - 邮件管理
- 📅 Smart Reminder - 智能日程提醒
- 🌐 Web Search - 网络搜索

---

## 💰 费用与成本

### Q9: 使用 OpenClaw 需要多少费用？

**A:** 主要费用构成：

**1. 服务器费用（如选择云部署）**
- 阿里云轻量应用服务器：68 元/年起（新人专享）
- 配置：2vCPU + 2GiB + 40GB

**2. 模型调用费用**
- **Coding Plan 套餐（推荐）：** 固定月费，成本可控
  - 首月体验：7.9 元起
  - 支持模型：qwen3.5-plus、kimi-k2.5 等
- **按 Token 计费：** 用多少付多少
  - qwen3.5-plus：约 0.004 元/1K tokens（输入）

**3. 第三方服务费用**
- 飞书/钉钉：基础功能免费
- WhatsApp/Telegram：免费

**成本优化建议：**
1. 使用 Coding Plan 套餐避免意外费用
2. 个人使用可选择本地部署（零服务器成本）
3. 合理设置自动化频率，减少不必要的 API 调用

---

## 🔧 故障排除

### Q10: Gateway 启动失败怎么办？

**A:** 常见问题排查：

```bash
# 1. 检查 Gateway 状态
openclaw gateway status

# 2. 查看详细日志
openclaw logs --follow

# 3. 检查端口占用
lsof -i :18789  # 默认端口

# 4. 检查配置文件
cat ~/.openclaw/openclaw.json

# 5. 重启 Gateway
openclaw gateway restart

# 6. 运行诊断工具
openclaw doctor
```

**常见错误：**
- `EADDRINUSE` - 端口被占用，修改端口或关闭占用进程
- `Authentication failed` - API Key 配置错误，检查配置
- `Network error` - 网络连接问题，检查防火墙设置

---

### Q11: 消息发送失败/收不到回复？

**A:** 排查步骤：

1. **检查频道配置**
   ```bash
   openclaw channels list
   openclaw channels status <channel-name>
   ```

2. **验证配对状态**
   - 确认机器人已完成配对
   - 检查配对码是否过期

3. **检查消息路由**
   - 群聊中需要@机器人才能触发
   - 私聊直接发送即可

4. **查看 Gateway 日志**
   ```bash
   openclaw logs --grep "message"
   ```

5. **测试消息发送**
   ```bash
   openclaw message send --target <user> --message "test"
   ```

---

## 📚 学习资源

### Q12: 如何快速学习 OpenClaw？

**A:** 推荐学习路径：

1. **新手入门（1-2 小时）**
   - [快速开始指南](./getting-started/01-quickstart.md)
   - [核心功能介绍](./getting-started/02-features.md)

2. **基础实践（2-4 小时）**
   - 配置一个聊天频道（飞书/WhatsApp）
   - 创建第一个 Cron 定时任务
   - 安装一个技能插件

3. **进阶应用（1-2 天）**
   - [Cron 定时任务教程](./automation/01-cron-jobs.md)
   - 自定义 SOUL.md 人设
   - 配置多 Agent 路由

4. **实战项目（1 周+）**
   - 搭建个人自动化工作流
   - 集成多个频道和技能
   - 优化配置和性能

**实战教程：** [6 大神级用法](./usecases/6-advanced-usecases.md)

---

## ❓ 其他问题

没有找到你的问题？

- 📖 查看 [官方文档](https://docs.openclaw.ai)
- 💬 参与 [GitHub Discussions](https://github.com/openclaw/openclaw/discussions)
- 🐛 提交 [Issue](https://github.com/openclaw/openclaw/issues)
- 📝 在 OpenClaw-CN 仓库提问题

---

*最后更新：2026-03-02*
*本文档持续更新，欢迎贡献！*
