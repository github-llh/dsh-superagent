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

- **测试环境隔离**：`tests/fixtures/git.ts` 的 `gitEnv` 与 `tests/environment.spec.ts` 的 worker 环境固定 `LC_ALL=C`（git 消息钉为英文），消除测试对宿主 locale 的依赖——中文 locale 宿主上 2 个断言（`no email was given`、`bad config`）不再误报。1197 个用例全绿。

## [0.1.0] - 上游基线

- 上游研究原型首个发布版：Trigger→Eval→Ticket→Executor 全链路、GitHub/飞书 Source、SQLite 持久化、Effects 副作用队列、Web 面板、Session 工具与管理 API。详见上游 `README.md` 与 `docs/`。
