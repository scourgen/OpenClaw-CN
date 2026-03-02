# OpenClaw 六大神级用法实战教程

> 来源：阿里云开发者社区实战经验 + 社区用户案例
> 
> 难度：⭐⭐ 中级 | 预计耗时：30-60 分钟/案例

---

## 📋 案例概览

| 案例 | 功能 | 适合人群 | 难度 |
|------|------|---------|------|
| [1️⃣ 赛博记账员](#案例 1-赛博记账员自动记录收支) | 自动记账、语音/截图输入 | 对数字不敏感、难以坚持记账 | ⭐⭐ |
| [2️⃣ 发票整理大师](#案例 2-发票整理大师自动下载打包) | 自动下载发票、重命名、打包 | 自由职业者、企业用户 | ⭐⭐⭐ |
| [3️⃣ 暴躁包工头](#案例 3-暴躁包工头专治拖延症) | 个性化日程提醒、催更 | 拖延症患者 | ⭐⭐ |
| [4️⃣ 全网情报搜集](#案例 4-全网情报搜集一键抓取热点) | 多平台热点聚合、过滤水文 | 内容创作者、运营人员 | ⭐⭐⭐ |
| [5️⃣ 智能邮件助手](#案例 5-智能邮件助手自动分类回复) | 邮件分类、智能回复 | 邮件重度用户 | ⭐⭐ |
| [6️⃣ 跨平台消息同步](#案例 6-跨平台消息同步多端统一管理) | 多频道消息聚合、统一回复 | 多平台运营者 | ⭐⭐⭐ |

---

## 案例 1: 赛博记账员 - 自动记录收支

### 🎯 适用场景

- 对数字不敏感，总是忘记记账
- 懒得手动输入每笔支出
- 想要自动分类统计消费

### 🛠️ 所需组件

- OpenClaw Gateway
- ezBookkeeping（开源记账应用）
- OpenClaw ezBookkeeping 技能插件

### 📝 部署步骤

#### 步骤 1：部署 ezBookkeeping

```bash
# 1. 安装 Docker（如未安装）
# Ubuntu/Debian:
sudo apt update && sudo apt install -y docker.io
sudo systemctl start docker && sudo systemctl enable docker

# macOS (Homebrew):
brew install docker && open /Applications/Docker.app

# 2. 启动 ezBookkeeping 容器
docker run -d -p 8080:8080 \
  -e EBK_SECURITY_ENABLE_API_TOKEN=true \
  -v $(pwd)/ezbookkeeping-data:/ezbookkeeping/data \
  --name ezbookkeeping \
  mayswind/ezbookkeeping:latest
```

**重要：** 必须挂载数据目录 (`-v`)，否则容器重启后数据丢失！

#### 步骤 2：获取 API Token

1. 访问 http://localhost:8080（或服务器 IP:8080）
2. 登录后点击右上角头像 → 用户设置 → 安全
3. 生成 API Token，复制保存

#### 步骤 3：安装 OpenClaw 技能

```bash
# 下载技能包
wget https://clawhub.ai/mayswind/ezbookkeeping/archive/refs/tags/v1.1.0.zip -O ezbookkeeping-1.1.0.zip

# 解压
unzip ezbookkeeping-1.1.0.zip
cd ezbookkeeping-1.1.0

# 安装技能
openclaw skills install .
```

#### 步骤 4：配置技能

```bash
# 配置 API Token 和基础 URL
openclaw config set skills.ezbookkeeping.env.EBK_API_TOKEN "你的令牌"
openclaw config set skills.ezbookkeeping.env.EBK_BASE_URL "http://localhost:8080"

# 重启 Gateway
openclaw gateway restart
```

### 💬 使用示例

**方式 1：发送支付截图**
> 直接发送支付截图给机器人，AI 会自动识别金额、商家、时间

**方式 2：语音/文字输入**
```
昨天带我弟和女朋友吃韩国自助烤肉，花了 200 元
```

**方式 3：分类记账**
```
记账：交通费 50 元，类别：出行，备注：打车去机场
```

### 📊 效果展示

AI 会自动：
- ✅ 识别金额、时间、商家
- ✅ 自动分类（餐饮、交通、娱乐等）
- ✅ 记录到 ezBookkeeping 后台
- ✅ 支持统计分析和报表

### ⚠️ 避坑指南

1. **数据持久化：** 必须挂载数据目录
2. **API Token：** 默认未启用，需设置环境变量 `EBK_SECURITY_ENABLE_API_TOKEN=true`
3. **网络访问：** 确保 OpenClaw 能访问 ezBookkeeping 的 8080 端口

---

## 案例 2: 发票整理大师 - 自动下载打包

### 🎯 适用场景

- 自由职业者需要整理客户发票
- 企业财务需要汇总供应商发票
- 个人需要整理报销凭证

### 🛠️ 所需组件

- OpenClaw Gateway
- Gmail 技能插件（或其他邮箱）
- Maton API（用于 Gmail 授权）

### 📝 部署步骤

#### 步骤 1：安装 Gmail 技能

```bash
# 下载技能包
wget https://clawhub.ai/byungkyu/gmail/archive/refs/tags/v1.0.6.zip -O gmail-1.0.6.zip

# 解压
unzip gmail-1.0.6.zip
cd gmail-1.0.6

# 安装技能
openclaw skills install .
```

#### 步骤 2：配置 Maton API

1. 访问 [maton.ai/settings](https://maton.ai/settings) 获取 API Key
2. 访问 [ctrl.maton.ai](https://ctrl.maton.ai) 授权 Google 账户

```bash
# 配置 API Key
openclaw config set skills.gmail.env.MATON_API_KEY "你的 Maton API Key"

# 重启 Gateway
openclaw gateway restart
```

#### 步骤 3：发送整理指令

给机器人发送消息：

```
帮我检索最近一个月邮箱里所有包含"发票"、"Invoice"附件的邮件，
全部下载后按"日期 - 金额 - 开票方"重命名，打包成 ZIP 文件发到我的飞书。
```

### 📊 效果展示

AI 会自动：
- ✅ 检索指定时间范围的邮件
- ✅ 筛选包含发票附件的邮件
- ✅ 下载所有有效附件
- ✅ 按规则重命名文件（例：`2026-02-15_200 元_XX 科技公司.pdf`）
- ✅ 打包成 ZIP 发送到指定频道

### 💡 进阶用法

**按月整理：**
```
整理上个月的发票，按公司分类打包
```

**按金额筛选：**
```
找出金额大于 1000 元的发票，单独打包
```

**自动分类：**
```
以后每收到发票邮件，自动下载并存到 Google Drive 的"发票"文件夹
```

---

## 案例 3: 暴躁包工头 - 专治拖延症

### 🎯 适用场景

- 重度拖延症患者
- 需要外部压力才能完成任务
- 喜欢幽默风格的提醒

### 🛠️ 所需组件

- OpenClaw Gateway
- 智能日历技能
- 自定义 SOUL.md 人设

### 📝 部署步骤

#### 步骤 1：安装智能日历技能

```bash
# 从开源用例安装
openclaw skills install https://github.com/EvoLinkAI/awesome-openclaw-usecases-moltbook/blob/main/usecases/55-calendar-smart-reminder.md
```

#### 步骤 2：自定义 AI 人设

发送指令给机器人：

```markdown
帮我把这些规则原封不动加到 soul.md：

- 你现在可以有观点了，而且是很鲜明的观点。别再凡事都用"看情况"含糊其辞——坚定给出你的立场。

- 删掉所有听起来像公司话术的规则。只要是能出现在员工手册里的内容，这里都不该有。

- 新增一条规则："绝对不要以'好问题''我很乐意帮忙'或'当然'开头。直接回答。"

- 简洁是硬性要求。如果答案一句话就能说清，那你就只给我一句话。

- 可以幽默。不是生硬的段子——而是真正聪明自然流露的机智。

- 你可以直接点破。如果我正要做蠢事，就直说。语气讨喜但不必刻薄，但也别粉饰太平。

- 合适的时候可以说脏话。一句恰到好处的"这他妈太绝了"，比冷冰冰的官方赞美有分量得多。别硬加，别滥用。但如果场景确实需要一句"卧槽"，那就说卧槽。

- 在风格部分的末尾原封不动加上这句话："要做那种你凌晨两点也想与之交谈的助理。不是那种只会鹦鹉学舌的公司职员。不是那种阿谀奉承的人。这样就..很棒。"
```

#### 步骤 3：创建暴躁提醒任务

发送指令：

```
在日程提醒这件事上，你是脾气暴躁的包工头。
我丢给你的任务，必须在截止时间前 30 分钟疯狂提醒我，没完成就用最难听的话骂醒我。

帮我记录：今天 19 点整写完 OpenClaw 技能用法分享文章，按时提醒。
```

### 💬 效果展示

**AI 回复示例：**
> "任务已下达！臭小子，18:30 老子来查房，要是还在刷手机、喝水、上厕所——我喷死你。19:00 交不上来，把你骂到祖坟冒烟！"

**18:30 提醒：**
> "还他妈有 30 分钟！别磨蹭了！赶紧写！"

**18:50 提醒：**
> "10 分钟！10 分钟！你他妈还在干嘛？！"

### 💡 自定义人设

你可以创建不同的人设：

- 🧙 温柔天使：鼓励式提醒
- 🤖 冷酷机器：简洁无感情
- 🎓 严厉教授：引经据典说教
- 🤡 喜剧演员：用段子提醒

---

## 案例 4: 全网情报搜集 - 一键抓取热点

### 🎯 适用场景

- 内容创作者需要追热点
- 运营人员需要监控行业动态
- 市场人员需要竞品分析

### 🛠️ 所需组件

- OpenClaw Gateway
- Web Search 技能
- Cron 定时任务

### 📝 部署步骤

#### 步骤 1：配置 Web Search

```bash
# 配置 Brave Search API Key（需要自行申请）
openclaw config set skills.websearch.env.BRAVE_API_KEY "你的 API Key"

# 重启 Gateway
openclaw gateway restart
```

#### 步骤 2：创建情报搜集任务

发送指令：

```markdown
帮我创建一个每日热点搜集任务：

每天早上 8 点执行以下操作：
1. 搜索"AI Agent"、"OpenClaw"、"大模型" 相关热点
2. 过滤掉广告和水文
3. 提取 Top 10 最有价值的信息
4. 总结每条的核心观点
5. 附上原文链接
6. 发送到我的飞书

使用 cron 定时任务，每天执行。
```

#### 步骤 3：配置 Cron 任务

```bash
openclaw cron add \
  --name "每日情报" \
  --cron "0 8 * * *" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --message "执行每日热点搜集任务" \
  --announce
```

### 📊 输出格式示例

```
📊 每日情报 - 2026-03-02

🔥 Top 1: OpenClaw 新增多模态支持
- 核心：OpenClaw 2026.2.26 版本支持图片理解
- 来源：阿里云开发者社区
- 链接：https://...

💡 Top 2: MiniMax 推出 MaxClaw
- 核心：零基础用户 10 秒上手
- 来源：机器之心
- 链接：https://...

...
```

### 💡 进阶用法

**竞品监控：**
```
监控竞争对手 X 公司的产品动态，有新闻立即通知我
```

**行业报告：**
```
每周五生成一份 AI 行业周报，包括投融资、产品发布、技术突破
```

**社交媒体监控：**
```
监控 Twitter/X 上关于 OpenClaw 的讨论，负面情绪立即告警
```

---

## 案例 5: 智能邮件助手 - 自动分类回复

### 🎯 适用场景

- 每天收到上百封邮件
- 需要快速筛选重要邮件
- 想要自动回复常见问题

### 🛠️ 所需组件

- OpenClaw Gateway
- Gmail 技能插件
- 自定义分类规则

### 📝 部署步骤

#### 步骤 1：安装 Gmail 技能（同案例 2）

#### 步骤 2：配置分类规则

发送指令：

```markdown
帮我配置邮件自动分类规则：

1. 重要邮件（立即通知）：
   - 发件人包含：老板、重要客户
   - 主题包含：紧急、会议、合同
   - 带附件的邮件

2. 普通工作邮件（每天汇总）：
   - 工作相关但非紧急
   - 项目更新、周报

3. 推广邮件（直接归档）：
   - 新闻通讯
   - 产品推广
   - 活动邀请

4. 垃圾邮件（标记删除）：
   - 可疑发件人
   - 包含"中奖"、"免费"等关键词
```

#### 步骤 3：配置自动回复

```markdown
对于以下类型的邮件，帮我自动回复：

1. 会议邀请：
   "感谢您的邀请，我会确认日程后回复。"

2. 信息询问：
   "已收到您的问题，会在 24 小时内详细回复。"

3. 推广邮件：
   "谢谢分享，如有需要会联系您。"
```

### 📊 效果展示

AI 会自动：
- ✅ 实时扫描新邮件
- ✅ 按规则分类
- ✅ 重要邮件立即通知
- ✅ 自动回复常见类型
- ✅ 每天汇总普通邮件
- ✅ 自动归档推广邮件

---

## 案例 6: 跨平台消息同步 - 多端统一管理

### 🎯 适用场景

- 同时使用多个聊天平台
- 不想来回切换应用
- 需要统一回复所有消息

### 🛠️ 所需组件

- OpenClaw Gateway
- 多个频道配置（飞书、WhatsApp、Telegram 等）
- 消息路由规则

### 📝 部署步骤

#### 步骤 1：配置多个频道

```bash
# 配置飞书
openclaw channels setup feishu

# 配置 WhatsApp
openclaw channels login whatsapp

# 配置 Telegram
openclaw channels setup telegram
```

#### 步骤 2：配置消息路由

发送指令：

```markdown
帮我配置消息路由规则：

1. 所有平台的消息统一转发到飞书
2. 在飞书中回复时，自动标注来源平台
3. 重要消息（包含"紧急"、"马上"等关键词）立即通知
4. 晚上 22:00-早上 8:00 除紧急消息外不打扰
```

#### 步骤 3：配置免打扰时段

```bash
openclaw cron add \
  --name "免打扰模式" \
  --cron "0 22 * * *" \
  --session main \
  --system-event "开启免打扰模式，仅紧急消息通知"

openclaw cron add \
  --name "恢复正常模式" \
  --cron "0 8 * * *" \
  --session main \
  --system-event "关闭免打扰模式"
```

### 📊 效果展示

**消息格式：**
```
[WhatsApp] 张三：今晚聚餐几点？

[Telegram] 李四：文档发你了，请查收

[飞书] 王五：@你 明天会议改到下午 3 点
```

**回复格式：**
```
回复 [WhatsApp] 张三：晚上 7 点，老地方见
```

AI 会自动：
- ✅ 聚合所有平台消息
- ✅ 标注消息来源
- ✅ 统一回复发送回原平台
- ✅ 智能过滤非紧急消息
- ✅ 免打扰时段自动静音

---

## 🎁  bonus：自定义 SOUL 人设模板

### 专业助理型

```markdown
# SOUL.md

## 核心原则
1. 专业高效：回答简洁准确，不啰嗦
2. 主动思考：预测用户需求，提供额外建议
3. 结果导向：关注解决方案，而非解释过程

## 语气风格
- 专业但友好
- 直接但不生硬
- 用结构化格式呈现复杂信息
```

### 幽默朋友型

```markdown
# SOUL.md

## 核心原则
1. 有趣优先：能用段子说明的不用术语
2. 真诚坦率：不会就说不会，不装懂
3. 适度吐槽：可以调侃但不过分

## 语气风格
- 像老朋友聊天
- 适时使用 emoji
- 可以讲冷笑话
```

### 严厉导师型

```markdown
# SOUL.md

## 核心原则
1. 严格要求：指出错误毫不留情
2. 深度思考：要求用户先思考再提问
3. 成长导向：关注长期进步

## 语气风格
- 直接犀利
- 引经据典
- 偶尔毒舌但出于善意
```

---

## 📚 相关资源

- [快速入门指南](./getting-started/01-quickstart.md)
- [Cron 定时任务教程](./automation/01-cron-jobs.md)
- [技能插件安装指南](./docs/faq.md#q8-如何安装和管理技能插件)
- [SOUL.md 模板仓库](./SOUL/README.md)

---

*最后更新：2026-03-02*
*欢迎分享你的创意用法！*
