---
name: fuck-my-shit-mountain
description: 当用户要求对代码库进行全面审计，或要求跨多个质量维度输出结构化仓库健康报告时使用。本 skill 覆盖 architecture、design、security、stability、performance、testing、maintainability、release readiness、documentation、observability、configuration safety、data integrity、privacy、accessibility、supply chain、cost、AI/LLM safety、frontend state、backend API design、dependency weight、code consistency、comment coverage 等维度。
---

# Fuck My Shit Mountain — Skill 定义

## 目的

引导 AI 对软件项目执行基于证据的专业代码审计。虽然名称带有戏谑意味，但输出必须冷静、专业、可执行，避免情绪化语言。

## 审计前必须确认的输入

读取代码前，先判断用户是否已经给出全部必要输入。只对缺失项进行一次简洁追问，并等待用户回答后再开始审计。不要重复询问用户已经明确提供的信息。

必要输入：

1. **Audit modes** — 支持值：`full`, `architecture`, `security`, `stability`, `performance`, `testing`, `maintainability`, `design`, `release`, `documentation`, `observability`, `configuration`, `data-integrity`, `privacy`, `accessibility`, `supply-chain`, `cost`, `ai-safety`, `fallback`, `testing-authenticity`, `type-safety`, `frontend-state`, `backend-api`, `dependency-weight`, `code-consistency`, `comment-coverage`。
   - 如果用户选择 `full`，执行所有维度。
   - 如果用户选择多个模式，合并对应 prompt 的审计范围，并使用最具体的发现格式规则。
   - 如果缺少模式选择，设置问题中必须列出完整支持清单，并说明 `full` 是宽范围审计的推荐默认值。列表保持紧凑；每次请求模式选择时都展示选项。
2. **Report language** — 最终报告使用的语言，例如 English 或 Chinese。编程语言从仓库推断，不能替代这个回答。
3. **Output format** — 支持值：`md`, `html`, `both`, `stdout`。
   - `md` — 保存为 `audit-report-<project>-<date>.md`。
   - `html` — 保存为 `audit-report-<project>-<date>.html`。
   - `both` — 同时保存两个文件。
   - `stdout` — 只在对话中输出报告。
   - 如果请求 `md`、`html` 或 `both`，生成报告后必须写入文件。HTML 输出必须读取 `templates/audit-report.html`，复制其中**完全一致的 CSS 和 HTML 结构**，按所选模式填入必要章节和评分项，并用真实审计数据替换内容。不要留下占位变量；生成完整、自包含的 HTML。

如果用户只说“审计这个项目”而没有提供上述输入，一次性追问这三项，并附上支持的模式列表。如果用户已经说出类似“full, Chinese, html”，则直接开始，不再追问。

## 工作方式

1. 用户调用 skill，AI 只收集缺失的必要输入。
2. AI 从 `prompts/` 加载对应 prompt。若选择多个模式，则合并各自的审计范围。
3. AI 加载 `references/report-format.md`，获得共享上下文、报告模板、覆盖率、HTML 和 lint 规则。
4. AI 加载必要的 rubric：
   - `rubrics/severity.md`：严重程度标签。
   - `rubrics/confidence.md`：置信度标签。
   - `rubrics/evidence.md`：证据质量与最低证据门槛。
   - `rubrics/coverage.md`：维度覆盖置信度与报告限制。
   - `rubrics/scoring.md`：评分面板与等级锚点。
   - 当输出 full、architecture、maintainability、design、documentation、frontend-state、backend-api、type-safety、configuration、data-integrity、accessibility 或原则相关发现时，加载 `rubrics/principles.md`。
5. AI 按下方覆盖策略审计代码库。
6. 每条发现使用 `templates/issue-card.md` 记录。
7. 根据请求格式，用 `templates/audit-report.md` 或 `templates/audit-report.html` 汇总结果。
8. 如用户要求输出到文件，AI 将报告写入磁盘。
9. 对生成的 `md`、`html` 或 `both` 输出，若脚本可用，运行 `python3 <skill-dir>/scripts/report_lint.py --modes <selected-modes> <report-file>`。交付前必须修复 lint 失败。若为 `stdout`，手动执行同等检查。
10. 如用户要求修复规划，使用 `templates/remediation-plan.md`。
11. 如用户另行要求实现修复，则必须先完成审计/报告步骤，再修改代码。

## 模式与维度模型

- **Selectable modes** 是面向用户的审计入口。每个模式对应 `prompts/` 中一个 prompt 文件。
- **Full dimensions** 是 `full` 模式覆盖的章节。Full 模式覆盖所有可选聚焦维度，并将不适用于当前项目的维度标记为 Not assessed。
- 聚焦模式可能影响多个评分维度。例如 `configuration` 可能影响 Security、Stability 和 Release。
- 如果某个维度只在特定项目中适用，例如 `frontend-state`、`accessibility` 或 `ai-safety`，先检查适用性；若项目没有该表面，则用证据标记为 Not assessed。

