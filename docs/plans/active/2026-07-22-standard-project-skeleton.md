# 标准项目骨架实施计划

日期：2026-07-22

状态：当前可执行

## 目标

将仓库从“机制说明文档集合”重构为可以直接 fork 的标准项目骨架。

## 全局约束

- 不创建 `templates/`、隐藏管理目录、Profile、CLI 或同步工具。
- 仓库自身必须真实使用最终结构。
- 旧理论文档从当前树删除，必要规则只保留一个权威落点。
- 空生命周期目录通过 `.gitkeep` 进入 Git。
- 所有写作使用中文，必要英文术语保留原文。

## Task 1：重写根入口

- [x] 重写 `README.md`，使其成为可 fork 骨架的使用入口。
- [x] 重写 `AGENTS.md`，写入稳定协作规则、读取顺序、写回规则和项目地图。
- [x] 验证两份入口不再把本仓库描述成机制说明文档集合。

## Task 2：建立真实文档结构

- [x] 创建 `docs/README.md` 文档路由。
- [x] 创建 specs、plans、dev-log、pitfalls、decisions、reports 的索引。
- [x] 用 `.gitkeep` 保留当前为空的 archive 和 current 目录；active 在最终归档时补齐。
- [x] 确认目录位置能直接表达设计、计划和报告状态。

## Task 3：写入仓库自身的真实状态

- [x] 创建 `docs/project-overview.md`，描述本仓库自身。
- [x] 创建 `docs/current-development-status.md`，记录本次重构状态。
- [x] 创建当月 dev-log 索引和本次重构里程碑记录。
- [x] 确认文档不是待填写模板或占位说明。

## Task 4：删除旧说明层并收口

- [x] 删除六份独立理论说明和旧案例。
- [x] 删除旧 `docs/superpowers/` 结构，依赖 Git 历史追溯。
- [ ] 将本设计和本计划移入各自 archive，并完成状态更新。
- [ ] 检查所有 Markdown 相对链接。
- [ ] 运行范围扫描和 `git diff --check`。
- [ ] 提交并推送 `main`，核验本地 HEAD 与 `origin/main` 一致。
