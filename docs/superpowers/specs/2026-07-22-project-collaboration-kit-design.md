# Project Collaboration Kit 设计规格

日期：2026-07-22

状态：已批准，待实现

## 1. 项目定位

Project Collaboration Kit（仓库名 `project-collaboration-kit`，命令名
`collabkit`）是一套可版本化分发的跨会话项目协作协议与工具包。它不集中保存各业务
项目的真实状态，而是向每个目标项目安装一套稳定的协作入口、文档生命周期、模板和
一致性检查。

每个业务项目继续在自己的 Git 仓库中保存项目目标、当前状态、设计、执行计划、开发
证据、踩坑和报告。CollabKit 仓库只保存通用协议、生成模板、项目 Profile、CLI、迁移
逻辑和测试。

`benchmark_eval` 是第一个迁移试点和 benchmark Profile 示例，不是内核的默认语义。

## 2. 要解决的问题

CollabKit 解决跨会话、跨代理和长期项目中的四类问题：

1. 新会话无法在低上下文成本下恢复当前目标、边界、阻塞和下一步。
2. 当前事实、历史证据、过期计划和旧结论混在一起，导致代理误执行。
3. 决策、验证证据与可复用故障知识只留在聊天或临时产物里，无法稳定传承。
4. 多个项目复制同一套规则后各自漂移，无法安全接收通用机制升级。

## 3. 目标与非目标

### 3.1 目标

- 提供明确的 source of truth 分层和目录生命周期。
- 让新会话默认只加载短入口，再根据任务渐进读取相关材料。
- 同时支持新项目初始化和已有项目非破坏式接入。
- 将通用规则升级与项目事实、项目定制解耦。
- 提供本地与 CI 都可运行的一致性检查。
- 对协议版本、生成文件和迁移过程提供可追溯信息。
- 仅依赖 Python 标准库运行，降低目标项目接入成本。

### 3.2 非目标

- 不建设中心化知识库、文档 SaaS 或跨仓库状态数据库。
- 不替代 Git、Issue、Pull Request 或具体项目管理平台。
- 不强制所有项目采用相同业务术语、测试命令或交付流程。
- 不把 benchmark adapter、runtime、judge、模型、凭据、机器路径或运行产物写入内核。
- 不自动总结聊天记录，也不把聊天记录视为权威事实。
- 第一版不提供可视化后台、插件市场或远程执行服务。

## 4. 用户与核心场景

### 4.1 用户

- 维护多个长期项目、需要频繁切换会话的人。
- 在同一仓库中使用 Codex、Claude Code 或其他代理的团队。
- 希望统一协作约定，但又需要按软件、研究或 benchmark 项目定制的维护者。

### 4.2 核心场景

1. 在空仓库中初始化完整但轻量的协作骨架。
2. 将已有仓库接入协议，并保留原有 `AGENTS.md` 内容和已有文档。
3. 新会话从根入口恢复状态，不默认吞入 archive 或完整历史。
4. 项目维护者升级 CollabKit 后同步通用规则，不覆盖项目事实。
5. CI 检查文档路由、唯一当前状态、生命周期位置、链接和陈旧度。

## 5. 三层架构

### 5.1 通用内核

通用内核由以下内容组成：

- 协作协议和 source of truth 定义。
- 新会话读取顺序与写回规则。
- 状态、设计、计划、日志、踩坑、报告和决策模板。
- 生命周期与归档规则。
- CLI、校验器、迁移器和生成清单。
- 协议版本和兼容策略。

### 5.2 Project Profile

Profile 只增加领域默认值，不改变核心生命周期。第一版提供：

- `general`：最小通用项目。
- `software`：增加代码地图、测试和发布入口。
- `research`：增加研究问题、证据等级和结论晋升规则。
- `benchmark`：增加运行证据、评分口径与当前报告路由，作为
  `benchmark_eval` 迁移示例。

Profile 可以增加模板章节、建议目录和校验规则，但不得写入具体项目名、绝对路径、凭据
或业务结论。

### 5.3 项目实例

项目实例保存在目标项目自身仓库中，包括：

- 项目简介、目标与非目标。
- 当前状态和当前下一步。
- 当前有效设计、已授权计划和 ADR。
- 里程碑证据、踩坑、报告和历史归档。
- 项目命令、代码地图、风险边界和本地扩展规则。

这些内容由项目维护者拥有，`collabkit sync` 永不覆盖。

## 6. 目标项目布局

