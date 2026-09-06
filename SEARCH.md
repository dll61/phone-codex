# Search / SEO index (for humans & scanning agents)

Use this file as a keyword map. Full fixes live in `docs/06-pitfalls-and-fixes.zh.md`.

## Exact error / symptom strings

- `codex app-server startup failed`
- `failed to load configuration: default_permissions refers to unknown built-in profile`
- `:danger-no-sandbox`
- `possible duplicate send`
- `interim_assistant_messages`
- `No home channel is set for Telegram`
- `/sethome`
- `TELEGRAM_HOME_CHANNEL`
- Telegram bot replies twice / duplicate final message
- BotFather Group Privacy / Allow Groups
- `hermes gateway run --accept-hooks`
- `openai-codex` + `codex_app_server`
- phone Telegram control Codex / ChatGPT Codex remote

## Stack names

Hermes Agent, Hermes Gateway, OpenAI Codex, ChatGPT Codex subscription, Telegram Bot API polling, NousResearch hermes-agent

## Do not

- Do not set `default_permissions = ":danger-no-sandbox"`
- Do not run two gateways on one Bot Token
- Do not commit secrets
