# OpenClaw 部署指南汇总

> 三种部署方式，选择最适合你的方案

---

## 📋 部署方式对比

| 部署方式 | 优势 | 劣势 | 适用场景 | 难度 | 成本 |
|---------|------|------|---------|------|------|
| [**阿里云部署**](#阿里云部署) | 7×24 小时稳定、公网访问、多用户共享 | 需要服务器费用 | 团队协作、长期自动化 | ⭐⭐ | 68 元/年起 |
| [**本地部署**](#本地部署) | 数据私有、零服务器成本、低延迟 | 需要设备常开 | 个人使用、敏感数据 | ⭐⭐ | 免费 |
| [**MaxClaw**](#maxclaw-零门槛部署) | 零配置、10 秒上手、内置工具 | 功能受限、依赖第三方 | 小白用户、快速体验 | ⭐ | 免费/付费 |

---

## 阿里云部署

### 适合人群

- ✅ 需要 7×24 小时运行
- ✅ 团队协作场景
- ✅ 需要公网访问
- ✅ 长期自动化任务

### 前置准备

- 阿里云账号（需实名认证）
- 预算：68 元/年起（新人专享）

### 步骤 1：购买服务器

1. 访问 [阿里云 OpenClaw 一键部署页面](https://www.aliyun.com/activity/ecs/clawdbot)
2. 点击【一键购买并部署】
3. 配置参数（默认已适配）：
   - **地域：** 美国弗吉尼亚/中国香港/新加坡（海外免备案）
   - **镜像：** OpenClaw 开源镜像
   - **实例规格：** 2vCPU + 4GiB 内存（推荐）
   - **购买时长：** 12 个月（年付更划算）

4. 支付订单，等待 3 分钟实例创建完成

### 步骤 2：配置环境

#### 远程登录服务器

在阿里云控制台点击【远程连接】登录服务器

#### 克隆 OpenClaw 仓库

```bash
git clone https://github.com/openclaw/openclaw
cd openclaw
```

#### 配置环境变量

```bash
# 复制示例配置文件
cp .env.example .env

# 编辑环境变量
vim .env
```

在 `.env` 文件中添加：

```bash
# LLM API 密钥（支持 MiniMax、通义千问等）
API_KEY=你的 API 密钥

# 通讯工具 Token（以飞书为例）
FEISHU_APP_ID=你的飞书 AppID
FEISHU_APP_SECRET=你的飞书 AppSecret

# 数据库配置（默认 SQLite，高并发可改用 PostgreSQL）
DATABASE_URL=sqlite://./data.db

# 端口配置（默认 3000）
PORT=3000
```

#### 端口放通

1. 登录阿里云轻量应用服务器控制台
2. 找到目标实例，点击【防火墙】
3. 点击【添加规则】，放行 3000 端口（或自定义端口），协议选择 TCP

### 步骤 3：启动服务

```bash
# 安装依赖
npm install

# 后台启动服务
nohup npm start > openclaw.log 2>&1 &

# 验证服务可用性
curl localhost:3000/health
```

若输出 `{"status":"healthy","version":"x.x.x"}`，说明部署成功。

### 步骤 4：Docker 部署（推荐生产环境）

```bash
# 构建 Docker 镜像
docker build -t openclaw .

# 后台运行容器（挂载数据目录确保持久化）
docker run -d -p 3000:3000 \
  --env-file .env \
  -v $(pwd)/data:/openclaw/data \
  --name openclaw \
  openclaw

# 查看容器日志
docker logs -f openclaw
```

### 步骤 5：配置 OpenClaw

1. 在轻量应用服务器控制台，进入服务器详情页
2. 在 OpenClaw 使用步骤区域：
   - 点击【端口放通】下的执行命令，开放防火墙
   - 点击【配置 OpenClaw】下的执行命令，配置百炼 API Key
   - 点击【访问控制】下的执行命令，获取 WebUI 访问地址

### 费用说明

**阿里云轻量应用服务器费用：**
- 2vCPU + 2GiB + 40GB：约 68 元/年（新人专享）
- 2vCPU + 4GiB + 60GB：约 120 元/年

**模型调用费用：**
- **Coding Plan 套餐（推荐）：** 固定月费，首月 7.9 元起
  - 支持模型：qwen3.5-plus、kimi-k2.5、MiniMax-M2.5、glm-5
  - 优点：成本可控，超出限额不计费
- **按 Token 计费：** 用多少付多少
  - qwen3.5-plus：约 0.004 元/1K tokens（输入）

---

## 本地部署

### 适合人群

- ✅ 个人日常使用
- ✅ 注重数据隐私
- ✅ 有常开设备
- ✅ 想零成本体验

### 前置准备

**系统要求：**
- Windows 10+ / macOS 12+ / Linux (Ubuntu 20.04+)

**软件依赖：**
- Node.js 18.0.0+
- npm 8.0.0+
- Git

### 步骤 1：安装依赖

#### 检查和安装 Node.js

```bash
# 检查 Node.js 版本
node --version

# 如果版本低于 18，执行升级
# macOS/Linux:
nvm install 18 && nvm use 18

# Windows (PowerShell):
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
nvm install 18 && nvm use 18
```

#### 安装 Git

- Windows：https://git-scm.com/download/win
- macOS：`xcode-select --install`
- Linux：`sudo apt install git` (Ubuntu/Debian)

### 步骤 2：克隆项目

```bash
# 克隆仓库
git clone https://github.com/openclaw/openclaw
cd openclaw

# 安装依赖
npm install
```

### 步骤 3：配置环境变量

```bash
# 复制示例配置文件
cp .env.example .env

# 编辑配置
# Windows 用户可以用记事本打开 .env 文件
# macOS/Linux 用户：
vim .env
```

填入以下信息：

```bash
# API 密钥（支持多种模型）
API_KEY=你的 API 密钥

# 通讯工具 Token（根据需要配置）
FEISHU_APP_ID=你的飞书 AppID
FEISHU_APP_SECRET=你的飞书 AppSecret

# 数据库配置
DATABASE_URL=sqlite://./data.db

# 端口配置
PORT=3000
```

### 步骤 4：启动服务

```bash
# 启动服务
npm start

# 在新终端验证
curl localhost:3000/health
```

服务启动后：
- 访问 http://localhost:3000 查看控制台
- 通过飞书、Telegram 等工具发送指令测试

### 步骤 5：后台运行（可选）

```bash
# macOS/Linux 使用 nohup
nohup npm start > openclaw.log 2>&1 &

# 或者使用 PM2（推荐）
npm install -g pm2
pm2 start npm --name "openclaw" -- start
pm2 save
pm2 startup
```

### 常见问题

**Q: 启动时报错 "EADDRINUSE"**
- A: 端口被占用，修改 `.env` 中的 `PORT` 为其他端口

**Q: 无法访问外网 API**
- A: 检查网络代理设置，或使用国内模型 API

**Q: 内存占用过高**
- A: 考虑增加 swap 空间，或降低并发设置

---

## MaxClaw 零门槛部署

### 适合人群

- ✅ 零基础小白用户
- ✅ 快速体验 OpenClaw
- ✅ 不想配置环境
- ✅ 临时使用场景

### 部署步骤

#### 步骤 1：访问 MaxClaw

访问 MiniMax 官网：https://agent.minimaxi.com/

#### 步骤 2：创建 MaxClaw

1. 点击 "MaxClaw"
2. 选择默认配置或专业模板（如"爆款猎手"）
3. 等待 10 秒，专属 AI 助手自动创建完成

#### 步骤 3：配置跨端协作

发送指令："连接飞书"

按提示输入：
- 飞书 AppID
- 飞书 AppSecret

完成跨端协作配置

### 优势

- ✅ 10 秒上手，无需任何技术背景
- ✅ 内置常用工具，开箱即用
- ✅ 自动更新，无需维护
- ✅ 支持基础自定义

### 限制

- ⚠️ 功能相对固定，自定义能力有限
- ⚠️ 依赖 MiniMax 平台
- ⚠️ 高级功能可能需付费

---

## 部署后配置

### 配置模型 API

```bash
# 使用 Coding Plan 套餐（推荐）
openclaw config set llm.apiKey "你的 Coding Plan API Key"
openclaw config set llm.model "qwen3.5-plus"

# 或使用按量计费
openclaw config set llm.apiKey "你的按量付费 API Key"
```

### 配置频道

```bash
# 飞书
openclaw channels setup feishu

# WhatsApp
openclaw channels login whatsapp

# Telegram
openclaw channels setup telegram
```

### 安装技能插件

```bash
# 从 ClawHub 安装
openclaw skills install https://clawhub.ai/username/skill-name

# 查看已安装技能
openclaw skills list
```

---

## 故障排除

### Gateway 启动失败

```bash
# 查看详细日志
openclaw logs --follow

# 运行诊断工具
openclaw doctor

# 检查端口占用
lsof -i :3000  # 或你配置的端口
```

### 模型调用失败

1. 检查 API Key 是否正确
2. 确认 API Key 对应地域与服务器匹配
3. 查看账户余额/额度是否充足

### 频道连接失败

1. 检查 Token/密钥是否过期
2. 确认网络能访问对应服务
3. 查看频道配置是否正确

---

## 相关资源

- [快速开始指南](./tutorials/getting-started/01-quickstart.md)
- [常见问题 FAQ](./docs/faq.md)
- [核心功能介绍](./tutorials/getting-started/02-features.md)
- [六大神级用法](./usecases/6-advanced-usecases.md)

---

*最后更新：2026-03-02*
*部署遇到问题？查看 [FAQ](./docs/faq.md) 或提交 Issue*
