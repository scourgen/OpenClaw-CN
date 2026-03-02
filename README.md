<p align="center">
    <img width="512" src="content/media/avatar.jpg">
</p>

# OpenClaw 中文技术汇总

## 持续更新的 OpenClaw 中文技术教程

[![](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)[![](https://img.shields.io/github/issues/scourgen/OpenClaw-CN.svg)](https://github.com/scourgen/OpenClaw-CN/issues)  [![](https://img.shields.io/github/forks/scourgen/OpenClaw-CN.svg)](https://github.com/scourgen/OpenClaw-CN/network) [![](https://img.shields.io/github/stars/scourgen/OpenClaw-CN.svg)](https://github.com/scourgen/OpenClaw-CN/stargazers) [![](https://travis-ci.org/scourgen/OpenClaw-CN.svg?branch=master)](https://travis-ci.org/scourgen/OpenClaw-CN) [![](https://img.shields.io/github/release/scourgen/OpenClaw-CN.svg)](https://github.com/scourgen/OpenClaw-CN/releases)

---

## 📖 项目介绍

欢迎来到 **OpenClaw 中文技术教程** 仓库！

这是一个专注于 **OpenClaw** 使用技巧、最佳实践和实战案例的中文教程集合。我们的目标是帮助中文开发者更高效地使用 OpenClaw，将其打造成真正的 AI 助手。

### 🎯 项目特点

- **系统化**：从基础到进阶，循序渐进
- **实战导向**：每个教程都包含实际可用的示例
- **中文友好**：为中文开发者量身定制
- **持续更新**：定期添加新的教程和技巧
- **快速上手**：每个教程都有"快速使用"部分

### 💡 适合人群

- 🔰 OpenClaw 初学者 - 想要快速上手
- 💼 专业开发者 - 寻求效率提升
- 🎓 技术爱好者 - 探索 AI 编程的未来
- 👥 开发团队 - 建立 AI 协作工作流

---

## 基本概念

### SOUL\.md

| 文件 | 说明 |
|------|------|
| `SOUL.md` | 机器人的人设 |
| `USER.md` | 关于你的信息 — 姓名、偏好、语言等等，让SOUL识别你认识你 |
| `MEMORY.md` | 长期记忆 — 它应该一直记住的事 |
| `HEARTBEAT.md` | 待办清单 — 机器人定期检查并自主执行 |

## 📚 基础系列

### SOUL\.md仓库

- 搜集了大量SOUL.md 配置，[SOUL.md模板仓库](/SOUL/README.md)

### 📚 新增中文教程

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
- [ ] WhatsApp/Telegram/Discord频道配置
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
   - [OpenClaw（原Moltbot、Clawdbot）集成飞书](https://help.aliyun.com/zh/simple-application-server/use-cases/openclaw-integrated-fly-book)
   - [OpenClaw（原Moltbot、Clawdbot）企业微信集成](https://help.aliyun.com/zh/simple-application-server/use-cases/openclaw-enterprise-wechat-integration)
   - [通过OpenClaw（原Moltbot、Clawdbot）调用IMessage](https://help.aliyun.com/zh/simple-application-server/use-cases/invoking-imessage-via-openclaw)
   - [玩转OpenClaw指南：阿里云+本地部署+6大神级用法，普通人也能10秒上手解锁AI Agent助手生产力，效率翻倍](https://developer.aliyun.com/article/1713540)

#### 📺 视频教程

   - [【傅盛×三万】直播之夜：断腿养伤14天，养出了一个“AI天团”](https://www.bilibili.com/video/BV1XNAhzcE5x/)

#### 🔩 高级技巧

1. 飞书相关
   - [OpenClaw 飞书API调用次数耗尽解决办法](https://xx0a.com/blog/openclaw-feishu)
飞书的官方插件默认每分钟调用一次/open-apis/bot/v3/info接口，24小时调用1440次，而普通飞书账号每个月限额调用10000次API，所以几天就会耗尽

##### ❓ 常见问题解答 (FAQ)

  - 为什么OpenClaw连接不了浏览器？
OpenClaw使用CDP连接浏览器，并且通过接口指挥浏览器执行各种操作，在实际使用中你首先需要安装OpenClaw Browser Relay这个插件，然后在你要操作的网页上点击这个插件的按钮，按钮出现ON即表示插件已经在这个页面上打开了CDP。但是如果你的OpenClaw没有安装在本地，这就牵涉到远程IP见的通信，而由于种种原因，Chrome的插件是无法监听0.0.0.0ip的（只能监听在localhost上），所以你可以尝试：使用特殊的参数打开chrome，例如`chrome --remote-debugging-port=18792 --remote-debugging-address=0.0.0.0`，然后开启插件，再通过`lsof -i :18792`查看chrome有没有监听在0.0.0.0上，如果还是没有的话，可以使用socat进行端口转发，把0.0.0.0:18792转发到127.0.0.1:18792上，命令如下`socat TCP-LISTEN:18792,fork,reuseaddr TCP:127.0.0.1:18792`

--- 

### OpenClaw的变现、商业化和企业级应用（怎么赚钱 or 帮你的公司赚钱）

#### 个人变现

  - 上门代安装OpenClaw，500一次🤣

#### 小团队

#### 中大企业

---

<!-- ## 🎓 学习建议

### 1. 动手实践最重要

理论知识只是第一步，**实际动手**才能真正掌握技巧。

**建议**：
- 每学完一个教程，立即在实际项目中应用
- 创建一个测试项目专门练习新技巧
- 记录使用过程中的心得体会

### 2. 循序渐进

不要试图一次学完所有教程，按照推荐的学习路径逐步前进。

**推荐节奏**：
- Week 1：掌握文档技巧（教程01）
- Week 2：学习GitHub工具（教程02）
- Week 3：在真实项目中应用
- Week 4：分享经验，帮助他人

### 3. 建立自己的工作流

每个开发者的工作习惯不同，不要生搬硬套。

**建议**：
- 选择适合自己的技巧组合
- 根据项目特点调整工作流程
- 定期回顾和优化

### 4. 参与社区

分享你的经验，从他人那里学习。

**方式**：
- 在 Issues 中讨论问题
- 提交 Pull Request 改进教程
- 分享你的成功案例

--- -->

<!-- ## 🤝 如何贡献

我们欢迎各种形式的贡献！

### 贡献方式

#### 📝 改进现有教程

发现教程中的错误或不清楚的地方？

1. Fork 这个仓库
2. 创建你的特性分支 (`git checkout -b improve-tutorial-01`)
3. 提交你的修改 (`git commit -m '改进教程01的某某部分'`)
4. 推送到分支 (`git push origin improve-tutorial-01`)
5. 创建一个 Pull Request

#### ✨ 添加新教程

有好的技巧想要分享？

1. 在 `tutorials/` 下创建新目录（如 `03-your-topic/`）
2. 按照现有教程的格式编写
3. 确保包含：
   - 快速使用部分
   - 完整教程
   - 实际示例
   - 元信息（作者、版本、日期）
4. 更新主 README.md 的教程目录
5. 提交 Pull Request

#### 🐛 报告问题

发现 bug 或有改进建议？

- 在 [Issues](https://github.com/scourgen/OpenClaw-CN/issues) 中创建新 issue
- 详细描述问题或建议
- 如果可能，提供截图或代码示例

#### 💬 分享使用心得

- 在 Discussions 中分享你的经验
- 帮助其他学习者解答问题
- 提供实际项目中的应用案例

### 贡献者公约

- 尊重他人
- 保持专业
- 乐于助人
- 开放包容

--- -->

## 🔗 相关资源

### 模型推荐

### 推荐模型

| 模型 | 厂商 | 一句话 | 购买链接 | 推荐程度 |
|------|------|------|-----|-----|
| `qwen3.5-plus` | 阿里通义 | 开源模型国产首选，中文能力强 | 首月 7.9 元起 [点击购买](https://www.aliyun.com/benefit/ai/aistar?clubBiz=subTask..12408319..10263..) |  🌟🌟🌟🌟🌟 |
| `GLM-v5` | 智谱AI | 高性价比之选 | 49元/月起，每天早上10点限量抢购 [点击购买](https://www.bigmodel.cn/glm-coding?ic=KY1ACMPOVX) |  🌟🌟🌟🌟 |
| `kimi-k2.5` | 月之暗面 | 超长上下文 |  |  🌟🌟🌟 |
| `minimax-m2.1` | MiniMax | 对话创作出色 |  |  🌟🌟🌟 |
| `claude-sonnet-4-6` | Anthropic | 性能均衡 | claude.ai | 🌟🌟🌟
| `claude-opus-4-6` | Anthropic | 旗舰能力最强 | |🌟🌟🌟 |
| `deepseek-v3` | DeepSeek | 高性价比之选 |  |  🌟🌟 |

- 注意：推荐程度仅为个人评价，仅限参考
- 除此以外，还可以通过一些特殊方法，嫁接OpenClaw使其使用Google的Antigravity模型，但近期大量发生封号事件，不再建议使用

### 官方资源

- ClawHub OpenClaw生态的“应用商店”和“技能市场” https://clawhub.ai/ ⚠️注意：近期有媒体发现在clawhub里有恶意技能会窃取用户数据，使用时一定要小心
**TOP5的Skill介绍：**
  - Ontology 为AI构建了结构化知识图谱，提升复杂任务处理能力
  - self-improving-agent 能让AI通过经验迭代，实现自我优化
  - gog 可自动化Google Workspace全流程操作
  - tavily-search 面向AI优化的网页搜索功能
  - find-skills 让AI具备查找有没有现成的Skill可以解决问题的能力

### 推荐工具


### 学习资源
 - Awesome OpenClaw Usecases 搜集了大量的OpenClaw使用场景 https://github.com/hesamsheikh/awesome-openclaw-usecases
 - Awesome OpenClaw Tutorial 从零开始打造你的AI工作助手：最全面的中文教程，涵盖安装、配置、实战案例和避坑指南 https://github.com/xianyu110/awesome-openclaw-tutorial
 - OpenClaw Showcase 官方搜集了大量正在使用OpenClaw的用户的真实使用场景 https://openclaw.ai/showcase
 - OpenClaw 101 OpenClaw 101 是一个开源的 OpenClaw 资源聚合站，旨在帮助中文用户快速上手 OpenClaw https://openclaw101.dev/

### 相关项目

- OpenKita 一个你睡觉时还在变强的 AI Agent https://github.com/openakita/openakita/blob/main/README_CN.md
- miniclaw 纯C实现的类OpenClaw，精简、小巧、省电，能跑在30元的 ESP32 USB开发板上 https://github.com/memovai/mimiclaw

## Vibe Coding 相关

### 工具介绍
- Warp AI时代的Terminal终端工具 http://warp.dev/
- Shellman 在网页中运行多个Vibe Coding Agent https://www.shellman.dev/
  
---

## 推荐阅读

### Jason Zhang 的个人博客，讲了许多使用OpenClaw的经验和个人体会 https://junxinzhang.com/

---

<!-- ## 📞 联系方式

### 项目维护者

**Scourgen**
- GitHub: [@scourgen](https://github.com/scourgen)
- Email: scourgen@gmail.com

### 获取帮助

- 📮 通过 [Issues](https://github.com/scourgen/OpenClaw-CN/issues) 提问
- 💬 在 [Discussions](https://github.com/scourgen/OpenClaw-CN/discussions) 讨论
- 🐦 关注我们的更新 -->

<!-- ## 📄 许可证

本项目采用 [MIT License](LICENSE) 开源协议。

你可以自由地：
- ✅ 使用本教程的内容
- ✅ 修改和改进教程
- ✅ 分享给其他人

但请：
- 📌 保留原作者信息
- 📌 注明修改内容
- 📌 使用相同的开源协议

--- -->


## 社区

<table>
  <tr>
    <td align="center">
      <img src="content/media/wechat_qr.jpg" width="200" alt="微信二维码" /><br/>
      <b>小助理微信</b><br/>
    </td>
    <td>
      <b>微信群：</b><br/><br/>
      <b>OpenClaw使用交流</b>: 扫码加小助理微信后让拉你进群<br/>
      <b>OpenClaw商业和企业化</b>: 仅限邀请，暂不开放<br/>
      <b>OpenClaw-CN维护</b>: 仅限邀请，暂不开放<br/>
      <b>VibeCoding交流</b>: 仅限邀请，暂不开放<br/>
      <b>邮箱</b> — <a href="mailto:scourgen@gmail.com">scourgen@gmail.com</a>
    </td>
  </tr>
</table>

## ⭐ Star History

如果这个项目对你有帮助，请给我们一个 Star ⭐️

---

<!-- ## 💝 致谢

感谢所有为本项目做出贡献的开发者和用户。

特别感谢：
- OpenClaw 团队提供的强大工具
- 开源社区的无私分享
- 所有提供反馈和建议的用户

--- -->

<div align="center">

**用 AI 让你的生活更美好**

Made with ❤️ by Scourgen

</div>