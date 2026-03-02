# OpenClaw 网络搜索技能汇总

> 搜索 X/Twitter、Google、Medium、博客等各种网络资源的技能集合
> 
> *最后更新：2026-03-02*

---

## 🎯 快速推荐

| 需求 | 推荐技能 | 难度 | 成本 |
|------|---------|------|------|
| **搜索 X/Twitter** | [xint](#1-xint---x-twitter-情报工具) | ⭐⭐ | 免费 + API |
| **全能网页抓取** | [Decodo](#2-decodo---全能网页抓取) | ⭐ | 免费额度 + 付费 |
| **Google 搜索** | Decodo google_search 工具 | ⭐ | 同上 |
| **YouTube 字幕** | Decodo youtube_subtitles | ⭐ | 同上 |
| **Reddit 监控** | Decodo reddit_post/reddit_subreddit | ⭐ | 同上 |
| **Amazon 比价** | Decodo amazon/amazon_search | ⭐ | 同上 |

---

## 1️⃣ xint - X (Twitter) 情报工具

**GitHub:** https://github.com/0xNyk/xint

**定位：** 专业的 X/Twitter 搜索和监控 CLI 工具，AI Agent 技能

### ✨ 核心功能

| 功能 | 命令 | 说明 |
|------|------|------|
| 🔍 **全文搜索** | `xint search "query"` | 搜索 X 上的任意内容 |
| 📊 **实时监控** | `xint watch "query" -i 5m` | 每 5 分钟监控一次 |
| 📡 **实时流** | `xint stream` | 连接 Twitter 实时流 |
| 👤 **用户分析** | `xint profile @username` | 获取用户资料 |
| 🧵 **主题追踪** | `xint thread <tweet_id>` | 抓取完整推文串 |
| 📈 **粉丝变化** | `xint diff @username` | 追踪粉丝增减 |
| 🎯 **趋势分析** | `xint trends` | 获取热门话题 |
| 🤖 **AI 情感分析** | `xint analyze "query"` | AI 分析情感倾向 |
| 📰 **文章总结** | `xint article <url> --ai "summarize"` | AI 总结文章 |
| 📊 **生成报告** | `xint report "topic"` | 生成主题分析报告 |

### 🛠️ 安装方法

#### 方式 1：一键安装（推荐）

```bash
curl -fsSL https://raw.githubusercontent.com/0xNyk/xint/main/install.sh | bash
```

#### 方式 2：Homebrew（macOS）

```bash
brew tap 0xNyk/xint
brew install xint
```

#### 方式 3：源码安装

```bash
git clone https://github.com/0xNyk/xint.git
cd xint
bun install
```

需要：[Bun](https://bun.sh) 运行时

### ⚙️ 配置认证

**必需的环境变量：**

```bash
# X API Bearer Token（必需）
export X_BEARER_TOKEN="your_bearer_token"

# xAI API Key（用于 AI 分析功能）
export XAI_API_KEY="your_xai_key"

# OAuth 认证（用于书签、列表等）
xint auth setup
```

**获取 X Bearer Token：**
1. 访问 https://developer.x.com
2. 创建/选择你的应用
3. 在 App Settings 中找到 Bearer Token

### 💬 使用示例

#### 作为 OpenClaw 技能

```bash
# 克隆技能
git clone https://github.com/0xNyk/xint.git
cd xint

# 安装为 OpenClaw 技能
openclaw skills install .

# 配置环境变量
openclaw config set skills.xint.env.X_BEARER_TOKEN "your_token"
openclaw config set skills.xint.env.XAI_API_KEY "your_xai_key"

# 重启 Gateway
openclaw gateway restart
```

#### 搜索示例

```bash
# 基础搜索
xint search "AI agents"

# 高互动内容（最近 1 小时，至少 50 点赞）
xint search "react 19" --since 1h --sort likes --min-likes 50

# 全文搜索（历史所有数据）
xint search "bitcoin ETF" --full --pages 3

# 带情感分析
xint search "solana" --sentiment

# 导出 CSV
xint search "startups" --csv > data.csv

# 导出 JSONL（适合 AI 处理）
xint search "AI" --jsonl | jq '.text'
```

#### 监控示例

```bash
# 每 5 分钟监控一次
xint watch "solana" --interval 5m

# 监控特定用户
xint watch "@vitalikbuterin" -i 1m

# 发送到 Webhook（如 Slack）
xint watch "breaking" -i 30s --webhook https://hooks.slack.com/xxx
```

#### AI 分析示例

```bash
# 分析某个话题的情感
xint analyze "best AI frameworks?"

# 生成报告
xint report "crypto" --sentiment --accounts @aaboronkov,@solana

# 总结文章
xint article "https://example.com" --ai "Key takeaways?"
```

### 📊 输出格式

**快速模式：**
```bash
xint search "AI agents" --quick
```

**详细 JSON：**
```bash
xint search "AI" --jsonl
```

**CSV 导出：**
```bash
xint search "startups" --csv > data.csv
```

### ⚠️ 注意事项

- **API 配额：** X API 需要预付费积分，查看 https://developer.x.com/pricing
- **速率限制：** 注意 API 调用频率限制
- **数据准确性：** 免费 API 可能有数据延迟

---

## 2️⃣ Decodo - 全能网页抓取工具

**GitHub:** https://github.com/Decodo/decodo-openclaw-skill

**定位：** 基于 Decodo API 的全能网页抓取技能，支持 Google、YouTube、Reddit、Amazon 等

### ✨ 核心功能

| 工具 | 说明 | 适用场景 |
|------|------|---------|
| **google_search** | Google 搜索结果抓取 | 市场调研、竞品分析、新闻监控 |
| **universal** | 任意网页转 Markdown | 文章总结、内容聚合、数据集构建 |
| **amazon** | Amazon 商品页解析 | 价格追踪、竞品分析 |
| **amazon_search** | Amazon 商品搜索 | 产品调研、趋势追踪 |
| **youtube_subtitles** | YouTube 字幕提取 | 视频总结、内容分析 |
| **reddit_post** | Reddit 帖子抓取 | 社区监控、用户反馈 |
| **reddit_subreddit** | Reddit 版块监控 | 行业动态、小众市场研究 |

### 🚀 优势特点

- ✅ **零封锁** - 1.25 亿 + 住宅和数据中心代理，自动绕过 CAPTCHA
- ✅ **实时数据** - 获取最新网页内容
- ✅ **LLM 优化输出** - 结构化 JSON 或干净 Markdown
- ✅ **高可扩展** - 从小任务到大规模采集
- ✅ **简单配置** - 单一 Token 认证
- ✅ **JavaScript 渲染** - 自动处理动态网页
- ✅ **地理位置定位** - 支持指定国家/地区

### 🛠️ 安装步骤

#### 步骤 1：克隆仓库

```bash
git clone https://github.com/Decodo/decodo-openclaw-skill.git
cd decodo-openclaw-skill
```

#### 步骤 2：安装依赖

```bash
pip install -r requirements.txt
```

需要 Python 3.9+

#### 步骤 3：获取 Decodo Token

1. 访问 https://dashboard.decodo.com/
2. 注册/登录账号
3. 在 Dashboard 获取 Web Scraping API Token
4. Token 是 Base64 格式

#### 步骤 4：配置环境变量

**Linux/macOS:**
```bash
export DECODO_AUTH_TOKEN="your_base64_token"
```

**Windows (PowerShell):**
```powershell
$env:DECODO_AUTH_TOKEN="your_base64_token"
```

**或创建 .env 文件：**
```bash
DECODO_AUTH_TOKEN=your_base64_token
```

#### 步骤 5：安装为 OpenClaw 技能

```bash
# 在技能目录中
cd decodo-openclaw-skill
openclaw skills install .

# 配置 Token
openclaw config set skills.decodo.env.DECODO_AUTH_TOKEN "your_token"

# 重启 Gateway
openclaw gateway restart
```

### 💬 使用示例

#### 1. Google 搜索

```bash
# 搜索并获取结构化 JSON
python3 tools/scrape.py --target google_search --query "AI agent frameworks 2026"
```

**输出包含：**
- organic（自然搜索结果）
- ai_overviews（AI 生成的摘要）
- paid（广告）
- related_questions（相关问题）
- related_searches（相关搜索）
- discussions_and_forums（讨论和论坛）

#### 2. 通用网页抓取

```bash
# 抓取任意网页为 Markdown
python3 tools/scrape.py --target universal --url "https://example.com/article"
```

**适用场景：**
- 新闻文章总结
- 博客内容聚合
- 技术文档整理
- 竞争对手网站监控

#### 3. YouTube 字幕提取

```bash
# 提取视频字幕（使用视频 ID）
python3 tools/scrape.py --target youtube_subtitles --query "dFu9aKJoqGg"
```

**视频 ID 获取：**
从 URL `https://www.youtube.com/watch?v=dFu9aKJoqGg` 中提取 `dFu9aKJoqGg`

**适用场景：**
- 视频内容总结
- 教程文字版整理
- 多语言字幕获取
- 可访问性改进

#### 4. Reddit 监控

```bash
# 抓取单个帖子
python3 tools/scrape.py --target reddit_post --url "https://www.reddit.com/r/nba/comments/xxx"

# 抓取整个版块
python3 tools/scrape.py --target reddit_subreddit --url "https://www.reddit.com/r/opensource/"
```

**适用场景：**
- 社区舆情监控
- 产品反馈收集
- 行业趋势发现
- 用户痛点分析

#### 5. Amazon 商品分析

```bash
# 解析商品页面
python3 tools/scrape.py --target amazon --url "https://www.amazon.com/dp/B09H74FXNW"

# 搜索商品
python3 tools/scrape.py --target amazon_search --query "laptop"
```

**输出包含：**
- 价格、评分、评论数
- 产品规格
- ASIN 编号
- 卖家信息

### 💰 定价说明

Decodo 提供：
- **免费额度：** 新用户注册有一定免费额度
- **付费套餐：** 根据请求量计费
- **企业方案：** 大规模采集需求

查看最新定价：https://dashboard.decodo.com/scrapers/pricing

### 🔗 相关资源

- **官方文档：** https://help.decodo.com/docs/web-scraping-api-introduction
- **Discord 社区：** https://discord.gg/Ja8dqKgvbZ
- **OpenClaw 文档：** https://docs.openclaw.ai/start/getting-started

---

## 3️⃣ 其他相关技能

### Web Search 技能（需要 Brave API）

如果配置了 Brave Search API，可以使用 OpenClaw 内置的 web_search 工具：

```bash
# 配置 Brave API
openclaw configure --section web

# 在对话中使用
"搜索一下最新的 AI Agent 框架"
```

### GitHub Explorer 技能

**GitHub:** https://github.com/blessonism/github-explorer-skill

用于深度分析 GitHub 项目：

```bash
git clone https://github.com/blessonism/github-explorer-skill.git
cd github-explorer-skill
openclaw skills install .
```

**功能：**
- 项目多维度分析
- 代码质量评估
- 贡献者分析
- 技术栈识别

---

## 🎯 实战组合

### 场景 1：全网舆情监控

```bash
# 1. 监控 X 上的讨论
xint watch "OpenClaw" -i 30m

# 2. 搜索 Google 新闻
python3 tools/scrape.py --target google_search --query "OpenClaw news"

# 3. 监控 Reddit 讨论
python3 tools/scrape.py --target reddit_subreddit --url "https://www.reddit.com/r/opensource/"
```

### 场景 2：竞品分析

```bash
# 1. Google 搜索竞品
python3 tools/scrape.py --target google_search --query "competitor_name reviews"

# 2. 抓取竞品官网
python3 tools/scrape.py --target universal --url "https://competitor.com"

# 3. 监控 X 上的讨论
xint search "competitor_name" --sentiment

# 4. 查看 Amazon 评价（如有）
python3 tools/scrape.py --target amazon_search --query "competitor product"
```

### 场景 3：内容创作研究

```bash
# 1. 搜索热门话题
xint trends

# 2. Google 搜索深度内容
python3 tools/scrape.py --target google_search --query "AI agent tutorial 2026"

# 3. 抓取 Medium 文章
python3 tools/scrape.py --target universal --url "https://medium.com/@author/article"

# 4. YouTube 视频调研
python3 tools/scrape.py --target youtube_subtitles --query "video_id"
```

### 场景 4：市场调研

```bash
# 1. 行业趋势搜索
xint search "industry trend 2026" --full

# 2. Google 学术/新闻
python3 tools/scrape.py --target google_search --query "market research report"

# 3. Reddit 用户反馈
python3 tools/scrape.py --target reddit_subreddit --url "https://www.reddit.com/r/YourIndustry/"

# 4. 生成综合报告
xint report "industry analysis" --sentiment
```

---

## 📚 学习资源

### 官方文档

- [xint 完整文档](https://github.com/0xNyk/xint/blob/main/README.md)
- [Decodo API 文档](https://help.decodo.com/docs/web-scraping-api-introduction)
- [OpenClaw 技能开发](https://docs.openclaw.ai/cli/skills.md)

### 视频教程

- xint 使用教程（待补充）
- Decodo 快速上手（待补充）

### 社区支持

- OpenClaw-CN Issues: https://github.com/scourgen/OpenClaw-CN/issues
- OpenClaw 官方 Discord: https://discord.gg/clawd
- Decodo Discord: https://discord.gg/Ja8dqKgvbZ

---

## 🔧 故障排除

### xint 常见问题

**Q: 提示 API 认证失败**
- A: 检查 `X_BEARER_TOKEN` 是否正确，确保未过期

**Q: 搜索结果为空**
- A: 可能是查询词太具体，尝试更宽泛的关键词

**Q: 速率限制错误**
- A: X API 有调用限制，降低频率或升级套餐

### Decodo 常见问题

**Q: Token 无效**
- A: 确认 Token 是 Base64 格式，检查是否复制完整

**Q: 抓取失败**
- A: 检查 URL 是否正确，某些网站可能有反爬机制

**Q: 输出格式混乱**
- A: 尝试使用 `--format json` 或 `--format markdown` 指定格式

---

## 📊 成本对比

| 工具 | 免费额度 | 付费起点 | 适合场景 |
|------|---------|---------|---------|
| **xint** | 有限 API 调用 | $100 起 | X/Twitter 专业用户 |
| **Decodo** | 新用户额度 | 按量计费 | 全能网页抓取 |
| **Brave Search** | 2000 次/月 | $3/月 | 基础搜索需求 |

**省钱建议：**
1. 优先使用免费额度测试
2. 合理设置监控频率
3. 批量处理而非实时调用
4. 使用缓存减少重复请求

---

*欢迎贡献更多搜索技能推荐！*
*最后更新：2026-03-02*
