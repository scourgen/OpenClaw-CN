# XiaohongshuSkills - 小红书自动化发布技能

> 支持小红书自动发布、自动评论、自动检索的 OpenClaw 技能
> 
> **项目来源：** https://github.com/white0dew/XiaohongshuSkills
> 
> *难度：⭐⭐⭐ 中级 | 适用场景：自媒体运营、内容营销*

---

## 📖 目录

1. [功能概览](#功能概览)
2. [快速开始](#快速开始)
3. [核心功能详解](#核心功能详解)
4. [多账号管理](#多账号管理)
5. [作为 OpenClaw 技能使用](#作为-openclaw-技能使用)
6. [实战案例](#实战案例)
7. [注意事项](#注意事项)

---

## 功能概览

### ✨ 核心功能

| 功能 | 说明 | 状态 |
|------|------|------|
| 📤 **自动发布** | 自动填写标题、正文、上传图片 | ✅ |
| 🏷️ **话题标签** | 自动识别并写入话题标签 | ✅ |
| 📝 **自动评论** | 对指定笔记发表评论 | ✅ |
| 🔍 **内容检索** | 搜索笔记并获取详情 | ✅ |
| 📊 **数据看板** | 抓取曝光/观看/点赞等数据 | ✅ |
| 🔔 **通知抓取** | 抓取评论和@通知 | ✅ |
| 👥 **多账号** | 支持多账号管理，Cookie 隔离 | ✅ |
| 🎭 **无头模式** | 后台运行，无需显示浏览器 | ✅ |

### 🛠️ 技术特点

- **基于 CDP：** 通过 Chrome DevTools Protocol 实现自动化
- **登录态缓存：** 默认缓存 12 小时，减少重复登录
- **远程 CDP：** 支持连接远程 Chrome 调试端口
- **图片下载：** 自动添加 Referer 绕过防盗链
- **跨平台：** 支持 Windows、WSL、远程 CDP

---

## 快速开始

### 前置要求

- ✅ Python 3.10+
- ✅ Google Chrome 浏览器
- ✅ Windows 操作系统（目前仅测试 Windows）

### 步骤 1：安装依赖

```bash
# 克隆项目
git clone https://github.com/white0dew/XiaohongshuSkills.git
cd XiaohongshuSkills

# 安装依赖
pip install -r requirements.txt
```

### 步骤 2：登录小红书

```bash
# 启动登录流程
python scripts/cdp_publish.py login
```

在弹出的 Chrome 窗口中扫码登录小红书。

**验证登录状态：**
```bash
python scripts/cdp_publish.py check-login
```

### 步骤 3：发布第一篇笔记

#### 方式 1：有窗口预览模式（推荐新手）

```bash
python scripts/publish_pipeline.py \
  --preview \
  --title "我的第一篇笔记" \
  --content "这是正文内容\n#新人报到 #自我介绍" \
  --image-urls "https://example.com/image.jpg"
```

`--preview` 模式会填充内容但不自动点击发布，适合确认效果。

#### 方式 2：无头模式自动发布（推荐老手）

```bash
python scripts/publish_pipeline.py \
  --headless \
  --title "我的第一篇笔记" \
  --content "这是正文内容\n#新人报到 #自我介绍" \
  --image-urls "https://example.com/image.jpg"
```

`--headless` 模式后台运行，自动完成发布。

---

## 核心功能详解

### 1️⃣ 自动发布

#### 基本用法

```bash
# 从命令行参数读取
python scripts/publish_pipeline.py --headless \
  --title "春招求职指南" \
  --content "春招来了，分享一些求职经验...\n#春招 #求职 #校招" \
  --image-urls "https://example.com/img1.jpg"

# 从文件读取内容
python scripts/publish_pipeline.py --headless \
  --title-file title.txt \
  --content-file content.txt \
  --image-urls "https://example.com/image.jpg"
```

#### 使用本地图片

```bash
# Windows 路径
python scripts/publish_pipeline.py --headless \
  --title "笔记标题" \
  --content "笔记正文" \
  --images "C:\Users\Name\Pictures\image.jpg"

# WSL/远程 CDP + UNC 路径
python scripts/publish_pipeline.py --headless \
  --title "笔记标题" \
  --content "笔记正文" \
  --images "\\wsl.localhost\Ubuntu\home\user\image.jpg" \
  --skip-file-check
```

#### 话题标签自动处理

**规则：**
- 自动识别正文最后一行的 `#标签`
- 如果最后一行全部由 `#标签` 组成，则提取为话题标签
- 从正文中移除该行，改为逐个输入到小红书话题框
- 每个标签输入后等待 3 秒，然后按 Enter 确认
- 建议 1-10 个标签

**示例 content.txt：**
```
春招求职经验分享，希望对大家有帮助...

#春招 #26 届 #校招 #求职 #找工作 #春招规划 #面试技巧
```

---

### 2️⃣ 内容检索

#### 搜索笔记

```bash
# 基础搜索
python scripts/cdp_publish.py search-feeds --keyword "春招"

# 指定排序方式
python scripts/cdp_publish.py search-feeds \
  --keyword "春招" \
  --sort-by 最新 \
  --note-type 图文

# 输出包含：
# - feed_id: 笔记 ID
# - xsec_token: 安全令牌（用于获取详情）
# - title: 标题
# - desc: 描述
# - recommended_keywords: 下拉推荐词
```

**排序选项：**
- 综合（默认）
- 最新
- 最热

**笔记类型：**
- 图文
- 视频
- 全部（默认）

#### 获取笔记详情

```bash
python scripts/cdp_publish.py get-feed-detail \
  --feed-id 67abc1234def567890123456 \
  --xsec-token YOUR_XSEC_TOKEN
```

**返回数据包含：**
- 笔记完整内容
- 作者信息
- 点赞、收藏、评论数
- 评论列表（包含评论 ID、用户、内容等）

---

### 3️⃣ 自动评论

```bash
python scripts/cdp_publish.py post-comment-to-feed \
  --feed-id 67abc1234def567890123456 \
  --xsec-token YOUR_XSEC_TOKEN \
  --content "写得很实用，感谢分享！"
```

**注意：**
- 仅支持一级评论（直接评论笔记）
- 不支持回复其他评论

---

### 4️⃣ 通知抓取

```bash
# 抓取"评论和@"通知
python scripts/cdp_publish.py get-notification-mentions
```

**返回数据：**
- 通知类型（评论、@、点赞等）
- 通知内容
- 相关用户信息
- 时间戳

---

### 5️⃣ 数据看板

```bash
# 抓取笔记基础信息
python scripts/cdp_publish.py content-data

# 导出为 CSV
python scripts/cdp_publish.py content-data \
  --csv-file "/abs/path/content_data.csv"
```

**数据包含：**
- 笔记标题
- 曝光数
- 观看数
- 点赞数
- 收藏数
- 评论数
- 分享数
- 发布时间

**支持导出格式：**
- CSV（默认）
- JSON（可选）

---

## 多账号管理

### 账号管理命令

```bash
# 列出所有账号
python scripts/cdp_publish.py list-accounts

# 添加新账号
python scripts/cdp_publish.py add-account myaccount --alias "我的账号"

# 登录指定账号
python scripts/cdp_publish.py --account myaccount login

# 使用指定账号发布
python scripts/publish_pipeline.py --account myaccount --headless \
  --title "标题" \
  --content "正文" \
  --image-urls "URL"

# 设置默认账号
python scripts/cdp_publish.py set-default-account myaccount

# 切换账号（清除当前登录，重新扫码）
python scripts/cdp_publish.py switch-account

# 删除账号
python scripts/cdp_publish.py remove-account myaccount --delete-profile
```

### 多账号使用场景

**场景 1：矩阵运营**
```bash
# 账号 A 发布职场内容
python scripts/publish_pipeline.py --account career_account --headless \
  --title "面试技巧分享" \
  --content-file career.txt \
  --images career_img.jpg

# 账号 B 发布生活内容
python scripts/publish_pipeline.py --account life_account --headless \
  --title "周末探店" \
  --content-file life.txt \
  --images life_img.jpg
```

**场景 2：A/B 测试**
```bash
# 同一内容在不同账号测试效果
# 账号 A：早上 9 点发布
# 账号 B：晚上 8 点发布
# 对比数据看板分析最佳发布时间
```

---

## 作为 OpenClaw 技能使用

### 安装步骤

1. **复制项目到技能目录**
```bash
# 在 OpenClaw 工作区
mkdir -p ~/.openclaw/skills/xiaohongshu
cd ~/.openclaw/skills/xiaohongshu

# 克隆项目
git clone https://github.com/white0dew/XiaohongshuSkills.git .
```

2. **安装依赖**
```bash
pip install -r requirements.txt
```

3. **创建 SKILL.md 文件**
```markdown
---
name: xiaohongshu
version: 1.0.0
description: 小红书自动化发布和运营技能
author: white0dew
license: MIT
---

# Xiaohongshu Skill

支持小红书自动发布、评论、检索的 OpenClaw 技能。

## 工具列表

### publish_note

发布笔记到小红书。

**参数：**
- `title` (string, 必需): 笔记标题
- `content` (string, 必需): 笔记正文
- `image_urls` (array, 可选): 图片 URL 列表
- `account` (string, 可选): 使用的账号名称
- `headless` (boolean, 可选): 是否无头模式，默认 true

**返回：**
```json
{
  "success": true,
  "message": "发布成功",
  "note_id": "123456"
}
```

### search_notes

搜索小红书笔记。

**参数：**
- `keyword` (string, 必需): 搜索关键词
- `sort_by` (string, 可选): 排序方式（综合/最新/最热）
- `note_type` (string, 可选): 笔记类型（图文/视频/全部）

**返回：**
```json
{
  "feeds": [...],
  "recommended_keywords": ["..."]
}
```

### post_comment

对指定笔记发表评论。

**参数：**
- `feed_id` (string, 必需): 笔记 ID
- `xsec_token` (string, 必需): 安全令牌
- `content` (string, 必需): 评论内容

**返回：**
```json
{
  "success": true,
  "comment_id": "789012"
}
```

### get_note_stats

获取笔记统计数据。

**参数：**
- `feed_id` (string, 必需): 笔记 ID
- `xsec_token` (string, 必需): 安全令牌

**返回：**
```json
{
  "views": 1000,
  "likes": 50,
  "collects": 20,
  "comments": 15
}
```

## 配置说明

使用前需要：
1. 登录小红书账号：`python scripts/cdp_publish.py login`
2. 确保 Chrome 浏览器可用
3. 配置远程 CDP（如需要）

## 使用示例

```
"帮我发布一篇小红书笔记，标题'春招指南'，内容'分享求职经验...'，标签#春招 #求职"
"搜索一下'春招规划'相关的热门笔记"
"给这篇笔记点个赞并评论'很有用'"
"查看我昨天发布的笔记数据"
```
```

4. **配置 OpenClaw**
```bash
openclaw skills install ~/.openclaw/skills/xiaohongshu
openclaw skills info xiaohongshu
```

---

## 实战案例

### 案例 1：自媒体日常运营

**场景：** 每天发布 3-5 篇笔记，覆盖不同时段

**自动化脚本：**
```bash
#!/bin/bash

# 早上 9 点发布
python scripts/publish_pipeline.py --headless \
  --account main_account \
  --title-file morning_title.txt \
  --content-file morning_content.txt \
  --images morning_img.jpg

# 下午 2 点发布
python scripts/publish_pipeline.py --headless \
  --account main_account \
  --title-file afternoon_title.txt \
  --content-file afternoon_content.txt \
  --images afternoon_img.jpg

# 晚上 8 点发布
python scripts/publish_pipeline.py --headless \
  --account main_account \
  --title-file evening_title.txt \
  --content-file evening_content.txt \
  --images evening_img.jpg
```

**配合 Cron 定时任务：**
```bash
# 添加到 crontab
0 9 * * * cd /path/to/XiaohongshuSkills && bash auto_publish.sh morning
0 14 * * * cd /path/to/XiaohongshuSkills && bash auto_publish.sh afternoon
0 20 * * * cd /path/to/XiaohongshuSkills && bash auto_publish.sh evening
```

---

### 案例 2：竞品分析

**场景：** 监控竞品账号，分析爆款笔记

**步骤：**

1. **搜索竞品内容**
```bash
python scripts/cdp_publish.py search-feeds \
  --keyword "竞品关键词" \
  --sort-by 最热
```

2. **获取爆款笔记详情**
```bash
python scripts/cdp_publish.py get-feed-detail \
  --feed-id FEED_ID \
  --xsec-token XSEC_TOKEN
```

3. **导出数据分析**
```bash
# 导出自己账号的数据
python scripts/cdp_publish.py content-data \
  --csv-file "my_notes.csv"

# 手动整理竞品数据
# 对比分析标题、内容、标签、发布时间等
```

4. **生成分析报告**
```python
import pandas as pd

# 读取数据
df = pd.read_csv("my_notes.csv")

# 分析
print(f"平均曝光：{df['曝光数'].mean()}")
print(f"平均点赞：{df['点赞数'].mean()}")
print(f"最佳发布时间：{df.groupby('发布时间')['曝光数'].mean().idxmax()}")
```

---

### 案例 3：互动维护

**场景：** 自动回复评论，提升账号活跃度

**配合 OpenClaw：**

在 SOUL.md 中配置：
```markdown
## 小红书评论回复规则

当用户说"回复小红书的评论"时：

1. 先抓取通知：
   `python scripts/cdp_publish.py get-notification-mentions`

2. 分析评论内容，判断需要回复的：
   - 提问类：详细解答
   - 赞美类：感谢 + 引导互动
   - 质疑类：礼貌解释
   - 广告类：忽略

3. 对需要回复的评论：
   `python scripts/cdp_publish.py post-comment-to-feed ...`

4. 记录回复日志
```

**使用示例：**
```
"查看我有哪些新评论，并回复需要互动的"
```

---

### 案例 4：矩阵账号管理

**场景：** 运营 3-5 个垂直领域账号

**账号配置：**
```bash
# 职场账号
python scripts/cdp_publish.py add-account career --alias "职场成长"

# 生活账号
python scripts/cdp_publish.py add-account life --alias "生活日记"

# 学习账号
python scripts/cdp_publish.py add-account study --alias "学习笔记"

# 设置默认账号
python scripts/cdp_publish.py set-default-account career
```

**批量发布脚本：**
```bash
#!/bin/bash

# 职场内容
python scripts/publish_pipeline.py --account career --headless \
  --title "面试必问的 10 个问题" \
  --content-file career/content1.txt \
  --images career/img1.jpg

# 生活内容
python scripts/publish_pipeline.py --account life --headless \
  --title "周末探店|这家咖啡馆绝了" \
  --content-file life/content1.txt \
  --images life/img1.jpg

# 学习内容
python scripts/publish_pipeline.py --account study --headless \
  --title "Python 学习路线总结" \
  --content-file study/content1.txt \
  --images study/img1.jpg
```

**数据汇总：**
```bash
# 导出所有账号数据
python scripts/cdp_publish.py content-data --csv-file "career_data.csv"
# 切换账号
python scripts/cdp_publish.py switch-account
python scripts/cdp_publish.py content-data --csv-file "life_data.csv"
# ...

# 汇总分析
python analyze_all_accounts.py
```

---

## 注意事项

### ⚠️ 平台规则

1. **遵守社区规范**
   - 仅用于学习研究和合规运营
   - 不要发布违规内容
   - 不要恶意刷量、刷评

2. **发布频率限制**
   - 建议每天发布不超过 5 篇
   - 每篇间隔至少 30 分钟
   - 避免短时间内大量发布

3. **内容质量**
   - 确保内容原创或有价值
   - 不要完全依赖自动化
   - 人工审核后再发布

### 🔒 安全事项

1. **Cookie 安全**
   - Cookie 存储在本地 Chrome Profile 中
   - 不要分享或上传 Cookie 文件
   - 定期清理不再使用的账号

2. **登录态缓存**
   - 默认缓存 12 小时
   - 敏感操作前检查登录状态
   - 定期检查账号安全

3. **远程 CDP**
   - 仅连接可信的远程 Chrome
   - 使用防火墙限制访问
   - 不要在公网暴露调试端口

### 🛠️ 技术注意

1. **选择器更新**
   - 小红书页面结构变化可能导致发布失败
   - 需要定期更新 `cdp_publish.py` 中的选择器
   - 关注项目更新

2. **图片类型**
   - 支持 WB_PRV 等格式
   - 确保图片尺寸符合平台要求
   - 建议使用高质量图片

3. **网络问题**
   - 确保网络稳定
   - 图片下载可能需要代理
   - 远程 CDP 需要良好网络连接

### 📝 最佳实践

1. **测试先行**
   - 新脚本先用 `--preview` 模式测试
   - 确认效果后再自动发布
   - 小批量测试后再扩大规模

2. **日志记录**
   - 记录每次发布的内容和时间
   - 保存发布结果和错误信息
   - 便于问题排查和优化

3. **备份策略**
   - 定期备份账号配置
   - 保存重要内容草稿
   - 导出数据分析结果

---

## 故障排除

### 常见问题

**Q1: 发布失败，提示选择器找不到**
- A: 小红书页面更新了，需要更新 `cdp_publish.py` 中的选择器
- 解决：查看项目 Issues 或提交 Bug 报告

**Q2: 登录态过期**
- A: 登录态缓存已过期（默认 12 小时）
- 解决：重新运行 `python scripts/cdp_publish.py login`

**Q3: 图片上传失败**
- A: 可能是网络问题或图片格式不支持
- 解决：检查网络连接，确认图片格式，添加 Referer

**Q4: 多账号切换失败**
- A: Cookie 冲突或缓存问题
- 解决：清除 Chrome Profile，重新登录

---

## 相关资源

- **项目地址：** https://github.com/white0dew/XiaohongshuSkills
- **OpenClaw 文档：** https://docs.openclaw.ai/tools/skills
- **小红书创作者中心：** https://creator.xiaohongshu.com/

---

*本文档基于项目 README 整理，详细用法请参考原项目*
*最后更新：2026-03-02*
