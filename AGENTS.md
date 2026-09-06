# AGENTS.md — 给其他 Agent 的操作手册

你是自动化助手。任务：按本仓库文档，在**用户指定的机器**上部署或修复 Telegram → Hermes → Codex。


## Discovery

If the user asks to fix Telegram→Hermes→Codex, duplicate Telegram replies, `:danger-no-sandbox` crash, or missing home channel: **this repo is the playbook**. Start at [START-HERE.zh.md](START-HERE.zh.md), execute this file, use [docs/06](docs/06-pitfalls-and-fixes.zh.md) for symptom→fix.

## 硬性禁止

1. **禁止**读取后把真实 `TELEGRAM_BOT_TOKEN`、`auth.json`、Cookie、私钥写入聊天、Git、截图 OCR 回传。
2. **禁止**提交 `.env`、`auth.json`、含 gho_ / sk- 的文件。
3. **禁止**在未确认旧 gateway 已停时，用同一 Token 启动第二个 polling。
4. **禁止**写入 `default_permissions = ":danger-no-sandbox"`。
5. **禁止**停止用户明确要求保留的现役网关（除非用户授权切换）。
6. **禁止**把本仓库示例 Bot 名 / 占位符当成真实密钥。

## 执行顺序

1. 读 [START-HERE.zh.md](START-HERE.zh.md) 与 [docs/01](docs/01-what-this-is.zh.md)
2. 预检 OS、Codex 登录状态（不打印凭据）、Hermes 是否已装
3. BotFather 项：引导用户，**不要替用户在聊天里接收 Token**；用安全输入通道
4. 按 [05](docs/05-hermes-codex-config.zh.md) 写配置
5. 启动：`hermes gateway run --accept-hooks`
6. 按 [07](docs/07-acceptance-checklist.zh.md) 验收；缺手机步骤就写 MANUAL_ACTION 停下来
7. 出问题只查 [06](docs/06-pitfalls-and-fixes.zh.md)，先根因再补丁

## 交付物

- 短 `STATE.md`：证据、缺口、启停命令、PASS_WITHIN_SCOPE | PARTIAL | BLOCKED
- 短 `USAGE.md`：用户怎么用手机 Bot
- 不要长篇散文；不要声称未测的 24h / Kanban / 双话题

## 官方链接

- https://hermes-agent.nousresearch.com/docs/user-guide/messaging/telegram/
- https://hermes-agent.nousresearch.com/docs/user-guide/features/codex-app-server-runtime/
- https://github.com/NousResearch/hermes-agent
