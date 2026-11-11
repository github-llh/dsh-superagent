# Changelog

本文件记录 dsh-superagent（fork 自 [dsh-superagent-prototype](https://github.com/deepseek-ai/dsh-superagent-prototype) v0.1.0）的完善变更。
格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)。

## [Unreleased]

### Added

- **`orchestrator_dismiss` 主 Session 工具**：把面板专属的“消掉未完成决定的问题”动作工具化，与其余面板动作的工具等价性对齐（`docs/007-architecture.md` 主 Session 工具 14→15）。仅接受 `unavailable` 决定，其余抛 `unknownProblem`；`tests/tools.spec.ts` 补工具契约测试。
- **bundle 显示资源**：新增原创 `icon.svg`（自主循环 + 智能星芒意象），`package.json` 导出 `./icon` 并纳入 `files`；补齐 Plugin Manager 显示清单。
- **中文文档**：新增 `README.zh-CN.md`（中文定位、工作流、目录速览、开发指引）；`README.md` 顶部挂中文版链接。
- **变更记录**：新增本 `CHANGELOG.md`。

### Changed

- **依赖**：`vitest` 4.1.8 → 4.1.12（同 minor patch 升级，全量回归通过）。`tsdown`/`typescript` 已是最新，`@deepseek-ai/*` 保持 0.2.1-alpha.2 钉版不动。

### Fixed

- **生命周期竞态（源码审查发现，5 项）**：
  - `Runtime.initialize` 与 `shutdown` 竞态：启动各 await 之间补 `closing` 检查，`shutdown` 先等 `ready` 收敛再关库，`ready` 挂失败处理器（恢复进程环境、记日志，消除无人 await 时的 unhandledRejection）——消除“关停后使用已关闭的 Store / 主 Session 泄漏”。
  - `Sessions.install` 注册的 4 个宿主监听器与 `agentPresets` 注入从不注销：disposer 落盘、`dispose()` 逆序释放——消除 in-process restart 后的 use-after-close 与提示词/工具双份注册。
  - 状态目录锁失效（`onCompromised`）后仍继续写库：现置 closing、中止运行并 `stop()` 关库，杜绝与另一进程双写 `orchestrator.sqlite`。
  - `Store.atomic` 的 `SAVEPOINT` 移入 try 保护：开仓失败时回调丢弃、`depth` 正常回退，事务嵌套与 `afterCommit` 层级不再失衡。
  - `Assignments.started` 幂等化：对已 running 的派工重复调用（launch 的 accepted 回调 + 兜底双路径）不再重复 `assignment.started` 事件与投递入队。
- **健壮性（审查风险项 3 项）**：迁移读取坏历史行（空值/损坏 JSON）不再用非空断言、不再阻断旧库启动；`orchestrator_ack` 的回执状态改 `?? reply.status` 回退；启动失败路径恢复 `process.env`（`setup.restore()`）。
- **测试环境隔离**：`tests/fixtures/git.ts` 的 `gitEnv` 与 `tests/environment.spec.ts` 的 worker 环境固定 `LC_ALL=C`（git 消息钉为英文），消除测试对宿主 locale 的依赖——中文 locale 宿主上 2 个断言（`no email was given`、`bad config`）不再误报。1197 个用例全绿。

## [0.1.0] - 上游基线

- 上游研究原型首个发布版：Trigger→Eval→Ticket→Executor 全链路、GitHub/飞书 Source、SQLite 持久化、Effects 副作用队列、Web 面板、Session 工具与管理 API。详见上游 `README.md` 与 `docs/`。
