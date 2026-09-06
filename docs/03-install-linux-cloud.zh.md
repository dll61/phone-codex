# 03 — Linux / 云电脑安装

适合：任意 Linux，或「云电脑 / 远程 Linux 沙箱」（有出站网络即可；Telegram 默认 **polling**，一般**不需要**公网入站端口）。

## 0. 预检（先跑这些）

```bash
uname -m
python3 --version
node --version   # 若有
git --version
which codex || echo "need Codex CLI logged in"
codex login status 2>/dev/null || true
```

确认：

- 有可写目录做部署根（示例：`/workspace/hermes-codex`）
- Codex **已经** ChatGPT 登录（不要从别的机器抄 `auth.json` 到聊天里）
- 出站能访问 Telegram API 与 Codex

## 1. 安装官方 Hermes

按官方文档安装稳定版（版本号会变，以官网为准）：

- 文档：https://hermes-agent.nousresearch.com/
- 源码：https://github.com/NousResearch/hermes-agent

安装后确认：

```bash
hermes --version
# 或
python -m hermes_cli.main --help
```

记录实际 tag / CLI 版本，写进你自己的 `STATE.md`。

## 2. 部署目录建议

```text
/workspace/hermes-codex/          # 或你选的持久目录
  work/                           # terminal.cwd：给 Codex 干活
  logs/                           # gateway 日志
  # 文档 / 验收笔记（可选）
```

```bash
mkdir -p /workspace/hermes-codex/{work,logs}
```

把 Hermes 的 `terminal.cwd` 设成**绝对路径**，例如 `/workspace/hermes-codex/work`。

## 3. 导入 Codex 登录到 Hermes（小心别覆盖）

目标：让 Hermes 的 `openai-codex` provider 能用你已有的 ChatGPT 登录。

原则：

1. **优先复用本机已有 Codex 登录**，不要无故 `codex login` 覆盖
2. 若 Hermes 需要自己的 auth 副本：用官方导入/迁移流程，**备份**后再做
3. 永远不要把 `auth.json` 内容贴进聊天、README、Git

（具体子命令以你安装的 `hermes` 帮助为准，例如 setup / auth 相关。）

## 4. 写配置

见 [05 配置](05-hermes-codex-config.zh.md)。最少：

- Hermes：`model.provider=openai-codex`，`openai_runtime=codex_app_server`，模型例如 `gpt-5.6-sol`，`reasoning_effort=medium`
- Codex `config.toml`：**只要**

```toml
approval_policy = "never"
sandbox_mode = "danger-full-access"
```

- **删除**任何 `default_permissions = ":danger-no-sandbox"`

## 5. 写 `.env`（权限 0600）

```bash
umask 077
# 编辑 ~/.hermes/.env （路径以实际 HERMES_HOME 为准）
chmod 600 ~/.hermes/.env
```

关键键（值自己填）：

```env
TELEGRAM_BOT_TOKEN=YOUR_BOT_TOKEN_FROM_BOTFATHER
TELEGRAM_ALLOWED_USERS=YOUR_TELEGRAM_USER_ID
GATEWAY_ALLOWED_USERS=YOUR_TELEGRAM_USER_ID
# 可选：直接指定 home，避免一直提示 No home channel
TELEGRAM_HOME_CHANNEL=YOUR_TELEGRAM_USER_ID
TELEGRAM_HOME_CHANNEL_NAME=cloud-dm
```

`GATEWAY_ALLOW_ALL_USERS` 保持 false / 不要设成 true。

## 6. 启动 Gateway

前台验证：

```bash
hermes gateway run --accept-hooks
```

看状态：

```bash
hermes gateway status
```

日志可重定向到部署目录，例如：

```bash
hermes gateway run --accept-hooks >> /workspace/hermes-codex/logs/gateway.log 2>&1
```

## 7. 常驻（有 systemd 时）

```bash
hermes gateway install
# 再按官方说明 enable / start
```

**重要：** `nohup` / 手动后台 PID **≠** 已验证 24h 可用。没 supervisor 就在文档里写 **UNVERIFIED**，别吹牛。

云平台若会杀空闲进程：老实写 BLOCKED / PARTIAL，不要假装永远在线。

## 8. 手机验收

打开你的 `@YOUR_CLOUD_BOT`，发创建 `hello.txt` 的任务。清单见 [07](07-acceptance-checklist.zh.md)。

## 9. 停 / 启

```bash
hermes gateway status
hermes gateway stop     # 或对进程 SIGTERM
```

维护前若装了自动恢复，先禁用恢复任务，再停网关。
