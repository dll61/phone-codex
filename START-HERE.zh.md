# 从这里开始（小白总入口）

你要做的事只有一件：**让手机 Telegram 能指挥一台已经登录 Codex 的电脑干活。**

## 先搞清楚三样东西

1. **Telegram Bot**：手机入口（用 BotFather 创建）
2. **Hermes Gateway**：收消息、调 Codex 的官方网关
3. **Codex（ChatGPT 订阅登录）**：真正写文件、跑命令的大脑

链路：

```text
手机 → Bot → Hermes Gateway → Codex App-Server → 电脑干活 → 回 Telegram
```

## 你需要准备什么

- 一个 Telegram 账号
- 一台 Linux（云电脑也行）或 Windows 本机
- 已能登录的 Codex / ChatGPT（OAuth），**不要把 auth.json 贴到聊天里**
- 大约 30–60 分钟（第一次）

## 推荐阅读顺序（别跳）

1. [01 这是什么](docs/01-what-this-is.zh.md) — 2 分钟
2. [02 BotFather](docs/02-botfather-setup.zh.md) — 按截图级步骤做
3. 选一条路：
   - 云 / Linux → [03](docs/03-install-linux-cloud.zh.md)
   - Windows 本机 → [04](docs/04-install-windows-local.zh.md)
4. [05 配置](docs/05-hermes-codex-config.zh.md) — **必看关键配置**
5. [06 踩坑](docs/06-pitfalls-and-fixes.zh.md) — 出问题先查这里
6. [07 验收](docs/07-acceptance-checklist.zh.md) — 过了才算成功
7. [08 安全](docs/08-security.zh.md) — 上线前再扫一眼

## 三条铁律（背下来）

1. **不要**在 Codex 配置里写 `default_permissions = ":danger-no-sandbox"`（未知 profile，会炸 App-Server）。只用 `sandbox_mode = "danger-full-access"` + `approval_policy = "never"`。
2. **云端 Bot 和本地 Bot 必须分开 Token**。同一个 Token 两边一起 polling = 必炸。
3. **Token / auth.json / 你的 Telegram 用户 ID 永远不要提交到 Git。**

## 成功长什么样

手机给 Bot 发：

> 请在工作目录创建 hello.txt，内容必须是 hello from codex

然后：

- 只收到 **一条** 回复（不是两条一模一样）
- 工作目录里真有 `hello.txt`，内容精确是 `hello from codex`
- 不再刷「No home channel」提示（或你已 `/sethome`）

过了上面三条，你就配通了。其余（Topics、Kanban、24h 常驻）是进阶项，见验收清单。
