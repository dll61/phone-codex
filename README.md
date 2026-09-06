# phone-codex

> **一句话：用手机 Telegram 遥控 OpenAI Codex（官方 Hermes 网关）。石头猪能看懂，Agent 能照做。**
>
> Control **OpenAI Codex** from **Telegram** via official **Hermes Agent** (`codex_app_server` / `openai-codex`).

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Docs](https://img.shields.io/badge/docs-START--HERE-blue)](START-HERE.zh.md)
[![For Agents](https://img.shields.io/badge/AGENTS.md-ready-green)](AGENTS.md)

**Keywords / 检索词（给人与扫描 Agent）：** `Telegram Hermes Codex` · `hermes-agent telegram` · `codex_app_server` · `openai-codex` · `danger-no-sandbox` · `interim_assistant_messages` · `possible duplicate send` · `No home channel` `/sethome` · `BotFather Group Privacy` · phone remote Codex · ChatGPT Codex Telegram bot

---

## 痛点 → 解法（裂变钩子）

| 你卡住的症状 | 本仓库给什么 |
|---|---|
| 想在**手机**上指挥 Codex 改文件/跑命令 | Telegram → Hermes Gateway → Codex App-Server 全链路文档 |
| `codex app-server startup failed` / unknown profile `:danger-no-sandbox` | **删掉该行**；只用 `sandbox_mode=danger-full-access` + `approval_policy=never` |
| Telegram **同一条回复两次** / `possible duplicate send` | `display.interim_assistant_messages=false` + 关 streaming |
| `No home channel is set for Telegram` | `/sethome` 或 `TELEGRAM_HOME_CHANNEL` |
| Bot 进群没反应 / 只认 slash | BotFather：**Allow Groups ON**，**Group Privacy OFF** |
| 本地和云端抢同一个 Bot Token | **两个 Bot、两套 Token**，禁止双端同时 polling |

👉 小白入口：[START-HERE.zh.md](START-HERE.zh.md) · Agent 入口：[AGENTS.md](AGENTS.md) · 踩坑速查：[docs/06-pitfalls-and-fixes.zh.md](docs/06-pitfalls-and-fixes.zh.md)

---

## 5 分钟看懂

```text
手机 Telegram（白名单）
        │
        ▼
Hermes Gateway   ←  hermes gateway run --accept-hooks
        │
        ▼
provider=openai-codex  +  openai_runtime=codex_app_server
        │
        ▼
已登录的 ChatGPT / Codex 订阅 → 文件/Shell/MCP → 回 Telegram
```

不是「Hermes 只调模型 API」，也不是「自己包一层 Codex CLI」。是官方完整 Agent Runtime。

---

## 3 步上手

1. **BotFather** 建 Bot → Allow Groups **ON**，Group Privacy **OFF**，Guest Chat **OFF**，Bot Management **OFF** → [docs/02](docs/02-botfather-setup.zh.md)
2. **Hermes + Codex** → `openai-codex` + `codex_app_server`；Codex **不要**写 `:danger-no-sandbox` → [docs/05](docs/05-hermes-codex-config.zh.md)
3. **`.env` 白名单 + 启动** → 手机验收 `hello.txt` → [docs/07](docs/07-acceptance-checklist.zh.md)

---

## 文档地图

| 文件 | 给谁 | 内容 |
|------|------|------|
| [START-HERE.zh.md](START-HERE.zh.md) | 人 | 小白总入口 |
| [AGENTS.md](AGENTS.md) | Agent | 执行顺序 + 硬性禁止（密钥不进 Git） |
| [docs/01](docs/01-what-this-is.zh.md) … [08](docs/08-security.zh.md) | 人/Agent | 概念→BotFather→安装→配置→踩坑→验收→安全 |
| [config/examples/](config/examples/) | 复制改 | 脱敏 `.env` / yaml / toml 片段 |

官方： [Telegram](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/telegram/) · [Codex App-Server Runtime](https://hermes-agent.nousresearch.com/docs/user-guide/features/codex-app-server-runtime/) · [hermes-agent](https://github.com/NousResearch/hermes-agent)

---

## 为什么值得转发（复制即用）

很多人：Codex 很强，但人要钉在电脑前。  
这套：地铁上发一句 Telegram，云端 Codex 干活，结果回手机。

**中文转发：**  
「手机 Telegram 遥控 Codex：官方 Hermes + 真实踩坑（`:danger-no-sandbox` 崩、重复回复、home channel、BotFather）。开箱：https://github.com/dll61/phone-codex」

**EN share：**  
「Control OpenAI Codex from Telegram via Hermes (`codex_app_server`). Fixes: unknown `:danger-no-sandbox`, duplicate replies, `/sethome`. https://github.com/dll61/phone-codex」

⭐ 有用就 Star；朋友要装只丢链接。喂给任意 Agent：先读 `AGENTS.md`。

---

## 安全（开源铁律）

本仓**只有文档与占位符示例**。不含 Bot Token、`auth.json`、Telegram UID、密码、API Key。真实密钥只进本机 `chmod 600` 的 `.env`，永不进 Git。

## License

MIT — [LICENSE](LICENSE)
