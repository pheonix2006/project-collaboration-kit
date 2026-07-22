# Initial Collaboration Mechanism Implementation Plan

状态：已完成，仅作实施证据，不再作为当前待办

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 交付一套可独立阅读、可由其他项目选择性复用的跨会话协作机制规范，并发布到私有 GitHub 仓库。

**Architecture:** 仓库只包含机制说明，不包含模板、隐藏管理目录、Profile、CLI、Schema、初始化或同步工具。README 提供短入口，六份主题文档分别承担原则、source of truth、生命周期、跨会话工作流、项目定制和 benchmark_eval 案例；主题之间只做摘要与链接，避免同一规则重复成为多个权威来源。

**Tech Stack:** Markdown、Git、GitHub CLI；验证使用 `rg`、`git diff --check` 和本地相对链接检查命令。

## Global Constraints

- 对话和文档使用中文，必要英文术语保留原文。
- 第一版不提供模板、`.collabkit/`、Project Profile、CLI、脚本、Schema、迁移或自动同步。
- 不强制下游项目复制固定目录结构；复用对象是信息职责、source of truth、生命周期和读写协议。
- `benchmark_eval` 只作为脱敏案例，不复制模型、凭据、机器绝对路径、运行分数或具体业务配置。
- 每份文档只承担一个主要职责，重复内容使用相对链接指向权威说明。
- 所有 Markdown 相对链接必须存在，`git diff --check` 必须通过。

---

### Task 1: 仓库入口与核心原则

**Files:**
- Create: `README.md`
- Create: `AGENTS.md`
- Create: `docs/principles.md`

**Interfaces:**
- Consumes: `docs/superpowers/specs/2026-07-22-project-collaboration-kit-design.md`
- Produces: 仓库定位、阅读路由、本仓库协作规则和核心原则，供其余主题文档引用。

- [x] **Step 1: 编写 README**

README 必须包含：一句话定位、解决的问题、适用/不适用场景、六种信息职责摘要、推荐阅读顺序、如何在其他项目中选择性采用、第一版明确不做的事项，以及指向全部主题文档和设计规格的相对链接。

- [x] **Step 2: 编写本仓库 AGENTS.md**

AGENTS 必须明确：默认中文；主代理与子代理职责；渐进读取；本仓库第一版仅维护规范；文档职责和防重复规则；修改后检查相对链接、敏感信息、绝对路径和 `git diff --check`。它是本仓库自身规则，不得称为下游模板。

- [x] **Step 3: 编写核心原则**

`principles.md` 必须解释：对话不是权威来源、渐进加载、当前与历史分离、一类事实一个主要权威位置、目录/索引可表达状态但不强制目录、验证分层、多代理分工，以及至少六个反模式。

- [x] **Step 4: 验证 Task 1**

Run: `rg -n '\.collabkit|Project Profile|初始化脚本|自动同步' README.md AGENTS.md docs/principles.md`
Expected: 只允许在“第一版不做”语境出现，不能把这些内容描述为交付能力。

Run: `git diff --check`
Expected: exit 0。

### Task 2: Source of truth 与生命周期

**Files:**
- Create: `docs/source-of-truth.md`
- Create: `docs/document-lifecycle.md`

**Interfaces:**
- Consumes: 设计规格第 5 至第 7 节。
- Produces: 信息职责映射和生命周期语义，供工作流与定制指南引用。

- [x] **Step 1: 编写 source of truth 说明**

文档必须用表格区分：稳定规则与路由、项目总览、当前状态、当前设计、当前计划、里程碑证据、可复用经验、当前报告、历史材料、代码/测试/配置/运行产物。每行给出回答的问题、主要权威来源、不得承载的内容和更新触发条件。

- [x] **Step 2: 解释冲突处理**

必须给出事实冲突优先级：实际代码/配置/运行证据优先于描述性文档；当前权威入口优先于历史材料；无法验证时明确标记未知，不用旧记录补全。至少给出三个重复记录导致漂移的例子和修复方式。

- [x] **Step 3: 编写生命周期说明**

文档必须定义“草拟、当前有效、当前可执行、已完成、已替代、历史材料”六种语义，说明设计、计划、状态、报告和日志分别如何流转，并强调归档不是删除、历史 TODO 不能自动恢复。项目可选目录位置、索引或显式元数据表达状态。

- [x] **Step 4: 验证 Task 2**

Run: `rg -n '当前有效|当前可执行|已完成|已替代|历史材料' docs/document-lifecycle.md`
Expected: 六种生命周期语义除“草拟”外均有独立定义；`草拟` 也必须存在。

Run: `git diff --check`
Expected: exit 0。

### Task 3: 跨会话工作流与项目定制

**Files:**
- Create: `docs/cross-session-workflow.md`
- Create: `docs/customization-guide.md`

**Interfaces:**
- Consumes: `docs/source-of-truth.md`、`docs/document-lifecycle.md` 约定的职责和状态语义。
- Produces: 可操作的会话读取/写回流程和按项目规模取舍方法。

- [x] **Step 1: 编写会话启动流程**

`cross-session-workflow.md` 必须覆盖：任务分类、默认最短读取链、按索引定位证据、何时读取当前设计/计划、禁止默认读取的材料、信息不足时如何扩大读取范围。

