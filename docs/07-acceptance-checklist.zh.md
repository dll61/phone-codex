# 07 — 验收清单（打勾再宣称成功）

原则：**安装成功 / 有 PID / 模型嘴上说 OK ≠ 通过。** 要有文件字节和 Telegram 真实回复。

## A. 最小 E2E（必过）

- [ ] `hermes gateway status` 显示 Gateway running
- [ ] 手机只给**白名单账号**发消息
- [ ] 任务示例：

```text
请在 <你的 terminal.cwd> 创建 hello.txt，内容必须是 hello from codex
```

- [ ] 工作目录出现 `hello.txt`
- [ ] 文件内容**精确**等于 `hello from codex`（注意有无多余换行）
- [ ] Telegram **只收到一条**最终回复（不是双发）
- [ ] 不再无意义刷「No home channel」（或已 `/sethome`）

## B. 配置核验（必过）

- [ ] Hermes：`provider=openai-codex`，`openai_runtime=codex_app_server`
- [ ] `terminal.cwd` 为绝对路径
- [ ] Codex：**无** `default_permissions = ":danger-no-sandbox"`
- [ ] Codex：有 `sandbox_mode = "danger-full-access"`（若你需要完整写权限）
- [ ] `.env` 权限 `0600`；Token 未进 Git
- [ ] allowlist 为数字 User ID

## C. 进阶（可选，未测就标 UNVERIFIED）

- [ ] 两个 Topics / 会话写不同标识文件，不串线
- [ ] Kanban 两工人时间重叠 + 下游汇总
- [ ] 人为杀掉 gateway 后，**原生恢复机制**自动拉起，再发一条手机任务通过
- [ ] 记录故障→重连耗时（示例量级：约数十秒，以你机器为准）
- [ ] `hermes gateway install` / systemd 用户服务真正在登出或会话结束后仍符合你的预期

## D. 明确不要宣称

- [ ] 未测 24h → 不写「永远在线」
- [ ] 云端 Bot 与本地 Bot **不会**自动互聊
- [ ] 无证据不写「网络卡顿已修好」

## E. 状态词建议

| 词 | 含义 |
|----|------|
| PASS_WITHIN_SCOPE | 声明范围内的项都有证据 |
| PARTIAL | 主链路通，常驻/并行等未测 |
| BLOCKED | 平台或权限阻止目标，说清唯一阻断项 |
| UNVERIFIED | 没测，别猜 |
