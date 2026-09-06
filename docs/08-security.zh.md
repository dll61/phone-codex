# 08 — 安全清单（上线前）

## Token 与凭据

- [ ] `TELEGRAM_BOT_TOKEN` 只在本机 `.env` 或平台「安全输入」
- [ ] `.env` 权限：`chmod 600`
- [ ] `auth.json` / ChatGPT 登录态：**不进 Git、不进聊天、不进公开文档**
- [ ] 仓库 `.gitignore` 已排除 `.env`、`auth.json`、密钥文件
- [ ] 泄露后立刻 BotFather `/revoke` 换新 Token，并轮换相关登录

## 访问控制

- [ ] `TELEGRAM_ALLOWED_USERS` + `GATEWAY_ALLOWED_USERS` 白名单
- [ ] **不要**轻易 `GATEWAY_ALLOW_ALL_USERS=true`
- [ ] BotFather：**Guest Chat OFF**，**Bot Management OFF**
- [ ] 群组场景才开 Allow Groups；Privacy 按 [02](02-botfather-setup.zh.md) 设置

## 权限边界

- [ ] `approval_policy = never` + `danger-full-access` = **高权限自动执行**  
  只在你信任的机器 + 白名单用户下使用
- [ ] 「整机访问」仍受 OS 用户权限 / 云平台沙箱限制，≠ root
- [ ] Kanban worker 可能更严；不要为省事拆掉官方保护
- [ ] 不要把整份主机 `.env` 传给子进程「图方便」

## 运维

- [ ] 云、本机双 Bot，避免 polling 冲突导致异常行为
- [ ] 日志里不要 `echo $TELEGRAM_BOT_TOKEN`
- [ ] 公开 Issue / PR 用占位符：`YOUR_BOT_TOKEN`、`YOUR_TELEGRAM_USER_ID`

## 本仓库承诺

本开源包**只含文档与示例**。若你 fork 后不慎提交密钥，责任在提交者；发现后立即轮换。
