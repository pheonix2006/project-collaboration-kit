# 当前开发状态

更新时间：2026-07-22

## 当前整体状态

第一版可 fork 标准项目骨架已经完成并进入 `main`。仓库自身正在使用这套入口、生命周期
和证据结构；其他项目可以直接 fork，然后按根 README 的最短定制步骤替换为项目真实内容。

## 已验证

- 根 `README.md` 和 `AGENTS.md` 已成为人和代理的真实入口，不再路由到独立理论说明层。
- specs、plans、dev-log、pitfalls、decisions、reports 均有真实目录、索引和生命周期边界。
- `docs/project-overview.md` 与本文件描述本仓库自身，不是待填写模板。
- 六份旧理论说明、旧案例和旧 `docs/superpowers/` 已从当前树删除，历史仍可从 Git 追溯。
- 本轮设计和计划已完成并移入 archive；active 目录为空，表示当前没有有效设计或可执行计划。
- 全部 Markdown 相对链接、敏感信息、机器绝对路径、Git 跟踪和空白错误已经检查。
- 本地 `main` 与 GitHub 远端通过提交 SHA 核验一致。

## 当前进行中

当前没有 active spec 或 active plan。后续工作不从归档计划自动恢复，只有出现真实复用
反馈并重新确认范围后才开始新的设计。

## 阻塞

当前没有已知阻塞。

## 下一步

1. 在一个真实新项目中 fork 本仓库，验证最短定制路径是否足够。
2. 在一个已有长期项目中对照这套结构，只采纳能解决真实问题的部分。
3. 根据多项目复用证据决定是否需要调整结构；不提前增加模板或自动化工具。

## 证据入口

- 历史设计：`docs/specs/archive/2026-07-22-standard-project-skeleton.md`
- 历史计划：`docs/plans/archive/2026-07-22-standard-project-skeleton.md`
- 项目长期边界：`docs/project-overview.md`
- 本次重构里程碑：`docs/dev-log/2026-07/2026-07-22-standard-project-skeleton.md`
- 2026 年 7 月索引：`docs/dev-log/2026-07/README.md`

历史材料只用于追溯，不得从旧 TODO、旧计划或旧说明中自动恢复任务。
