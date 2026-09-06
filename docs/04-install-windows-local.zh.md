# 04 — Windows 本机要点（不复制整套脚本）

Windows 本地可以跑同一条链路，但路径和「进程视角」比 Linux 坑。这里只记**原则**，不附整仓 PowerShell。

## 核心原则

1. **用官方 Hermes Windows 安装 / 启动方式**，不要从 Linux 目录盲拷 venv。
2. **逻辑路径 vs 物理路径**：若 Codex 装在 Microsoft Store / 包路径下，计划任务、其它进程可能看不见你在资源管理器里看到的 `AppData\Local\hermes\...`「逻辑」路径。  
   失败时核对 **Packages\...\LocalCache\Local\hermes\...** 这类物理路径。
3. **venv 的 `pyvenv.cfg` 里 `home =`** 必须指向「外部进程也看得到」的 Python；否则计划任务会报 `No Python at …`。
4. **恢复任务**：官方 Scheduled Task + 异步 VBS **不等于**「Python 崩了自动拉起」。若要「登录期间进程退出自动恢复」，需要单独、幂等的 `gateway start` 分钟级任务，并且：
   - 故意停机维护前 **先禁用** 恢复任务
   - 用**物理路径**指向 ensure 脚本
5. **不要**把 Windows `[windows]` sandbox 段原样抄到 Linux。

## 典型目录（示例，按你机器改）

```text
%LOCALAPPDATA%\hermes\                 # 逻辑 HERMES_HOME
%LOCALAPPDATA%\hermes\deployment\      # 部署笔记 / 恢复脚本
%LOCALAPPDATA%\hermes\telegram-workspace\   # 常见 terminal.cwd
```

Store/包路径机器还可能有物理镜像，以你本机实际为准。

## 配置同样适用

- Hermes：`openai-codex` + `codex_app_server`
- Codex：`sandbox_mode=danger-full-access`，`approval_policy` 按你需求（全自动可用 `never`）
- **禁止** `:danger-no-sandbox` 作为 profile 名乱写
- Telegram：独立 Bot Token + allowlist
- 重复回复：关 interim + streaming（见 [06](06-pitfalls-and-fixes.zh.md)）

## 与云端关系

| | 本机 Bot | 云端 Bot |
|--|----------|----------|
| Token | 各自一个 | 各自一个 |
| 会话 / Topics | 不互通 | 不互通 |
| 切换方式 | 手机点开不同 Bot | 同左 |

## 下一步

配置细节 → [05](05-hermes-codex-config.zh.md)。踩坑 → [06](06-pitfalls-and-fixes.zh.md)。
