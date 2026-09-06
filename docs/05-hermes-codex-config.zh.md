# 05 — Hermes × Codex 关键配置（抄对就能跑）

以下为**脱敏示例**。路径、模型名按你账号实际可用列表调整。

## A. Hermes `config.yaml`（关键片段）

```yaml
model:
  default: gpt-5.6-sol          # 示例；以你账号可用模型为准
  provider: openai-codex
  openai_runtime: codex_app_server
  reasoning_effort: medium

agent:
  reasoning_effort: medium

terminal:
  backend: local
  cwd: /workspace/hermes-codex/work   # Windows 换成绝对路径

# 网关侧流式（有的版本在 gateway.streaming）
streaming:
  enabled: false

gateway:
  streaming:
    enabled: false

display:
  interim_assistant_messages: false   # 防 Telegram 重复最终回复
  streaming: false
```

设置 cwd 示例：

```bash
hermes config set terminal.cwd /workspace/hermes-codex/work
```

## B. Codex `config.toml`（顶层）

**推荐（云端实测可用）：**

```toml
approval_policy = "never"
sandbox_mode = "danger-full-access"
```

### 绝对不要这样写

```toml
# ❌ 会让 Codex App-Server 因「未知 profile」直接挂掉（真实踩坑）
default_permissions = ":danger-no-sandbox"
```

说明：

- Hermes 部分旧文档/迁移注释里出现过 `:danger-no-sandbox` 字样
- 当前实践：**删掉该键**；权限用 `sandbox_mode = "danger-full-access"` 表达
- 不要「新旧两套键都写上保险」——容易更乱
- OpenAI 文档若出现 `default_permissions = ":danger-full-access"`，以**你本机 Codex 版本实际能否解析**为准，先在隔离 `CODEX_HOME` 试，再用于生产

## C. `.env.example`

见仓库 [`config/examples/.env.example`](../config/examples/.env.example)。

最少：

```env
TELEGRAM_BOT_TOKEN=
TELEGRAM_ALLOWED_USERS=
GATEWAY_ALLOWED_USERS=
TELEGRAM_HOME_CHANNEL=
TELEGRAM_HOME_CHANNEL_NAME=my-dm
```

## D. Home Channel

两种办法任选：

1. 在你常用的聊天里对 Bot 发：`/sethome`
2. 或在 `.env` 写：

```env
TELEGRAM_HOME_CHANNEL=YOUR_TELEGRAM_USER_ID
TELEGRAM_HOME_CHANNEL_NAME=cloud-dm
```

然后重启 gateway。

## E. 辅助项（可选）

若官方支持，可关闭自动起标题，减少旁路模型消耗：

- `auxiliary.title_generation.enabled=false`
- 以及文档中的 free_only 类开关（**不要**理解成「主 Agent 完全免费」）

## F. 改完必须重启

```bash
hermes gateway stop
hermes gateway run --accept-hooks
# 或 systemctl --user restart …（若已 install）
```

## 官方参考

- https://hermes-agent.nousresearch.com/docs/user-guide/features/codex-app-server-runtime/
- https://hermes-agent.nousresearch.com/docs/user-guide/messaging/telegram/
