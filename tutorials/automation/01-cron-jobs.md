# OpenClaw Cron Jobs 定时任务指南

> **Cron vs Heartbeat？** 详见 [Cron 与 Heartbeat 对比](/automation/cron-vs-heartbeat) 了解何时使用哪种方式。

Cron 是 Gateway 的内置调度器。它持久化存储任务，在正确时间唤醒 Agent，并可选择将输出发送回聊天。

如果你想实现"每天早上运行这个"或"20 分钟后提醒 Agent"，Cron 就是所需的机制。

故障排除：[/automation/troubleshooting](/automation/troubleshooting)

## 快速上手

### 创建一次性提醒

```bash
# 创建提醒任务
openclaw cron add \
  --name "提醒" \
  --at "2026-02-01T16:00:00Z" \
  --session main \
  --system-event "提醒：检查 cron 文档草稿" \
  --wake now \
  --delete-after-run

# 查看任务列表
openclaw cron list

# 立即运行任务
openclaw cron run <job-id>

# 查看运行历史
openclaw cron runs --id <job-id>
```

### 创建周期性任务

```bash
# 每天早上的简报
openclaw cron add \
  --name "每日简报" \
  --cron "0 7 * * *" \
  --tz "America/Los_Angeles" \
  --session isolated \
  --message "总结夜间更新" \
  --announce \
  --channel slack \
  --to "channel:C1234567890"
```

## Cron 任务存储位置

Cron 任务持久化存储在 Gateway 主机的 `~/.openclaw/cron/jobs.json`。
Gateway 将文件加载到内存并在更改时写回，因此手动编辑仅在 Gateway 停止时安全。
建议使用 `openclaw cron add/edit` 或 cron 工具调用 API 进行更改。

## Cron 调度类型

Cron 支持三种调度类型：

| 类型 | 说明 | 示例 |
|------|------|------|
| `at` | 一次性时间戳 | `--at "2026-02-01T16:00:00Z"` |
| `every` | 固定间隔（毫秒） | 每 30 分钟 |
| `cron` | 5 字段 cron 表达式 | `"0 7 * * *"` (每天 7 点) |

### Cron 表达式示例

```
# 每分钟
* * * * *

# 每小时
0 * * * *

# 每天上午 9 点
0 9 * * *

# 每周一上午 9 点
0 9 * * 1

# 每 15 分钟
*/15 * * * *
```

## 执行模式

### 主会话任务（Main Session）

主会话任务将系统事件加入队列，并可选择唤醒心跳运行器。

* 使用 `payload.kind = "systemEvent"`
* `wakeMode: "now"` (默认)：事件立即触发心跳运行
* `wakeMode: "next-heartbeat"`：事件等待下一次计划心跳

**适用场景：** 需要正常心跳提示和主会话上下文的任务

### 隔离任务（Isolated Session）

隔离任务在 `cron:<jobId>` 专用会话中运行独立的 Agent 轮次。

**关键特性：**
* 提示符前缀为 `[cron:<jobId> <任务名称>]` 便于追踪
* 每次运行都启动全新的会话 ID（不携带之前的对话历史）
* 默认行为：如果省略 `delivery`，隔离任务会宣布摘要 (`delivery.mode = "announce"`)

**交付模式 (`delivery.mode`):**
* `announce` - 向目标频道发送摘要，并向主会话发布简要摘要
* `webhook` - 完成时向指定 URL 发送 POST 请求
* `none` - 仅内部执行（无交付，无主会话摘要）

**适用场景：** 嘈杂、频繁或"后台事务"类任务，不应刷屏主聊天历史

## 实用示例

### 示例 1：每日新闻摘要

```bash
openclaw cron add \
  --name "每日新闻" \
  --cron "0 8 * * *" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --message "搜索并总结今天的科技新闻" \
  --announce
```

### 示例 2：每小时检查邮件

```bash
openclaw cron add \
  --name "邮件检查" \
  --cron "0 * * * *" \
  --session main \
  --system-event "检查新邮件" \
  --wake next-heartbeat
```

### 示例 3：周报告

```bash
openclaw cron add \
  --name "周报" \
  --cron "0 9 * * 1" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --message "生成本周工作总结报告" \
  --announce \
  --channel telegram
```

## 高级配置

### 时区设置

如果 ISO 时间戳省略时区，则视为 **UTC**。

使用 `--tz` 参数设置时区：
```bash
--tz "Asia/Shanghai"  # 中国标准时间
--tz "America/New_York"  # 美国东部时间
```

### 错峰执行

为减少整点负载峰值，OpenClaw 对周期性整点表达式应用确定性错峰窗口（最多 5 分钟）。

使用 `--stagger` 设置错峰窗口：
```bash
--stagger 30s  # 30 秒错峰
--stagger 1m   # 1 分钟错峰
--exact        # 强制精确时间（无错峰）
```

## 管理命令

| 命令 | 说明 |
|------|------|
| `openclaw cron list` | 列出所有任务 |
| `openclaw cron run <id>` | 手动运行任务 |
| `openclaw cron runs --id <id>` | 查看任务运行历史 |
| `openclaw cron edit <id>` | 编辑任务 |
| `openclaw cron delete <id>` | 删除任务 |

---

*翻译自：https://docs.openclaw.ai/automation/cron-jobs.md*
*最后更新：2026-03-02*
