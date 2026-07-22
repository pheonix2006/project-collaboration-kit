# 当前开发状态

更新时间：2026-07-22

## 当前重构目标

把本仓库从“跨会话合作机制说明文档集合”重构为仓库自身真实使用、可以直接 fork 的
标准项目骨架。当前实施依据是
`docs/specs/active/2026-07-22-standard-project-skeleton.md`，执行边界由
`docs/plans/active/2026-07-22-standard-project-skeleton.md` 约束。

## 已验证

- 标准项目骨架的设计和实施计划已经进入 `specs/active/` 与 `plans/active/`，目录位置明确
  表示它们当前有效、当前可执行。
- 目标生命周期目录已经建立：specs、plans、dev-log、pitfalls、decisions、reports 均有实际
  目录，当前与归档层级已分开。
- 本仓库使用 Git 管理，当前分支为 `main`，远端 `origin` 指向本项目的 GitHub 仓库。
- 本轮已开始写入仓库自身的项目总览、唯一当前状态和 2026 年 7 月里程碑记录；这些内容
  描述当前重构，不是供下游填写的占位模板。

## 当前进行中

- 重写面向人和代理的根入口，使仓库定位从机制说明切换为可 fork 骨架。
- 补齐各文档目录的路由索引和空目录保留方式。
- 写入本仓库自身的项目总览、当前状态和本次重构证据。

以上工作属于同一个骨架重构里程碑。在全部任务完成前，旧理论说明和旧
`docs/superpowers/` 仍可能出现在工作树中，不代表它们继续作为最终入口。

## 阻塞

当前没有已知外部阻塞。尚未完成的内容属于计划内收口工作，而不是基础设施或权限阻塞。

## 下一步

1. 完成根入口和所有目录索引的整合验证。
2. 删除已被新骨架吸收的六份旧理论说明、旧案例和旧 `docs/superpowers/` 结构。
3. 将本轮 active 设计和计划在实施完成后移入各自 archive，并更新状态。
4. 检查全部 Markdown 相对链接、敏感信息、机器绝对路径和 Git 跟踪状态。
5. 运行 `git diff --check`，审阅完整 diff 后提交并推送 `main`，核对本地与远端一致。

## 证据入口

- 当前设计：`docs/specs/active/2026-07-22-standard-project-skeleton.md`
- 当前计划：`docs/plans/active/2026-07-22-standard-project-skeleton.md`
- 项目长期边界：`docs/project-overview.md`
- 本次重构里程碑：`docs/dev-log/2026-07/2026-07-22-standard-project-skeleton.md`
- 2026 年 7 月索引：`docs/dev-log/2026-07/README.md`

历史材料只用于追溯，不得从旧 TODO、旧计划或旧说明中自动恢复任务。
