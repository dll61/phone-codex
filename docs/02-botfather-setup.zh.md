# 02 — BotFather 逐步设置（照着点）

目标：拿到 **Bot Token**，并打开群组/话题需要的开关。

## 1. 创建 Bot

1. 手机打开 Telegram，搜 `@BotFather`
2. 发 `/newbot`
3. 起一个显示名，例如 `My Cloud Codex`
4. 起一个用户名，必须以 `bot` 结尾，例如 `my_cloud_codex_bot`  
   （仓库示例里写成 `@YOUR_CLOUD_BOT`，别照抄别人的名字）
5. BotFather 会给你一串 **Token**（形如 `123456:ABC...`）

**立刻：** 把 Token 存到密码管理器或本机安全输入，**不要**发到群、不要贴 Git、不要截图发朋友圈。

## 2. 必开 / 必关（群组场景）

在 BotFather 里对这个 Bot：

| 设置 | 建议 | 为什么 |
|------|------|--------|
| **Allow Groups** | **ON** | 允许进群 |
| **Group Privacy** | **OFF** | Privacy ON 时 Bot 几乎听不见群里普通消息 |
| **Guest Chat** | **OFF** | 安全：别给陌生人乱入入口 |
| **Bot Management** | **OFF** | 安全：别随便开放管理类能力 |
| **Inline Mode** | 可选 OFF | 不用就关，少一个面 |
| **Threads / Topics** | 若要用话题模式再开 | DM Topics 需 BotFather Threads；见官方 Telegram 文档 |

云端实测可用组合：**Allow Groups ON + Group Privacy OFF**；Guest Chat / Bot Management 保持 OFF。

## 3. 拿到你自己的 Telegram User ID（白名单用）

白名单需要 **数字 User ID**，不是用户名。

常见办法：

- 用官方/社区的 `userinfobot` 类机器人看自己的 id  
- 或先临时启动 gateway 看日志里的 from id（再写入 allowlist）

文档和示例里一律写成：

```text
YOUR_TELEGRAM_USER_ID
```

**不要**把真实 ID 当「示范密钥」提交到公开仓库。

## 4. 双 Bot 原则（超重要）

| 场景 | 做法 |
|------|------|
| 本机已经有一个 Hermes Bot | **再建一个「云端」Bot**，名字里带 cloud / yun 都行 |
| 手机切换 | 打开不同 Bot 聊天即可 |
| 想复用同一个 Token | 必须先停掉旧 Gateway，确认没有第二个 polling；**禁止双端同时 getUpdates** |

## 5. 下一步

- Linux / 云电脑 → [03](03-install-linux-cloud.zh.md)
- Windows → [04](04-install-windows-local.zh.md)
