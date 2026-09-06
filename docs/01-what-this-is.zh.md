# 01 — 这是什么 / 不是什么

## 这是什么

一套**文档 + 脱敏配置示例**，教你把：

**手机 Telegram → 官方 Hermes Gateway → Codex App-Server → 已登录 ChatGPT/Codex**

串成一条可用的「手机遥控写代码/改文件」链路。

生产链路必须是官方 Hermes 的 `openai_runtime = codex_app_server`，让 Codex 以完整 Agent Runtime 跑，而不是：

- ❌ 只用 Hermes 调某个聊天模型 API
- ❌ 自己写脚本包装 `codex` CLI 当生产 Runtime
- ❌ 把 Desktop 里的宿主按钮当成 Hermes 也有的能力

## 你最终会得到

| 能力 | 说明 |
|------|------|
| 手机发自然语言任务 | 白名单用户才能用 |
| 云端或本机执行 | 文件、Shell、已配置的 MCP |
| 独立会话 | Topics / `/new` / `/branch`（按官方文档） |
| 可选常驻 | Linux `hermes gateway install`；Windows 另有计划任务方案 |

## 明确不在本包范围

- 交易 / 刷单 / 绕过平台策略
- 自动把本机第二大脑全库灌进云端
- 云端 Bot ↔ 本地 Bot 自动互聊（**不会**，要各自通道）
- 保证「无 systemd 的云电脑 24h 不死」（文件持久 ≠ 进程常驻）

## 架构一图

```text
┌─────────────┐     allowlist      ┌──────────────────┐
│  Telegram   │ ─────────────────► │ Hermes Gateway   │
│  手机客户端  │ ◄───────────────── │ polling / webhook│
└─────────────┘     回复            └────────┬─────────┘
                                             │
                                             ▼
                                   ┌──────────────────┐
                                   │ Codex App-Server │
                                   │ (ChatGPT auth)   │
                                   └────────┬─────────┘
                                             │
                                             ▼
                                   工作目录 / Shell / MCP
```

## 下一步

去 [02 BotFather](02-botfather-setup.zh.md) 建 Bot。
