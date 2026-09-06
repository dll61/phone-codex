# phone-codex

> **一句话：用手机 Telegram 遥控 Codex，官方 Hermes 当网关，石头猪也能配通。**
>
> One-liner: Control OpenAI Codex from your phone via Telegram + official Hermes Agent gateway.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

手机发一句话 → Hermes Gateway → Codex App-Server → 云端/本机改文件跑命令 → Telegram 回你。

不是「Hermes 只调个模型 API」，也不是「自己包一层 Codex CLI」。是官方完整 Agent Runtime。

## 你能得到什么

- 📱 手机 Telegram 当遥控器（白名单，不给陌生人）
- 🧠 官方 [Hermes Agent](https://github.com/NousResearch/hermes-agent) + [Codex App-Server Runtime](https://hermes-agent.nousresearch.com/docs/user-guide/features/codex-app-server-runtime/)
- ☁️ Linux 云电脑 / 🪟 Windows 本机 两套路线（**各用各的 Bot，别抢同一个 Token**）
- 🧯 真实踩坑修复：重复回复、未知权限 profile、home channel、polling 冲突……

## 5 分钟看懂

```text
  手机 Telegram
       │  (白名单用户)
       ▼
  Hermes Gateway  ←── hermes gateway run --accept-hooks
       │
       ▼
  openai-codex + codex_app_server
       │
       ▼
  已登录的 ChatGPT / Codex 订阅
       │
       ▼
  文件 / Shell / MCP …… 结果回 Telegram
```

## 3 步上手（超短版）

1. **BotFather 建 Bot** → Allow Groups **ON**，Group Privacy **OFF**，Guest Chat **OFF**，Bot Management **OFF**（详见 [docs/02](docs/02-botfather-setup.zh.md)）
2. **装 Hermes + 配 Codex** → `provider=openai-codex`，`openai_runtime=codex_app_server`，Codex 只设 `sandbox_mode=danger-full-access` + `approval_policy=never`（**不要**写 `:danger-no-sandbox`）
3. **写 `.env` 白名单 + 启动** → `hermes gateway run --accept-hooks`，手机发「创建 hello.txt」验收

👉 **小白请从这里读：** [START-HERE.zh.md](START-HERE.zh.md)

## 文档目录

| 文件 | 内容 |
|------|------|
| [START-HERE.zh.md](START-HERE.zh.md) | 小白总入口 |
| [docs/01-what-this-is.zh.md](docs/01-what-this-is.zh.md) | 这是什么 / 不是什么 |
| [docs/02-botfather-setup.zh.md](docs/02-botfather-setup.zh.md) | BotFather 逐步设置 |
| [docs/03-install-linux-cloud.zh.md](docs/03-install-linux-cloud.zh.md) | Linux / 云电脑安装 |
| [docs/04-install-windows-local.zh.md](docs/04-install-windows-local.zh.md) | Windows 本机要点 |
| [docs/05-hermes-codex-config.zh.md](docs/05-hermes-codex-config.zh.md) | Hermes + Codex 关键配置 |
| [docs/06-pitfalls-and-fixes.zh.md](docs/06-pitfalls-and-fixes.zh.md) | 全部踩坑：症状→原因→一行修复 |
| [docs/07-acceptance-checklist.zh.md](docs/07-acceptance-checklist.zh.md) | 验收清单 |
| [docs/08-security.zh.md](docs/08-security.zh.md) | 安全：token、白名单、权限 |
| [config/examples/](config/examples/) | 脱敏示例配置 |
| [AGENTS.md](AGENTS.md) | 给其他 Agent：按顺序执行，禁止提交密钥 |

## 官方文档

- Telegram：https://hermes-agent.nousresearch.com/docs/user-guide/messaging/telegram/
- Codex App-Server Runtime：https://hermes-agent.nousresearch.com/docs/user-guide/features/codex-app-server-runtime/
- 源码：https://github.com/NousResearch/hermes-agent

## 省额度小贴士

重活（装依赖、写配置、跑验收）交给已登录的 Codex；外层调度 Agent 只做派发和验收，别每条命令都开贵模型。

## License

MIT — 见 [LICENSE](LICENSE)。本仓库只含文档与脱敏示例，不含任何 Bot Token / auth.json。
