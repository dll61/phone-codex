# 06 — 踩坑大全（症状 → 原因 → 一行修复）

每个坑固定三行：**症状 / 原因 / 修复**。

---

## 1. Codex App-Server 直接崩溃 / 未知 profile

**症状：** Gateway 一调 Codex 就挂；日志里出现未知 permissions / profile 之类。

**原因：** 配置了 `default_permissions = ":danger-no-sandbox"`（不是当前可用的 profile 名）。

**修复：** 打开 Codex `config.toml`，**删掉**该行；只保留：

```toml
sandbox_mode = "danger-full-access"
approval_policy = "never"
```

然后重启 gateway。

---

## 2. Telegram 同一条回复发两次

**症状：** Bot 回两段一模一样的话；日志可能有 `possible duplicate send`。

**原因：** `display.interim_assistant_messages` + streaming 导致中间态和最终态都推到 Telegram。

**修复：** Hermes 配置：

```yaml
display:
  interim_assistant_messages: false
  streaming: false
streaming:
  enabled: false
```

重启 gateway。

---

## 3. 一直提示 No home channel

**症状：** 每次对话提醒设置 home channel。

**原因：** 未绑定 cron/跨平台通知用的 home。

**修复：**

```text
在目标聊天发送： /sethome
```

或：

```env
TELEGRAM_HOME_CHANNEL=YOUR_TELEGRAM_USER_ID
TELEGRAM_HOME_CHANNEL_NAME=my-dm
```

重启 gateway。

---

## 4. 两个 Bot / 两边抢同一个 Token

**症状：** `409 Conflict`、`terminated by other getUpdates`、消息丢失或乱跳。

**原因：** Telegram 一个 Bot Token **只允许一个长轮询消费者**。

**修复：** 云端、本机用**两个 Bot**；若必须同 Token，先停旧 gateway，确认无第二消费者再启新，**禁止双开**。

---

## 5. 白名单外完全没反应

**症状：** 别人能看到 Bot，但发消息没回；你自己有时也不回。

**原因：** `TELEGRAM_ALLOWED_USERS` / `GATEWAY_ALLOWED_USERS` 未包含你的数字 ID；或写错成用户名。

**修复：** 填正确数字 ID；`chmod 600` `.env`；重启。不要图省事开 `GATEWAY_ALLOW_ALL_USERS=true`（除非你真的要裸奔）。

---

## 6. 群里 Bot 装聋作哑

**症状：** 私聊正常，群里不搭理。

**原因：** BotFather **Group Privacy = ON**（默认常这样）。

**修复：** BotFather → Group Privacy **OFF**；Allow Groups **ON**。把 Bot 拉进群再试。

---

## 7. `terminal.cwd` 是相对路径，文件找不到

**症状：** 说写了文件但目录里没有；或写到奇怪位置。

**原因：** cwd 为 `.` 或相对路径，取决于启动时进程目录。

**修复：**

```bash
hermes config set terminal.cwd /绝对/路径/work
```

---

## 8. 以为 nohup 就是 24h 常驻

**症状：** 过几天 Bot 没了；平台休眠/会话结束进程没了。

**原因：** 文件还在 ≠ 进程还在。

**修复：** 有 systemd 用 `hermes gateway install`；没有就标明 UNVERIFIED，或换有用户级 supervisor 的环境。

---

## 9. Windows：计划任务找不到 Python

**症状：** 恢复任务 exit 非 0；`No Python at …`。

**原因：** 逻辑 AppData 路径与包物理路径不一致；venv `home=` 指向「只有某进程看得见」的解释器。

**修复：** 任务 Action 与 venv 都改用**外部也存在的物理路径**；先用官方幂等 `gateway start` 验证，再挂定时器。详见 [04](04-install-windows-local.zh.md)。

---

## 10. 从本机复制整份 auth / 会话库到云端

**症状：** 登录异常、会话串台、安全事故。

**原因：** 跨机盲拷凭据与 DB。

**修复：** 云端优先本机已有登录；必须 OAuth 时走官方流程；会话/Topics **各自重建**。

---

## 11. Kanban 工人权限和普通聊天不一样

**症状：** 私聊能写任意路径，Kanban 任务却被限制在 workspace-write / 无网络。

**原因：** Hermes 对 `HERMES_KANBAN_TASK` worker 有额外保护注入（版本相关）。

**修复：** 把 Kanban 当**单独验收项**；不要为图省事删保护。普通聊天成功 ≠ Kanban 成功。

---

## 12. 文档写 `:danger-full-access` 和本地版本对不上

**症状：** 按 OpenAI / Hermes 文档粘贴后报错。

**原因：** CLI 版本与文档不同步。

**修复：** 以**当前二进制** help / 报错为准；在隔离 `CODEX_HOME` 试验；记录版本号。本仓库推荐的稳妥云端组合是 `sandbox_mode` + `approval_policy`，并删除已知有害的 `:danger-no-sandbox`。
