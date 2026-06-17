> **给 AI 的指令：这是唯一有效的报告模板。不要使用被审计项目内部文件的格式、标题风格或结构。输出必须遵循本模板。**
>
> 为兼容 `scripts/report_lint.py`，关键英文标题和字段名会保留；正文、解释、结论和修复建议应使用用户要求的报告语言。

# Fuck My Shit Mountain 审计报告

**Project:** <项目名称>  
**Audit mode:** <full / architecture / security / stability / performance / testing / maintainability / design / release / documentation / observability / configuration / data-integrity / privacy / accessibility / supply-chain / cost / ai-safety / fallback / testing-authenticity / type-safety / frontend-state / backend-api / dependency-weight / code-consistency / comment-coverage>  
**Date:** <日期>  
**Reviewer:** <AI 模型 / 版本>

---

## 1. Executive Summary（执行摘要）

<用 2-3 段总结代码库状态：总体健康度、最关键风险、审计覆盖限制、优先修复方向。>

### Score Dashboard（评分面板）

```text
Security        ████████░░  8.0  A   <一句话说明>
Stability       ██████░░░░  6.0  B   <一句话说明>
Performance     ██████████  10.0 S   <一句话说明>
Testing         ████░░░░░░  4.0  C   <一句话说明>
Maintainability ███████░░░  7.0  A   <一句话说明>
Design          █████░░░░░  5.0  B   <一句话说明>
Release         ██████░░░░  6.0  B   <一句话说明>
─────────────────────────────────────
Overall         ██████░░░░  6.6  B
```

每个维度 0.0–10.0 分。**分数越高越好（10 = 干净 / 接近生产可用，0 = shit mountain / 不可接受）**。分数基于证据判断，不是公式扣分。评分锚点见 `rubrics/scoring.md`。

### Finding Statistics（发现统计）

| Severity | Count | Confirmed | Suspected |
|----------|-------|-----------|-----------|
| Critical | <N> | <N> | <N> |
| High | <N> | <N> | <N> |
| Medium | <N> | <N> | <N> |
| Low | <N> | <N> | <N> |
| Info | <N> | <N> | <N> |
| **Total** | **<N>** | **<N>** | **<N>** |

## 2. Project Map（项目地图）

<说明项目结构：关键组件、入口点、数据流、状态所有权、持久化、外部接口、安全边界。标出最可能产生风险的区域。>

### Coverage Matrix（覆盖矩阵）

| Dimension | Coverage | Evidence inspected | Exclusions / limits |
|-----------|----------|--------------------|---------------------|
| <dimension> | High / Medium / Low / Not assessed | <文件、命令、搜索、运行时表面> | <未检查内容及原因> |

## 3. Top Risks（最高优先级风险）

<按优先级列出 5-15 条发现。每条包含标题、Severity 和一句话摘要；完整细节放在 Detailed Findings。>

## 4. Detailed Findings（详细发现）

<所有发现必须使用 `templates/issue-card.md` 的字段结构。字段名保持英文，字段内容用报告语言填写。>

### Finding: <简短标题>

- Severity: Critical / High / Medium / Low / Info
- Confidence: High / Medium / Low
- Category: <维度>
- Status: Confirmed / Suspected
- Affected area: <模块、组件或子系统>
- Evidence:
  - File: <path:line-range>
  - Function / Module: <名称>
  - Relevant behavior: <代码实际行为>
- Problem: <问题描述>
- Why it matters: <工程影响>
- Realistic failure scenario: <现实失败场景>
- Minimal fix: <最小修复>
- Better long-term fix: <长期改进>
- Regression test suggestion: <回归测试建议>
- Estimated effort: <工作量>

## 5. Dimension Sections（维度分析章节）

根据所选模式生成相关章节。每个维度章节必须以以下三行开头：

- Coverage: High / Medium / Low / Not assessed
- Inspected evidence: <文件、命令、搜索、运行时表面>
- Exclusions / limits: <未检查内容及原因>

建议章节标题保留英文维度名并附中文，例如：

- `## Architecture Concerns（架构问题）`
- `## Security Concerns（安全问题）`
- `## Stability Concerns（稳定性问题）`
- `## Performance Concerns（性能问题）`
- `## Testing Gaps（测试缺口）`
- `## Maintainability Concerns（可维护性问题）`
- `## Design / Principles Concerns（设计 / 原则问题）`
- `## Release Concerns（发布问题）`
- `## Documentation Analysis（文档分析）`
- `## Observability / Operability Analysis（可观测性 / 可运维性分析）`
- `## Configuration Safety Analysis（配置安全分析）`
- `## Data Integrity Analysis（数据完整性分析）`
- `## Privacy / Data Governance Analysis（隐私 / 数据治理分析）`
- `## Accessibility / UX Correctness Analysis（可访问性 / UX 正确性分析）`
- `## Supply Chain / Reproducibility Analysis（供应链 / 可复现性分析）`
- `## Cost / Resource Economics Analysis（成本 / 资源经济性分析）`
- `## AI / LLM Safety Analysis（AI / LLM 安全分析）`
- `## Fallback / Defensive Code Analysis（Fallback / 防御性代码分析）`
- `## Testing Authenticity Analysis（测试真实性分析）`
- `## Type Safety Analysis（类型安全分析）`
- `## Frontend State Analysis（前端状态分析）`
- `## Backend API Analysis（后端 API 分析）`
- `## Dependency Weight Analysis（依赖重量分析）`
- `## Code Consistency Analysis（代码一致性分析）`
- `## Comment Coverage Analysis（注释覆盖分析）`

每个章节应包含：相关发现表、无发现说明或已验证清单。不要把“未发现问题”写成“绝对没有问题”，应绑定覆盖证据。

---

## <N+1>. Principles Compliance（工程原则合规性）

<总结代码库对 `rubrics/principles.md` 中工程原则的遵循程度。仅在 full、architecture、maintainability、design 等相关模式中生成。>

### Principles Violated（违反的原则）

| Principle | Violations | Severity | Affected Areas |
|-----------|------------|----------|----------------|
| Single Responsibility (SRP) | <N> | Medium | <modules> |
| File Size Limit | <N> | Low | <modules> |
| Fail-Fast | <N> | High | <modules> |
| ... | ... | ... | ... |

### Principles Respected（做得好的原则）

<列出代码库做得好的地方，并说明证据。>

---

## <N+2>. Recommended Fix Order（推荐修复顺序）

### Fix Immediately（立即修复）

<可能导致数据丢失、安全泄露、服务中断的问题。>

### Fix Before Stable Release（稳定发布前修复）

<会降低可靠性、正确性或安全性的问题。>

### Schedule Later（后续排期）

<增加维护成本或限制扩展的问题。>

### Ignore for Now（暂时忽略）

<低严重性、理论风险或纯风格偏好。>

## <N+3>. Quick Wins（速赢项）

<低成本、高价值修复，通常是 1-2 小时内能消除真实风险的改动。>

## <N+4>. Long-term Refactor Plan（长期重构计划）

<仅当证据支持结构性改进时包含。每项应包含：动机、方案、风险和测试策略。>
