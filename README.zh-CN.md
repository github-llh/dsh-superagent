# dsh-superagent · 自主编码智能体

> 基于 [dsh-superagent-prototype](https://github.com/deepseek-ai/dsh-superagent-prototype)（MIT）fork 并持续完善的研究原型；面向 [DeepSeek Harness](https://www.deepseek.com/en/harness/)（`dsh`）的 bundle 插件。
>
> ⚠️ 研究原型，非 DeepSeek 官方产品；代码源自团队日常开发实践，请在生产使用前充分审阅与适配。

一句话定位：**盯住一个 GitHub 仓库（可选飞书），谁在允许名单里提出请求，就开一张 Ticket，在独立的 DSH 会话与工作目录里把活干完，再回到原对话交付结果（例如一个 Pull Request）；Web 面板上只显示需要人来决定的事。**

工作方式的核心承诺：**尽量少占用人的注意力，尽量多交付价值。** 只在被请求、或团队自己写的规则允许时才行动；只在需要人介入、或有结果可用时才发声。

## 工作流

```
Source（GitHub/飞书） → Trigger（触发） → Eval（准入 + 研判） → Ticket（工单） → Assignment（派工） → Executor Session（执行器会话） → Channel（回到原对话）
```

- **Trigger**：来源事件的不可变记录，审计与幂等的单位。
- **Policy**：版本化的接单规则——谁可以在哪个来源上发起工作、哪些机器人可产生候选；规则变了不撤销已有授权，而是把变化送达受影响的工单。
- **Ticket**：一份获授权工作的单位，携带 Mandate（授权依据）、输入与派工历史；人的一句话可以随时让它续跑或停止。
- **Executor Session**：每个 Ticket 一个独立 DSH 会话 + 独立目录，互不干扰，可并行多张工单。

## 目录速览

| 路径 | 内容 |
|---|---|
| `src/core/` | 领域核心：Engine（Trigger/Eval/Ticket/Assignment 的唯一写入者）、Policy、领域模型 |
| `src/app/` | 运行时装配：Store（SQLite）、Effects（持久化副作用队列）、Session 工具、管理 HTTP API、面板视图数据 |
| `src/sources/` | Source 插件：GitHub（通知/Issue/PR/CI）与飞书（可选） |
| `src/client/` | Web 面板（React）：总览、各阶段状态、工单/线程详情、设置、Prompts |
| `prompts/` | 面向模型的提示词（评估、派工、回复、交接等） |
| `docs/` | 编号设计文档（英文）：哲学 → 领域模型 → 授权 → 执行 → 提示词 → 面板 → 部署 → 架构 → 食谱 |
| `tests/` | vitest 测试（1197 个用例：领域规则、持久化、并发、面板、部署脚本等） |

## 开发


```bash
npx pnpm@11.7.0 install     # 依赖（packageManager 钉版 pnpm@11.7.0）
npx pnpm@11.7.0 check       # typecheck + 全量测试 + 构建
```

- 运行前置（部署运行时才需要）：Node.js 22.19+ 或 24+、git 2.32+、tmux、GitHub CLI 2.48+、DSH `^0.2.1-alpha.2`；飞书可选 lark-cli。
- 安装与配置走 `SETUP.md` 的引导式流程（让 agent 跟你一问一答完成）。

## 文档

英文编号文档按顺序阅读，每篇只依赖前面的篇目：

- [000 Philosophy](docs/000-philosophy.md) · [001 Domain model](docs/001-domain.md) · [002 Authorization](docs/002-authorization.md)
- [003 Execution](docs/003-execution.md) · [004 Prompts](docs/004-prompts.md) · [005 Panel](docs/005-panel.md)
- [006 Deployment](docs/006-deployment.md) · [007 Architecture](docs/007-architecture.md) · [008 Cookbook](docs/008-cookbook.md)

## 本 fork 的完善记录

见 [CHANGELOG.md](CHANGELOG.md)。上游英文原版见 [README.md](README.md)。

## 许可

[MIT](LICENSE)