```text
AGENTS.md                              受管入口，只负责核心协议和项目规则路由
.collabkit/
  project.toml                         项目拥有的配置
  project-rules.md                     项目拥有的稳定规则
  manifest.json                        生成文件、版本和哈希清单
  managed/
    protocol.md                        CollabKit 管理的当前协议
docs/
  README.md                            项目拥有的文档地图
  project-overview.md                  项目拥有的稳定总览
  current-status.md                    唯一当前快照
  decisions/                           已接受 ADR
  specs/
    active/                            当前有效设计
    archive/                           已替代或退役设计
  plans/
    active/                            已确认且当前允许执行的计划
    archive/                           已完成或取消计划
  dev-log/
    README.md                          时间索引
    YYYY-MM/                           里程碑证据
  pitfalls/
    README.md                          症状路由
  reports/
    README.md                          报告路由
    current/                           当前权威报告
    archive/                           历史口径
```

`decisions/` 和 `reports/` 对 `general` Profile 可为空，但路径保留，以保持统一路由。

## 7. Source of truth 与生命周期

### 7.1 稳定入口

- 根 `AGENTS.md` 是代理自动发现入口。
- 它是受管生成文件，只包含 CollabKit 读取协议、项目配置摘要，以及读取
  `.collabkit/project-rules.md` 的强制路由。
- 项目规则只编辑 `.collabkit/project-rules.md`，不得直接编辑生成后的 `AGENTS.md`。

### 7.2 当前事实

- `docs/current-status.md` 是唯一项目级当前快照。
- 它只保留更新时间、当前目标、当前边界、已验证状态、阻塞和统一下一步。
- 单次执行细节必须下沉到 dev-log，再从状态页链接。

### 7.3 设计与计划

- `specs/active/` 只放已确认且当前有效的设计。
- `plans/active/` 只放已确认、尚未结束且允许执行的计划。
- 被替代、完成或取消后，在同一里程碑内移入对应 archive。
- archive 永不作为默认执行入口。

### 7.4 历史与经验

- dev-log 按月和里程碑保存事实、改动、验证、产物与剩余缺口。
- pitfalls 按稳定问题域保存“症状 / 原因 / 规避 / 证据”。
- reports/current 保存当前权威口径；被替代后按日期移入 archive。
- ADR 保存难以从代码解释、影响多个模块或难以回滚的已接受决定。

## 8. 渐进读取协议

新会话默认读取顺序：

1. 根 `AGENTS.md`。
2. `.collabkit/managed/protocol.md` 和 `.collabkit/project-rules.md`。
3. 只有需要整体判断时读取 `docs/current-status.md`。
4. 按问题先读索引，再读取一份命中的 dev-log、pitfall、spec、plan 或 report。
5. 只有明确追溯历史时才进入 archive。

禁止默认整读全部日志、全部踩坑、全部设计、全部计划、全部报告或任何 archive。

## 9. 管理边界与同步安全

文件分为两类：

- **受管文件**：`AGENTS.md`、`.collabkit/managed/*`、可选 CI workflow。
- **项目拥有文件**：`project.toml`、`project-rules.md` 和 `docs/` 下所有项目事实。

`manifest.json` 记录受管文件的协议版本、模板版本和内容 SHA-256。同步规则：

1. 受管文件与清单哈希一致时，可以安全更新。
2. 受管文件发生本地修改时，`sync` 必须 fail-loud，展示冲突文件，不静默覆盖。
3. 只有显式 `--force-managed` 才允许覆盖受管文件。
4. 项目拥有文件只在不存在时初始化，后续同步永不覆盖。
5. 所有写操作先计算变更计划；`--dry-run` 输出计划但不写入。

## 10. CLI 设计

命令入口为 `collabkit`：

### 10.1 `collabkit init`

用于新项目。参数包括目标路径、项目名和 Profile。若检测到 `.collabkit/` 或冲突文件则
停止，不进行部分写入。支持 `--dry-run`。

### 10.2 `collabkit adopt`

用于已有项目。它创建缺失结构，不覆盖已有项目文档。若已有 `AGENTS.md`，原文完整复制
到 `.collabkit/project-rules.md`，并在 `.collabkit/imported/` 保存接入前快照，然后生成新的
受管入口。整个操作应尽量表现为 Git 可识别的移动和新增。

### 10.3 `collabkit check`

默认输出人类可读结果，支持 `--format json` 和 `--strict`。第一版检查：

- 配置和协议版本可解析。
- 必需入口与目录存在。
- 受管文件哈希未漂移。
- `current-status` 唯一、更新时间合法且不过度陈旧。
- active 和 archive 生命周期位置合法。
- Markdown 相对链接指向存在。
- 入口包含渐进读取和项目规则路由。
- 项目文件中不包含模板占位符或明显本机绝对路径。