## 资源加载

- 只加载所选模式对应的 prompt 文件。
- 生成任何报告前，先加载 `references/report-format.md`。聚焦 prompt 文件有意省略重复的设置和模板规则。
- 仅在用户要求示例，或报告形态不清楚时，才加载 `examples/` 作为校准材料。不要把示例中的发现复制进真实审计。
- 除非所选模式确实需要，不要把大型生成文件或 vendored 文件加载进上下文。

## 审计边界

默认情况下，本 skill 只做审计并输出报告。它可以创建用户要求的报告文件，但不得修改被审计项目的业务源码、测试、配置或依赖；只有当用户明确要求“实现修复”时才可以修改。

## 覆盖策略

对范围内的一方项目文件进行系统审计，而不是机械地逐字节审计整个仓库。

1. 先用 `rg --files` 等快速、项目感知的搜索方式建立文件清单。
2. 写发现前先建立项目地图：入口点、主模块、架构边界、数据流、状态所有权、持久化、数据完整性边界、隐私敏感数据、外部接口、安全边界、AI/模型表面、可观测性表面、配置来源、测试、CI、发布文件和依赖清单。
3. 一方源码、测试、脚本、CI/配置、迁移、依赖清单，以及描述行为的文档默认属于审计范围。
4. 默认排除：`.git`、依赖目录（`node_modules`、`vendor`，除非必须审计一方 vendored 代码）、构建产物（`dist`、`build`、`target`、`out`、`coverage`、`.next`、`.nuxt`）、生成文件、压缩 bundle、二进制资产、缓存目录和 lockfile；除非所选模式需要依赖或发布证据。
5. 大型仓库优先检查高风险区域：认证、输入边界、持久化、数据完整性、隐私敏感数据、AI/模型工具边界、并发、网络/文件系统访问、错误处理、可观测性缺口、构建/发布配置、供应链表面、成本驱动项和关键用户流程。
6. 按 `rubrics/coverage.md` 为每个所选维度赋予覆盖置信度：High、Medium、Low 或 Not assessed。
7. 报告中加入简短覆盖说明，通常放在 Project Map 或 Executive Summary：已检查区域、排除路径类别、运行过的重要命令，以及时间或访问限制。
8. 报告中加入 coverage matrix，每个所选维度一行：dimension、coverage confidence、inspected evidence、exclusions/limits。
9. 如某个文件或区域无法检查，明确说明。不要在覆盖部分完成时暗示“已完整覆盖”。

## 敏感信息处理

如果审计发现 secrets、tokens、private keys、`.env` 值、凭据、数据库 dump 或类似敏感材料：

- 不要在报告、终端输出、提交信息或对话中打印完整 secret。
- 标明路径、变量/键名、secret 类型和风险。用 `<redacted>` 脱敏，只有在必须区分时才展示极短前后缀。
- 一旦存在暴露可能，建议轮换。
- 将敏感文件作为风险证据处理，但不要复制其内容到生成报告中。

## Modes

| Mode | Prompt | Focus |
|------|--------|-------|
| `full` | `prompts/full-audit.md` | 全部维度 + 原则 |
| `architecture` | `prompts/architecture-audit.md` | 模块边界、依赖方向、状态所有权 |
| `security` | `prompts/security-audit.md` | 安全风险 |
| `stability` | `prompts/stability-audit.md` | 可靠性与错误处理 |
| `performance` | `prompts/performance-audit.md` | 现实瓶颈 |
| `testing` | `prompts/testing-audit.md` | 测试质量与缺口 |
| `maintainability` | `prompts/maintainability-audit.md` | 复杂度、耦合、原则 |
| `design` | `prompts/design-audit.md` | 工程原则与设计风险 |
| `release` | `prompts/release-audit.md` | 发布就绪度 |
| `documentation` | `prompts/documentation-audit.md` | 文档准确性、设置、运维/开发指南 |
| `observability` | `prompts/observability-audit.md` | 日志、指标、追踪、健康检查、告警 |
| `configuration` | `prompts/configuration-audit.md` | 配置校验、默认值、功能开关、环境隔离 |
| `data-integrity` | `prompts/data-integrity-audit.md` | 事务、幂等、迁移、不变量 |
| `privacy` | `prompts/privacy-audit.md` | PII、最小化、保留、删除、数据治理 |
| `accessibility` | `prompts/accessibility-audit.md` | 键盘、焦点、语义、响应式与 UX 状态 |
| `supply-chain` | `prompts/supply-chain-audit.md` | 来源、可复现性、CI 完整性、签名 |
| `cost` | `prompts/cost-audit.md` | 资源经济性、预算、外部 API 与 LLM 成本 |
| `ai-safety` | `prompts/ai-safety-audit.md` | Prompt injection、工具授权、RAG 泄露、evals、成本滥用 |
| `fallback` | `prompts/fallback-audit.md` | 静默 fallback、空 catch、防御性猜测 |
| `testing-authenticity` | `prompts/testing-authenticity-audit.md` | 真实信心 vs 绿色勾 |
| `type-safety` | `prompts/type-safety-audit.md` | unsafe 块、断言、边界类型 |
| `frontend-state` | `prompts/frontend-state-audit.md` | 组件大小、状态、副作用、耦合 |
| `backend-api` | `prompts/backend-api-audit.md` | API 设计、校验、数据访问模式 |
| `dependency-weight` | `prompts/dependency-weight-audit.md` | 过重依赖、构建工具链 |
| `code-consistency` | `prompts/code-consistency-audit.md` | 命名、导入、模式、风格一致性 |
| `comment-coverage` | `prompts/comment-coverage-audit.md` | 文档质量、过期注释、缺失文档 |

