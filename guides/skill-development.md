# OpenClaw 技能开发入门教程

> 从零开始创建你的第一个 OpenClaw 技能
> 
> 难度：⭐⭐⭐ 中级 | 预计耗时：1-2 小时

---

## 📖 教程目录

1. [什么是 OpenClaw 技能](#什么是-openclaw-技能)
2. [技能结构解析](#技能结构解析)
3. [创建第一个技能](#创建第一个技能)
4. [技能测试与调试](#技能测试与调试)
5. [发布到 ClawHub](#发布到-clawhub)
6. [最佳实践](#最佳实践)

---

## 什么是 OpenClaw 技能

### 定义

OpenClaw 技能是一个**可复用的功能模块**，让 AI Agent 能够执行特定任务或访问外部服务。

### 类比理解

| 概念 | 类比 | 说明 |
|------|------|------|
| OpenClaw | 智能手机操作系统 | 提供基础运行环境 |
| 技能 | App 应用 | 提供特定功能 |
| ClawHub | App Store | 技能分发平台 |
| 工具 | API 接口 | 技能暴露给 AI 的调用能力 |

### 技能能做什么

- ✅ 访问外部 API（天气、股票、新闻等）
- ✅ 操作数据库和文件系统
- ✅ 控制硬件设备（摄像头、智能家居等）
- ✅ 集成第三方服务（飞书、Slack、GitHub 等）
- ✅ 执行自定义代码逻辑

### 什么时候需要开发技能

**需要开发技能：**
- 现有技能无法满足需求
- 需要访问私有 API 或内部系统
- 需要特定的业务逻辑
- 想分享给社区使用

**不需要开发技能：**
- 已有现成技能可用
- 只是简单的配置调整
- 临时性、一次性的需求

---

## 技能结构解析

### 最小技能结构

一个最简单的技能只需要一个文件：

```
my-skill/
└── SKILL.md
```

### 完整技能结构

```
my-skill/
├── SKILL.md              # 必需：技能定义文件
├── README.md             # 推荐：使用说明
├── requirements.txt      # 可选：Python 依赖
├── package.json          # 可选：Node.js 依赖
├── tools/                # 可选：工具实现
│   ├── __init__.py
│   └── my_tool.py
├── scripts/              # 可选：辅助脚本
│   └── setup.sh
├── assets/               # 可选：资源文件
│   └── logo.png
└── tests/                # 可选：测试文件
    └── test_skill.py
```

### SKILL.md 详解

`SKILL.md` 是技能的核心定义文件，包含：

1. **技能元信息**（YAML Front Matter）
2. **工具定义**（Tools）
3. **使用说明**（Instructions）

#### 示例：天气查询技能

```markdown
---
name: weather
version: 1.0.0
description: 查询全球任意地点的天气预报
author: Your Name
repository: https://github.com/yourname/weather-skill
license: MIT
---

# Weather Skill

查询天气信息的 OpenClaw 技能，基于 OpenWeatherMap API。

## 工具列表

### get_current_weather

获取指定地点的当前天气。

**参数：**
- `location` (string, 必需): 地点名称或城市代码
- `units` (string, 可选): 温度单位，`metric`(摄氏度) 或 `imperial`(华氏度)，默认 `metric`

**返回：**
```json
{
  "location": "Beijing",
  "temperature": 25,
  "condition": "Sunny",
  "humidity": 60,
  "wind_speed": 10
}
```

**实现：**
```python
import requests

def get_current_weather(location: str, units: str = "metric"):
    api_key = os.getenv("OPENWEATHER_API_KEY")
    url = "https://api.openweathermap.org/data/2.5/weather"
    params = {"q": location, "appid": api_key, "units": units}
    
    response = requests.get(url, params=params)
    data = response.json()
    
    return {
        "location": data["name"],
        "temperature": data["main"]["temp"],
        "condition": data["weather"][0]["description"],
        "humidity": data["main"]["humidity"],
        "wind_speed": data["wind"]["speed"]
    }
```

## 配置说明

使用前需要配置环境变量：

```bash
openclaw config set skills.weather.env.OPENWEATHER_API_KEY "你的 API 密钥"
```

## 使用示例

```
"北京今天天气怎么样？"
"上海和广州哪个更热？"
"纽约现在多少度？"
```

## 依赖安装

```bash
pip install requests
```
```

---

## 创建第一个技能

### 实战：创建「今日名言」技能

这个技能每天提供一句名人名言，支持按主题筛选。

#### 步骤 1：创建项目结构

```bash
# 创建项目目录
mkdir quote-skill
cd quote-skill

# 创建文件结构
mkdir -p tools
touch SKILL.md README.md requirements.txt
```

#### 步骤 2：编写 SKILL.md

```markdown
---
name: daily-quote
version: 1.0.0
description: 获取名人名言，支持按主题筛选
author: Your Name
license: MIT
---

# Daily Quote Skill

提供名人名言的技能，支持随机获取和按主题筛选。

## 工具

### get_random_quote

获取一条随机名言。

**参数：** 无

**返回：**
```json
{
  "quote": "The only way to do great work is to love what you do.",
  "author": "Steve Jobs",
  "category": "motivation"
}
```

### get_quote_by_category

按主题获取名言。

**参数：**
- `category` (string, 必需): 名言主题，如 `motivation`, `wisdom`, `success`

**返回：** 同上

### list_categories

列出所有可用的名言主题。

**参数：** 无

**返回：**
```json
["motivation", "wisdom", "success", "life", "love", "friendship"]
```

## 实现代码

```python
import random
import os

# 名言数据库（实际应用中可以放在外部文件或 API）
QUOTES_DB = {
    "motivation": [
        {"quote": "The only way to do great work is to love what you do.", "author": "Steve Jobs"},
        {"quote": "Believe you can and you're halfway there.", "author": "Theodore Roosevelt"},
    ],
    "wisdom": [
        {"quote": "The only true wisdom is in knowing you know nothing.", "author": "Socrates"},
        {"quote": "Wisdom is not a product of schooling but of the lifelong attempt to acquire it.", "author": "Albert Einstein"},
    ],
    # ... 更多主题
}

def get_random_quote():
    category = random.choice(list(QUOTES_DB.keys()))
    quote = random.choice(QUOTES_DB[category])
    return {
        "quote": quote["quote"],
        "author": quote["author"],
        "category": category
    }

def get_quote_by_category(category: str):
    if category not in QUOTES_DB:
        return {"error": f"Unknown category: {category}"}
    quote = random.choice(QUOTES_DB[category])
    return {
        "quote": quote["quote"],
        "author": quote["author"],
        "category": category
    }

def list_categories():
    return list(QUOTES_DB.keys())
```

## 使用示例

```
"给我来句名言"
"今天有什么励志的话？"
"关于智慧的名言有哪些？"
"列出所有名言主题"
```
```

#### 步骤 3：创建依赖文件

`requirements.txt:`
```
# 本技能无外部依赖，使用 Python 标准库
```

#### 步骤 4：创建 README.md

```markdown
# Daily Quote Skill

OpenClaw 技能，提供名人名言。

## 安装

```bash
git clone https://github.com/yourname/quote-skill.git
cd quote-skill
openclaw skills install .
```

## 使用

```
"来句名言"
"给我励志的话"
"有哪些主题？"
```

## License

MIT
```

---

## 技能测试与调试

### 本地测试

#### 方法 1：直接运行工具函数

```bash
# 进入技能目录
cd quote-skill

# 创建测试脚本
cat > test.py << 'EOF'
from tools.quote_tool import get_random_quote, get_quote_by_category

# 测试随机名言
print("随机名言：", get_random_quote())

# 测试按主题获取
print("励志名言：", get_quote_by_category("motivation"))

# 测试主题列表
print("所有主题：", list_categories())
EOF

# 运行测试
python test.py
```

#### 方法 2：使用 OpenClaw 测试

```bash
# 安装技能
openclaw skills install .

# 检查技能状态
openclaw skills info daily-quote

# 在对话中测试
openclaw message send --target "test" --message "来句名言"
```

### 常见问题排查

#### 问题 1：技能未识别

**症状：** AI 不知道有这个技能

**排查：**
```bash
# 检查技能是否安装
openclaw skills list

# 检查技能是否合格
openclaw skills check

# 查看技能详情
openclaw skills info daily-quote
```

#### 问题 2：工具调用失败

**症状：** AI 尝试调用但报错

**排查：**
1. 检查 SKILL.md 格式是否正确
2. 检查工具函数参数定义是否匹配
3. 查看 Gateway 日志：`openclaw logs --follow`
4. 确认依赖已安装：`pip list`

#### 问题 3：环境变量未配置

**症状：** 访问 API 时认证失败

**解决：**
```bash
openclaw config set skills.<skill-name>.env.<VAR_NAME> "value"
openclaw gateway restart
```

---

## 发布到 ClawHub

### 准备工作

1. **完善文档**
   - README.md 包含完整使用说明
   - SKILL.md 有清晰的工具定义
   - 添加使用示例

2. **代码质量检查**
   - 移除调试代码和测试文件
   - 确保无硬编码的密钥
   - 添加 LICENSE 文件

3. **版本管理**
   - 使用 Git 管理代码
   - 打标签：`git tag v1.0.0`
   - 推送到 GitHub

### 发布步骤

```bash
# 1. 创建 GitHub 仓库
git init
git add .
git commit -m "Initial release"
git tag v1.0.0
git remote add origin https://github.com/yourname/quote-skill.git
git push origin main --tags

# 2. 提交到 ClawHub
# 访问 https://clawhub.ai 提交你的技能
# 或通过 CLI（如果支持）
```

### 提交清单

- [ ] SKILL.md 格式正确
- [ ] README.md 完整
- [ ] 代码无硬编码密钥
- [ ] 有使用示例
- [ ] 有 License
- [ ] GitHub 仓库公开
- [ ] 版本标签正确

---

## 最佳实践

### 1. 技能设计原则

**单一职责：**
- ✅ 一个技能专注一个领域
- ❌ 一个技能什么都做

**清晰命名：**
- ✅ `get_weather`, `send_email`
- ❌ `do_stuff`, `handle_request`

**明确参数：**
- ✅ `location: str`, `temperature: float`
- ❌ `data: any`, `params: dict`

### 2. 错误处理

```python
# 好的错误处理
def get_weather(location: str):
    try:
        response = requests.get(url, timeout=5)
        response.raise_for_status()
        return parse_weather(response.json())
    except requests.Timeout:
        return {"error": "请求超时，请稍后重试"}
    except requests.HTTPError as e:
        if e.response.status_code == 404:
            return {"error": f"未找到地点：{location}"}
        return {"error": f"API 错误：{e.response.status_code}"}
    except Exception as e:
        return {"error": f"未知错误：{str(e)}"}
```

### 3. 文档规范

**好的文档：**
- 清晰的工具描述
- 完整的参数说明
- 实际的返回示例
- 多种使用场景

**差的文档：**
- 只有一句话说明
- 没有参数说明
- 没有示例
- 过时的信息

### 4. 安全注意

**绝对不要：**
- ❌ 硬编码 API 密钥
- ❌ 在代码中存储用户隐私
- ❌ 执行未经验证的输入
- ❌ 访问未授权的资源

**应该做：**
- ✅ 使用环境变量存储密钥
- ✅ 验证和清理所有输入
- ✅ 限制操作权限范围
- ✅ 记录敏感操作日志

### 5. 性能优化

**缓存策略：**
```python
from functools import lru_cache

@lru_cache(maxsize=100)
def get_weather(location: str):
    # 相同地点 5 分钟内不重复请求
    return fetch_weather(location)
```

**批量处理：**
```python
# 一次请求获取多个数据
def get_weather_batch(locations: list[str]):
    # 而不是循环调用单个接口
    return batch_fetch(locations)
```

---

## 📚 进阶资源

### 官方文档
- [Skills 系统](https://docs.openclaw.ai/tools/skills)
- [Skills 配置](https://docs.openclaw.ai/tools/skills-config)
- [ClawHub](https://docs.openclaw.ai/tools/clawhub)

### 示例技能
- [ezBookkeeping](https://clawhub.ai/mayswind/ezbookkeeping) - 记账技能
- [Gmail](https://clawhub.ai/byungkyu/gmail) - 邮件管理
- [Web Search](https://clawhub.ai/openclaw/websearch) - 网络搜索

### 社区资源
- [Awesome OpenClaw Skills](https://github.com/openclaw/awesome-openclaw-skills)
- [OpenClaw-CN 技能集合](./skills/README.md)

---

*遇到问题？查看 [FAQ](./faq.md) 或提交 Issue*
*最后更新：2026-03-02*