错误始终返回非零；警告只在 `--strict` 下返回非零。

### 10.4 `collabkit sync`

根据当前安装版本更新受管文件和清单，不覆盖项目拥有文件。支持 `--dry-run` 与
`--force-managed`。

### 10.5 `collabkit migrate`

读取 `schema_version` 和 `protocol_version`，按显式迁移链逐版升级。第一版没有历史迁移，
但命令必须能报告“已经是当前版本”，为后续版本建立稳定契约。

## 11. 配置格式

`.collabkit/project.toml` 使用 Python 3.11 标准库可读取的 TOML：

```toml
schema_version = 1
protocol_version = "0.1.0"
project_name = "example-project"
profile = "software"
status_max_age_days = 14

[commands]
test = "pytest"
lint = "ruff check ."

[project]
summary = "项目的一句话说明"
```

`commands` 允许为空。CLI 不执行这些命令，只把它们作为项目恢复信息和 Profile 校验输入。

## 12. 实现架构

```text
src/collabkit/
  cli.py                 参数解析和退出码
  config.py              TOML 配置模型与验证
  operations.py          init/adopt/sync/migrate 事务编排
  rendering.py           内置模板和 Profile 渲染
  manifest.py            受管文件哈希与版本清单
  validation.py          检查器与结构化诊断
  migrations.py          显式逐版本迁移注册表
  templates/             core 与 Profile 模板
tests/
  unit/                   配置、渲染、清单和校验单测
  integration/            临时 Git 仓库端到端测试
examples/
  benchmark-profile/      脱敏的 benchmark 示例
```

实现使用 Python 3.11+、`tomllib`、`argparse`、`pathlib`、`json` 和 `hashlib`。运行时无第三方
依赖；测试使用 pytest。所有写操作通过先渲染到内存、验证冲突、再落盘的方式避免部分写入。

## 13. 错误处理和退出码

- `0`：操作成功或检查无错误。
- `1`：检查发现错误，或 strict 模式发现警告。
- `2`：命令参数、配置或协议版本无效。
- `3`：写入冲突、受管文件漂移或迁移前置条件不满足。

错误信息必须包含目标路径、原因和下一步动作。任何同步、接入或迁移冲突都不得静默降级。

## 14. 测试与验收

### 14.1 自动测试

- 配置解析和非法配置测试。
- 四种 Profile 的渲染测试。
- manifest 哈希与漂移检测测试。
- init/adopt 的无覆盖和事务性测试。
- sync 的正常、冲突和 force 路径测试。
- check 的链接、陈旧状态、生命周期、占位符与 JSON 输出测试。
- migrate 当前版本和未知版本测试。
- CLI 退出码测试。

### 14.2 端到端验收

1. 在临时空 Git 仓库执行 `init`，运行 `check --strict` 必须通过。
2. 在含既有 `AGENTS.md` 和文档的临时仓库执行 `adopt`，原内容必须可追溯且不丢失。
3. 人工修改受管文件后执行 `sync` 必须拒绝覆盖；`--force-managed` 才能恢复。
4. 分别生成 software、research 和 benchmark 项目，验证定制内容不同而生命周期一致。
5. 运行打包安装后的 `collabkit --help`、`init` 和 `check`，证明不是只在源码目录可用。

## 15. Git 与远端发布

- 本地仓库保留 `main` 作为稳定分支。
- 设计规格先提交到 `main`，实现放在 `agent/initial-collabkit` 分支。
- 实现通过测试后推送并创建 Draft PR，保留完整评审和 CI 证据。
- GitHub 远端默认创建为私有仓库 `project-collaboration-kit`，避免未经确认公开。
- 第一版协议和包版本均为 `0.1.0`，后续使用语义化版本。
- 第一版使用明确的 proprietary/private 占位，不自动添加开源许可证；决定公开前再选择许可证。

## 16. 第一版交付物

- 可安装的 Python 包与 `collabkit` CLI。
- 四种 Profile、核心模板和脱敏 benchmark 示例。
- 完整的 source of truth 与生命周期说明。
- 初始化、接入、检查、同步和迁移命令。
- 单元测试、端到端测试、打包 smoke 和 GitHub Actions。
- README、CHANGELOG、贡献与安全说明。
- 私有 GitHub 远端、实现分支和 Draft PR。

## 17. 后续但不进入第一版

- 自动生成 PR 或跨仓库批量升级。
- 可选 Git submodule/subtree 分发模式。
- Markdown AST 级链接与标题分析。
- 自定义第三方 Profile 插件发现。
- Web UI、状态仪表盘和远程文档服务。