- [x] **Step 2: 编写执行与结束流程**

必须覆盖：主代理/子代理分工、边界决策门、验证层级、里程碑写回决策树、归档同步更新和最终交接检查清单。使用一个简短端到端示例展示新会话如何恢复并写回。

- [x] **Step 3: 编写定制指南**

`customization-guide.md` 必须给出小型短期项目、长期软件项目、研究/实验项目三种最小采用建议；每类说明应保留、可合并和可省略的职责。必须强调先解决真实痛点，再增加文档类型；不复制 benchmark_eval 目录。

- [x] **Step 4: 验证 Task 3**

Run: `rg -n '小型|长期软件|研究|默认整读|写回|子代理' docs/cross-session-workflow.md docs/customization-guide.md`
Expected: 所有关键主题均有命中。

Run: `git diff --check`
Expected: exit 0。

### Task 4: benchmark_eval 案例与全仓集成

**Files:**
- Create: `docs/case-study/benchmark-eval.md`
- Modify: `README.md`
- Modify: `docs/principles.md`
- Modify: `docs/source-of-truth.md`
- Modify: `docs/document-lifecycle.md`
- Modify: `docs/cross-session-workflow.md`
- Modify: `docs/customization-guide.md`

**Interfaces:**
- Consumes: 前三项任务的全部文档和 `/Users/deepwisdom/Desktop/project/benchmark_eval` 当前文档机制。
- Produces: 脱敏案例、完整导航和统一术语。

- [x] **Step 1: 编写 benchmark_eval 案例**

案例必须包含：原始症状、演进前的问题、形成的通用机制、仍然存在的局限、哪些内容不能泛化、其他项目可复用的判断方法。案例可以引用目录类别，但不写模型、密钥、绝对路径、具体成绩和一次性运行命令。

- [x] **Step 2: 统一术语和交叉链接**

统一使用“稳定规则与入口、项目总览、当前状态、当前设计、当前计划、历史证据、可复用经验”术语。补齐主题文档之间必要的相对链接，但不得通过复制大段正文建立导航。

- [x] **Step 3: 检查范围与敏感内容**

Run: `rg -n '/Users/|API[_ -]?KEY|token|qwen|gemini|glm|\.collabkit|Project Profile|collabkit (init|sync|check)' README.md AGENTS.md docs --glob '*.md' --glob '!docs/superpowers/**'`
Expected: 第一版正式说明中不得出现本机路径、凭据字段、模型名、`.collabkit`、Project Profile 或不存在的 CLI 能力。

- [x] **Step 4: 检查所有 Markdown 相对链接**

Run: `ruby -e 'Dir.glob("**/*.md").each { |f| File.read(f).scan(/\[[^\]]+\]\((?!https?:|#)([^)#]+)(?:#[^)]+)?\)/).flatten.each { |p| q=File.expand_path(p,File.dirname(f)); abort("broken: #{f} -> #{p}") unless File.exist?(q) } }'`
Expected: exit 0，无 broken link。

- [x] **Step 5: 完整验证并提交**

Run: `git diff --check && git status --short && git diff --stat main...HEAD`
Expected: diff check exit 0，状态只包含计划内 Markdown，分支差异覆盖全部第一版交付物。

### Task 5: 发布到 GitHub

**Files:**
- No repository content changes unless review fixes are required.

**Interfaces:**
- Consumes: 已通过最终审查和完整验证的实现分支。
- Produces: 私有 GitHub 仓库、远端跟踪分支和 Draft PR。

- [x] **Step 1: 创建私有远端并推送 main**

Run: `gh repo create project-collaboration-kit --private --source=. --remote=origin --description '跨会话长期项目协作机制规范'`
Expected: 创建 `pheonix2006/project-collaboration-kit`，配置 `origin`。

Run: `git push -u origin main`
Expected: 远端 main 包含两次设计提交。

- [x] **Step 2: 推送实现分支**

Run: `git push -u origin agent/initial-collaboration-mechanism`
Expected: 设置远端跟踪且无拒绝。

- [x] **Step 3: 创建 Draft PR**

先通过 `apply_patch` 创建 `/tmp/project-collaboration-kit-pr.md`，正文固定包含“变更内容、设计动机、使用影响、验证证据”四节及实际验证命令。然后运行：

Run: `gh pr create --draft --base main --head agent/initial-collaboration-mechanism --title 'docs: establish cross-session collaboration mechanism' --body-file /tmp/project-collaboration-kit-pr.md`
Expected: 返回可访问的 GitHub PR URL；正文包含变更、动机、影响和验证证据。

- [x] **Step 4: 远端核验**

Run: `gh repo view --json nameWithOwner,isPrivate,defaultBranchRef,url && gh pr view --json number,state,isDraft,url,baseRefName,headRefName`
Expected: 仓库为 private、默认分支为 main，PR 状态 OPEN 且 isDraft=true。

## 发布结果

- 私有仓库：`https://github.com/pheonix2006/project-collaboration-kit`
- 默认分支：`main`
- 实现分支：`agent/initial-collaboration-mechanism`
- Draft PR：`https://github.com/pheonix2006/project-collaboration-kit/pull/1`
- 远端核验：仓库 `isPrivate=true`，PR `state=OPEN` 且 `isDraft=true`。
