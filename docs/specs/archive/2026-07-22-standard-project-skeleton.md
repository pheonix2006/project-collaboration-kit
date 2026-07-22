# 标准项目骨架设计

日期：2026-07-22

状态：已完成，仅作历史设计

## 目标

把本仓库重构为一个可以直接 fork 的标准项目骨架。仓库自身真实使用这套结构，而不是
单独维护一套理论说明或模板目录。

fork 后，使用者直接修改项目内容和规则；不需要安装工具、同步上游模板或引入隐藏管理
目录。

## 最终结构

```text
AGENTS.md
README.md
docs/
  README.md
  project-overview.md
  current-development-status.md
  specs/
    README.md
    active/
    archive/
  plans/
    README.md
    active/
    archive/
  dev-log/
    README.md
    YYYY-MM/
  pitfalls/
    README.md
  decisions/
    README.md
  reports/
    README.md
    current/
    archive/
```

空目录使用 `.gitkeep` 保留。索引文件负责说明当前位置的职责和读取方式，不建立独立的
理论文档层。

## 入口职责

- `AGENTS.md`：稳定协作规则、渐进读取顺序、写回位置、验证要求和项目地图。
- `README.md`：面向人的仓库入口、fork 使用方法和最短启动步骤。
- `docs/README.md`：文档路由和目录状态解释。
- `docs/project-overview.md`：项目使命、目标、非目标、总体结构和长期质量标准。
- `docs/current-development-status.md`：唯一当前状态、阻塞和统一下一步。

## 生命周期

- `specs/active/` 只保存当前有效设计，完成或被替代后进入 archive。
- `plans/active/` 只保存已确认且当前允许执行的计划，完成或取消后进入 archive。
- `dev-log/` 按月份和里程碑保存实施与验证证据。
- `pitfalls/` 保存跨任务可复用的故障规律。
- `decisions/` 保存难以从代码或当前设计直接解释的重大既成决定。
- `reports/current/` 保存当前权威报告，被替代后进入 archive。

历史材料不得自动恢复为当前任务。目录位置直接表达 spec、plan 和 report 的状态。

## Fork 后的最短定制

1. 修改根 `README.md` 的项目名称、定位和使用方式。
2. 修改 `docs/project-overview.md` 的使命、目标、非目标和项目结构。
3. 修改 `docs/current-development-status.md`，写入真实当前状态和下一步。
4. 修改 `AGENTS.md` 的项目地图、验证命令和项目特定边界。
5. 清理本仓库遗留的历史 dev-log、归档设计和归档计划。

其他目录只在实际需要时使用，不要求为了结构完整而制造空文档。

## 删除与保留

删除当前独立的 `principles.md`、`source-of-truth.md`、`document-lifecycle.md`、
`cross-session-workflow.md`、`customization-guide.md` 和案例说明。仍然有效的规则压缩进入
`AGENTS.md`、`docs/README.md` 和各目录索引。

旧设计和旧实施计划不迁入新结构；Git 历史已经保留其演进证据。

## 验收标准

1. 根目录能直接看出这是可 fork 的项目骨架。
2. README 不再把仓库描述为一组机制说明文档。
3. 所有目标目录真实存在并进入 Git。
4. 新会话只读 `AGENTS.md` 即可知道如何渐进加载和写回。
5. 当前状态、当前设计、当前计划、历史证据和归档材料边界清楚。
6. 被删除理论文档中的必要规则在实际入口或目录索引中有唯一落点。
7. 相对链接有效，正式文件不包含本机绝对路径、凭据或项目特定敏感信息。
8. `main` 工作树干净，并与 GitHub 远端一致。
