<p align="center">
    <img width="512" src="content/media/avatar.jpg">
</p>

# OpenClaw-CN

<div align="center">

**OpenClaw 中文技术教程与资源聚合**

一站式 OpenClaw 中文学习平台 · 实战教程 · 配置模板 · 自动化案例

[MIT License](LICENSE) · [Issues](https://github.com/scourgen/OpenClaw-CN/issues) · [社区微信群](#社区)

</div>

---

## 📖 项目介绍

**OpenClaw-CN** 是一个专注于 **OpenClaw** 的中文技术教程、配置模板和实战案例的资源聚合仓库。

### 🎯 我们能帮助你

- ✅ **快速上手** - 5 分钟完成 OpenClaw 配置，发送第一条消息
- ✅ **掌握自动化** - Cron 定时任务、心跳任务、Webhooks 集成
- ✅ **定制人设** - 丰富的 SOUL.md 模板库，打造专属 AI 助理
- ✅ **实战案例** - 记账、日程管理、内容创作、竞品监控等真实场景
- ✅ **问题解决** - FAQ 常见问题、故障排查、最佳实践

### 💡 适合人群

| 人群 | 你能获得什么 |
|------|-------------|
| 🔰 初学者 | 系统化教程，快速入门不迷路 |
| 💼 职场人士 | 自动化工作流，效率提升 10 倍 |
| 🎓 学生 | 学习陪练、情绪支持、时间管理 |
| 👥 开发者 | 技能开发指南、API 集成、高级技巧 |
| 🏢 企业 | 私有化部署、团队协作、定制化方案 |

### 🌟 核心特色

- **中文友好** — 为中文用户量身定制，避免语言障碍
- **实战导向** — 每个教程都有可直接运行的示例
- **持续更新** — 每周新增教程、模板和案例
- **社区驱动** — 来自真实用户的经验分享
- **开源免费** — MIT 协议，可自由使用和修改

---

[![](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)[![](https://img.shields.io/github/issues/scourgen/OpenClaw-CN.svg)](https://github.com/scourgen/OpenClaw-CN/issues)  [![](https://img.shields.io/github/forks/scourgen/OpenClaw-CN.svg)](https://github.com/scourgen/OpenClaw-CN/network) [![](https://img.shields.io/github/stars/scourgen/OpenClaw-CN.svg)](https://github.com/scourgen/OpenClaw-CN/stargazers) [![](https://travis-ci.org/scourgen/OpenClaw-CN.svg?branch=master)](https://travis-ci.org/scourgen/OpenClaw-CN) [![](https://img.shields.io/github/release/scourgen/OpenClaw-CN.svg)](https://github.com/scourgen/OpenClaw-CN/releases)

---

## 基本概念

### SOUL.md 配置文件

| 文件 | 说明 |
|------|------|
| `SOUL.md` | 机器人的人设 - 定义行为准则和语气风格 |
| `USER.md` | 关于你的信息 - 姓名、偏好、语言等，让 SOUL 认识你 |
| `MEMORY.md` | 长期记忆 - AI 应该一直记住的重要事情 |
| `HEARTBEAT.md` | 待办清单 - AI 定期检查并自主执行的任务 |

### 📚 SOUL.md 模板库

整理了 **7 大类** 常用 SOUL.md 配置模板，涵盖个人效率、内容创作、技术支持、特殊关怀等场景：

- 📰 **信息获取助理** - 每日新闻简报、招投标监测
- 💙 **情绪鼓励师** - 压力管理、情绪陪伴
- 🎯 **内容选题助理** - 热点追踪、选题策划
- 🎬 **内容创作助理** - 网文/短剧/漫剧创作
- 💻 **程序员鼓励师** - 代码工作压力疏导
- 📚 **青少年学习陪练** - 题目讲解、学习计划
- 🏥 **特殊人群陪伴** - 老人/孕妇/病患关怀

👉 **完整模板：** [docs/soul-templates.md](docs/soul-templates.md)

---

### 📚 中文教程

以下是最新翻译和创建的中文教程：

#### 🔰 快速入门系列

| 教程 | 说明 | 难度 |
|------|------|------|
| [快速开始指南](./tutorials/getting-started/01-quickstart.md) | 从零开始配置 OpenClaw，5 分钟上手 | ⭐ |
| [核心功能介绍](./tutorials/getting-started/02-features.md) | 全面了解 OpenClaw 的所有功能 | ⭐ |

#### 🤖 自动化系列

| 教程 | 说明 | 难度 |
|------|------|------|
| [Cron 定时任务](./tutorials/automation/01-cron-jobs.md) | 使用 Cron 调度自动化任务，定时执行 Agent | ⭐⭐ |

#### 📖 文档索引

完整的翻译文档索引：[查看文档索引](./docs/translation/README.md)

---

### 🆕 即将推出

- [ ] Heartbeat 心跳任务教程
- [ ] Webhooks 集成指南
- [ ] WhatsApp/Telegram/Discord 频道配置
- [ ] 多 Agent 路由配置
- [ ] 移动端节点设置

---

## 🚀 快速开始

### 选择你的学习路径

#### 🔰 新手路径（推荐顺序）

1. **快速开始** → [教程 01](./tutorials/getting-started/01-quickstart.md)
   - 安装 OpenClaw
   - 配置认证和 Gateway
   - 打开 Control UI
   - 发送第一条消息

2. **了解功能** → [教程 02](./tutorials/getting-started/02-features.md)
   - 频道集成（WhatsApp、Telegram、Discord 等）
   - Agent 功能和会话管理
   - 媒体支持和移动端节点

3. **自动化入门** → [Cron 定时任务](./tutorials/automation/01-cron-jobs.md)
   - 创建定时任务
   - 配置每日简报
   - 设置提醒和周期性任务

#### 💼 进阶路径（按需选择）

1. **集成指南**
   - [OpenClaw 集成飞书](https://help.aliyun.com/zh/simple-application-server/use-cases/openclaw-integrated-fly-book)
   - [OpenClaw 企业微信集成](https://help.aliyun.com/zh/simple-application-server/use-cases/openclaw-enterprise-wechat-integration)
   - [通过 OpenClaw 调用 IMessage](https://help.aliyun.com/zh/simple-application-server/use-cases/invoking-imessage-via-openclaw)
   - [玩转 OpenClaw 指南：阿里云 + 本地部署 +6 大神级用法](https://developer.aliyun.com/article/1713540)

#### 📺 视频教程

   - [【傅盛×三万】直播之夜：断腿养伤 14 天，养出了一个"AI 天团"](https://www.bilibili.com/video/BV1XNAhzcE5x/)

#### 🔩 高级技巧

1. 飞书相关
   - [OpenClaw 飞书 API 调用次数耗尽解决办法](https://xx0a.com/blog/openclaw-feishu)
   
   > 飞书的官方插件默认每分钟调用一次/open-apis/bot/v3/info 接口，24 小时调用 1440 次，而普通飞书账号每个月限额调用 10000 次 API，所以几天就会耗尽

##### ❓ 常见问题解答 (FAQ)

  **为什么 OpenClaw 连接不了浏览器？**
  
  OpenClaw 使用 CDP 连接浏览器，并且通过接口指挥浏览器执行各种操作，在实际使用中你首先需要安装 OpenClaw Browser Relay 这个插件，然后在你要操作的网页上点击这个插件的按钮，按钮出现 ON 即表示插件已经在这个页面上打开了 CDP。
  
  但是如果你的 OpenClaw 没有安装在本地，这就牵涉到远程 IP 见的通信，而由于种种原因，Chrome 的插件是无法监听 0.0.0.0ip 的（只能监听在 localhost 上），所以你可以尝试：
  
  1. 使用特殊的参数打开 chrome：`chrome --remote-debugging-port=18792 --remote-debugging-address=0.0.0.0`
  2. 开启插件，再通过 `lsof -i :18792` 查看 chrome 有没有监听在 0.0.0.0 上
  3. 如果还是没有的话，可以使用 socat 进行端口转发：`socat TCP-LISTEN:18792,fork,reuseaddr TCP:127.0.0.1:18792`

---

## 💰 OpenClaw 变现与商业化

### 个人变现

- 上门代安装 OpenClaw，500 一次 🤣

### 小团队

- 为企业提供定制化技能培训
- 开发垂直领域技能包
- 提供技术咨询和部署服务

### 中大企业

- 私有化部署
- 内部工作流自动化
- 客服系统智能化
- 知识库管理与检索

---

## 🔗 相关资源

### 推荐模型

| 模型 | 厂商 | 一句话 | 购买链接 | 推荐程度 |
|------|------|------|-----|-----|
| `qwen3.5-plus` | 阿里通义 | 开源模型国产首选，中文能力强 | [首月 7.9 元起](https://www.aliyun.com/benefit/ai/aistar?clubBiz=subTask..12408319..10263..) | 🌟🌟🌟🌟🌟 |
| `GLM-v5` | 智谱 AI | 高性价比之选 | [49 元/月起](https://www.bigmodel.cn/glm-coding?ic=KY1ACMPOVX) | 🌟🌟🌟🌟 |
| `kimi-k2.5` | 月之暗面 | 超长上下文 | | 🌟🌟🌟 |
| `minimax-m2.1` | MiniMax | 对话创作出色 | | 🌟🌟🌟 |
| `claude-sonnet-4-6` | Anthropic | 性能均衡 | claude.ai | 🌟🌟🌟 |
| `claude-opus-4-6` | Anthropic | 旗舰能力最强 | | 🌟🌟🌟 |
| `deepseek-v3` | DeepSeek | 高性价比之选 | | 🌟🌟 |

> 注意：推荐程度仅为个人评价，仅供参考

### 官方资源

- **ClawHub** - OpenClaw 生态的"应用商店"和"技能市场"：https://clawhub.ai/
  > ⚠️ **安全提醒**：近期有媒体发现在 clawhub 里有恶意技能会窃取用户数据，使用时一定要小心

- **TOP5 技能推荐：**
  - **Ontology** - 为 AI 构建了结构化知识图谱，提升复杂任务处理能力
  - **self-improving-agent** - 能让 AI 通过经验迭代，实现自我优化
  - **gog** - 可自动化 Google Workspace 全流程操作
  - **tavily-search** - 面向 AI 优化的网页搜索功能
  - **find-skills** - 让 AI 具备查找有没有现成的 Skill 可以解决问题的能力

### 学习资源

#### 📚 精选教程与案例（15 个）

| # | 资源名称 | 简介 | 链接 |
|---|---------|------|------|
| 1 | **Awesome OpenClaw Usecases** | 社区精选 34+ 实战案例：Reddit 摘要、YouTube 每日简报、X 账号分析等 | [GitHub](https://github.com/hesamsheikh/awesome-openclaw-usecases) |
| 2 | **Awesome OpenClaw Tutorial** | 29 万字中文教程，70+ 案例，57+ 截图，7 天学习路径 | [GitHub](https://github.com/xianyu110/awesome-openclaw-tutorial) |
| 3 | **OpenClaw Showcase** | 官方真实案例：邮件管理、日历同步、智能提醒、自动创建 Issue 等 | [Showcase](https://openclaw.ai/showcase) |
| 4 | **OpenClaw 101** | 中文资源聚合站，快速上手指南、技能市场导航 | [OpenClaw101](https://openclaw101.dev/) |
| 5 | **Daily Reddit Digest** | 自动汇总关注的 subreddit，基于偏好生成个性化摘要 | [用例](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/daily-reddit-digest.md) |
| 6 | **Daily YouTube Digest** | 每日追踪关注频道的新视频，不错过任何更新 | [用例](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/daily-youtube-digest.md) |
| 7 | **Multi-Source Tech News** | 聚合 109+ 科技新闻源（RSS/Twitter/GitHub），智能评分推送 | [用例](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/multi-source-tech-news-digest.md) |
| 8 | **YouTube Content Pipeline** | 自动化视频选题、调研、追踪，一站式内容创作工作流 | [用例](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/youtube-content-pipeline.md) |
| 9 | **Multi-Agent Content Factory** | Discord 多 Agent 协作：调研、写作、封面设计 Agent 分工 | [用例](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/content-factory.md) |
| 10 | **Autonomous Game Dev** | 教育游戏全生命周期：需求→开发→文档→Git 提交 | [用例](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/autonomous-game-dev-pipeline.md) |
| 11 | **Podcast Production Pipeline** | 播客全流程：嘉宾调研→大纲→Show Notes→社媒推广 | [用例](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/podcast-production-pipeline.md) |
| 12 | **n8n Workflow Orchestration** | 通过 Webhooks 委托 n8n 执行 API，Agent 不接触凭证 | [用例](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/n8n-workflow-orchestration.md) |
| 13 | **Self-Healing Home Server** | 24 小时基础设施 Agent，SSH+ 自动修复+Cron 任务 | [用例](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/self-healing-home-server.md) |
| 14 | **成本计算器** | 本地/云端部署成本对比，省钱攻略（节省 64%-96%） | [计算器](https://github.com/xianyu110/awesome-openclaw-tutorial/blob/main/COST-CALCULATOR.md) |
| 15 | **学习路径指南** | 7 天掌握核心技能，每天 2 小时，3 个实战项目 | [学习路径](https://github.com/xianyu110/awesome-openclaw-tutorial/blob/main/LEARNING-PATH.md) |

### 相关项目

- [OpenKita](https://github.com/openakita/openakita/blob/main/README_CN.md) - 一个你睡觉时还在变强的 AI Agent
- [miniclaw](https://github.com/memovai/mimiclaw) - 纯 C 实现的类 OpenClaw，精简、小巧、省电，能跑在 30 元的 ESP32 USB 开发板上

### Vibe Coding 工具

- [Warp](http://warp.dev/) - AI 时代的 Terminal 终端工具
- [Shellman](https://www.shellman.dev/) - 在网页中运行多个 Vibe Coding Agent

---

## 📖 推荐阅读

- [Jason Zhang 的个人博客](https://junxinzhang.com/) - 讲了许多使用 OpenClaw 的经验和个人体会

---

## 社区

<table>
  <tr>
    <td align="center">
      <img src="content/media/wechat_qr.jpg" width="200" alt="微信二维码" /><br/>
      <b>小助理微信</b><br/>
    </td>
    <td>
      <b>微信群：</b><br/><br/>
      <b>OpenClaw 使用交流</b>: 扫码加小助理微信后让拉你进群<br/>
      <b>OpenClaw 商业和企业化</b>: 仅限邀请，暂不开放<br/>
      <b>OpenClaw-CN 维护</b>: 仅限邀请，暂不开放<br/>
      <b>VibeCoding 交流</b>: 仅限邀请，暂不开放<br/>
      <b>邮箱</b> — <a href="mailto:scourgen@gmail.com">scourgen@gmail.com</a>
    </td>
  </tr>
</table>

## ⭐ Star History

如果这个项目对你有帮助，请给我们一个 Star ⭐️

---

<div align="center">

**用 AI 让你的生活更美好**

Made with ❤️ by Scourgen

</div>
