# OpenClaw 快速入门指南

> 目标：用最少的设置从零开始实现第一次聊天

<Info>
  最快的聊天方式：打开 Control UI（不需要配置频道）。运行 `openclaw dashboard` 
  并在浏览器中聊天，或者在 gateway 主机上打开 `http://127.0.0.1:18789/`。
  文档：[Dashboard](/web/dashboard) 和 [Control UI](/web/control-ui)。
</Info>

## 前置要求

* Node 22 或更新版本

<Tip>
  如果不确定，可以用 `node --version` 检查 Node 版本。
</Tip>

## 快速设置（CLI 方式）

### 步骤 1：安装 OpenClaw（推荐）

**macOS/Linux:**
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**Windows (PowerShell):**
```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

<Note>
  其他安装方法和要求见：[安装文档](/install)。
</Note>

### 步骤 2：运行 onboarding 向导

```bash
openclaw onboard --install-daemon
```

向导会配置认证、gateway 设置和可选的频道。
详见 [Onboarding 向导](/start/wizard)。

### 步骤 3：检查 Gateway

如果安装了服务，它应该已经在运行：

```bash
openclaw gateway status
```

### 步骤 4：打开 Control UI

```bash
openclaw dashboard
```

<Check>
  如果 Control UI 加载成功，你的 Gateway 就可以使用了。
</Check>

## 可选检查和额外功能

### 在前台运行 Gateway

用于快速测试或故障排除：

```bash
openclaw gateway --port 18789
```

### 发送测试消息

需要配置好的频道：

```bash
openclaw message send --target +15555550123 --message "Hello from OpenClaw"
```

## 有用的环境变量

如果你以服务账户运行 OpenClaw 或想要自定义配置/状态位置：

* `OPENCLAW_HOME` - 设置用于内部路径解析的主目录
* `OPENCLAW_STATE_DIR` - 覆盖状态目录
* `OPENCLAW_CONFIG_PATH` - 覆盖配置文件路径

完整的环境变量参考：[环境变量](/help/environment)

## 深入了解

* [Onboarding 向导（详情）](/start/wizard) - 完整 CLI 向导参考和高级选项
* [macOS 应用 onboarding](/start/onboarding) - macOS 应用的首次运行流程

## 你将获得什么

* 一个运行中的 Gateway
* 配置好的认证
* Control UI 访问或已连接的频道

## 下一步

* 私信安全和审批：[配对](/channels/pairing)
* 连接更多频道：[频道](/channels)
* 高级工作流和源码安装：[设置](/start/setup)

---

*翻译自：https://docs.openclaw.ai/start/getting-started*
*最后更新：2026-03-02*