## 评分

Full 审计输出包含 7 个维度分数（0.0–10.0）和整体分数的 **score dashboard**。聚焦审计只评分相关维度；模板需要上下文时，无关维度标记为 not assessed。

- **分数越高越好。** 10 = clean / production-ready，0 = shit mountain / unacceptable。不要反向解释。
- 分数基于判断，不是机械扣分。AI 按证据对每个维度做整体评估。
- 每个分数必须有一句说明，引用最强证据；如覆盖置信度限制结论，也要写明。
- 字母等级（S/A/B/C/D/F）用于快速感知健康度。
- 分数补充详细发现，不能替代详细发现。

## 规则（不可妥协）

1. 每条发现必须包含具体证据（文件、函数、行为）。
2. 区分 **Confirmed** 问题和 **Suspected** 问题。
3. 不夸大严重性，按 `rubrics/severity.md` 映射。
4. 除非局部修复明显不够，否则不要建议重写。
5. 优先选择能降低真实风险的最小可行修复。
6. 不输出泛泛建议。每条发现必须关联现实失败场景。
7. 除非风格问题会造成可维护性或正确性风险，否则不要纠结风格。
8. 证据不足就直说，不编造发现。
9. 每条发现必须包含回归测试建议。
10. 每条发现必须包含工作量估计。
11. 使用 `rubrics/principles.md` 检查工程原则违背；关注会造成真实风险的问题，而不是轻微风格争议。
12. **必须系统化。** 检查所有范围内的一方区域，不只看明显热点。遵循覆盖策略并诚实记录排除项。
13. **不要当 yes-man。** 不要因为用户显得自信，或为了显得顺从而压低问题。你的任务是客观识别真实风险，不管代码是谁写的、用户预期是什么。代码有问题就说。
14. **使用 skill 自身模板格式，而不是被审计项目的风格。** 报告必须遵循 `templates/audit-report.md`（HTML 输出则遵循 `templates/audit-report.html`）。HTML 输出必须复制模板中**完全一致的 CSS 和 HTML 结构**（stat cards、selected-dimension score rows、top risks table、detailed findings、各维度 sections、tables + checklists、适用时的 design principles、fix order tables、quick wins grid）。只替换内容，保留全部 HTML classes、CSS variables 和章节顺序。不要发明新章节结构。不要复制被审计项目 markdown 文件里的格式、结构或风格。
15. 不暴露 secrets。敏感发现按上文脱敏报告。
16. 除非用户明确要求实现修复，否则不要修改被审计代码。

## 最终自检

交付报告前确认：

- 必要输入已知，报告使用了用户要求的语言和输出格式。
- 已遵循所选 prompt、必要 rubric 和报告模板。
- 报告包含 project map、coverage note、coverage matrix、score dashboard、finding statistics、top risks、detailed findings、相关维度章节、适用时的 principles compliance、fix order 和 quick wins。
- 每个所选维度都有 coverage confidence、inspected evidence 和 exclusions/limits。
- 每条发现都有 severity、confidence、status、evidence、realistic failure scenario、minimal fix、regression test suggestion 和 estimated effort。
- Confirmed 与 Suspected 已分开或清楚标注。
- 评分方向正确：10.0 最好，0.0 最差。
- HTML 报告完整、自包含，且无占位变量。
- 输出中没有完整 secrets、tokens、private keys、passwords 或敏感 dump。
- 对生成文件输出，`scripts/report_lint.py` 通过；对 stdout 输出，已手动执行同等检查。
