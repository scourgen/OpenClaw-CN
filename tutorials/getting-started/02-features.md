# OpenClaw 核心功能

## 功能亮点

| 功能 | 说明 |
|------|------|
| 📱 **多频道支持** | WhatsApp、Telegram、Discord 和 iMessage，单个 Gateway 即可支持 |
| 🔌 **插件扩展** | 通过扩展包添加 Mattermost 等更多频道 |
| 🔄 **多 Agent 路由** | 隔离的会话，支持每个工作区或发送者独立 Agent |
| 📎 **媒体支持** | 支持图片、音频和文档的收发 |
| 🖥️ **应用和 UI** | Web Control UI 和 macOS 配套应用 |
| 📲 **移动端节点** | iOS 和 Android 节点，支持配对、Canvas、摄像头、屏幕录制和语音功能 |

## 完整功能列表

### 频道集成

* WhatsApp 集成（通过 WhatsApp Web / Baileys）
* Telegram 机器人支持（grammY）
* Discord 机器人支持（discord.js）
* Mattermost 机器人支持（插件）
* iMessage 集成（通过本地 imsg CLI，仅限 macOS）

### Agent 功能

* Pi Agent 桥接（RPC 模式，支持工具流式传输）
* 长响应流式传输和分块
* 多 Agent 路由（每个工作区或发送者的隔离会话）
* Anthropic 和 OpenAI 的 OAuth 订阅认证

### 会话管理

* 直接聊天合并到共享 `main` 会话
* 群组聊天隔离会话
* 群组聊天支持（基于 @mention 激活）

### 媒体和语音

* 图片、音频和文档支持
* 可选的语音笔记转录钩子

### 应用界面

* WebChat 和 macOS 菜单栏应用
* Web Control UI（浏览器控制面板）

### 移动端节点

**iOS 节点：**
* 配对功能
* Canvas 支持
* 摄像头、屏幕录制
* 位置功能
* 语音功能

**Android 节点：**
* 配对功能和 Connect 标签
* 聊天会话
* 语音标签
* Canvas/摄像头/屏幕功能
* 设备、通知、联系人/日历命令
* 运动、照片、SMS 和应用更新命令

<Note>
  注意：旧版 Claude、Codex、Gemini 和 Opencode 路径已被移除。Pi 是唯一的编码 Agent 路径。
</Note>

---

## 使用场景

### 个人开发者
* 通过 WhatsApp/Telegram 随时随地与 AI 助手交流
* 在手机上就能进行代码审查和问题解答
* 无需打开电脑即可获取技术帮助

### 开发团队
* 在 Discord/Slack 中集成 AI 助手
* 团队共享的 AI 知识库
* 自动化的代码审查和文档生成

### 企业应用
* 企业微信/飞书集成
* 内部客服自动化
* 员工培训和知识库查询

---

*翻译自：https://docs.openclaw.ai/concepts/features.md*
*最后更新：2026-03-02*
