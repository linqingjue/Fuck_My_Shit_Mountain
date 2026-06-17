# Scoring Rubric（评分标准）

## 原则

评分是**基于判断的，不是基于公式的**。AI 根据收集到的证据对每个维度做整体评估，然后给出分数。不要使用机械扣分，例如“Critical = -2.0”；这种做法会制造虚高分数和虚假精确感。

## 分值范围

每个维度按 **0.0 – 10.0** 评分，**分数越高越好**。

| Score | Meaning |
|-------|---------|
| **10.0** | **Perfect.** 干净，接近生产可用，未发现问题。 |
| **0.0** | **Maximum shit mountain.** 完全不可维护，风险不可接受。 |
| 分数反映的是**工程质量和可维护性**，不是代码风格偏好。 |

**关键规则：不要反向解释。10 是最好分数，0 是最差分数。永远是 Higher = better。**

## 评分维度

| Dimension | What It Measures | Mapped From Modes |
|-----------|-----------------|-------------------|
| Security | 抗攻击能力：认证、注入、secret、依赖风险、隐私边界、供应链风险、AI/工具滥用。 | security, type-safety, configuration, privacy, supply-chain, ai-safety |
| Stability | 故障下的可靠性：panic 路径、错误处理、重试、超时、状态一致性、fallback 质量、数据正确性、可运维性。 | stability, fallback, observability, configuration, data-integrity, privacy, ai-safety |
| Performance | 现实负载和成本下的效率：热路径、内存、I/O、竞争、外部 API/模型支出。 | performance, dependency-weight, cost, ai-safety |
| Testing | 测试提供的真实信心：覆盖质量、测试类型、测试真实性、可访问性/AI 安全 eval。 | testing, testing-authenticity, accessibility, ai-safety |
| Maintainability | 变更难度：架构、复杂度、耦合、重复、命名、文档、状态管理、API 设计。 | architecture, maintainability, documentation, frontend-state, backend-api, code-consistency, comment-coverage, accessibility |
| Design | 工程原则遵循程度：SRP、DRY、KISS、fail-fast、类型安全、边界设计。 | architecture, design, type-safety, accessibility |
| Release | 发布就绪度：CI/CD、版本管理、升级、回滚、依赖重量、配置、可观测性、迁移安全、供应链、成本。 | release, dependency-weight, observability, configuration, data-integrity, documentation, supply-chain, cost, privacy |

## 评分锚点

以下描述是**参考**，不是机械规则。最终分数仍由审计者基于证据判断。

### 9.0 – 10.0（Excellent / Clean）

- 该维度状态很好，或只有轻微问题。
- 发现的问题彼此隔离、严重程度低、容易修复。
- 没有结构性债务，没有系统性风险。
- **一句话模式：**“整体扎实，只有少量轻微问题，不阻塞发布。”

### 7.0 – 8.9（Good / Needs attention）

- 存在真实问题，但范围可控。
- 特定区域存在中等风险，但没有系统性失败。
- 修复需要局部改动，而不是重写。
- **一句话模式：**“有一些真实问题，但范围可控，建议下次发布前处理。”

### 5.0 – 6.9（Fair / Significant risk）

- 该维度存在系统性问题。
- 有多个 Medium 或更高严重程度发现。
- 需要有计划地投入，而不是只做 quick fix。
- **一句话模式：**“存在系统性问题，需要有计划地投入，不是打补丁能解决。”

### 3.0 – 4.9（Poor / Critical debt）

- 该维度有严重失败。
- High 级别问题不是孤立点，而是形成模式。
- 修复需要结构性调整。
- **一句话模式：**“存在结构性失败，需要实质性重做，不是点状修复。”

### 0.0 – 2.9（Shit Mountain / Unacceptable）

- 该维度接近灾难状态。
- Critical 问题普遍存在。
- 该维度的基本方法就是错的。
- **一句话模式：**“基本坏掉，需要重做。”

## Overall Score

默认对 7 个维度分数取平均，并四舍五入到 1 位小数。

对于聚焦审计模式（例如仅 security），只报告相关维度分数，并说明其他维度未评估。

## Grade Map

**Higher = better.** S 最好，F 最差。

| Score | Grade | Label | Meaning |
|-------|-------|-------|---------|
| 9.0 – 10.0 | S | Excellent | 接近生产可用，只有轻微挑剔项。 |
| 7.0 – 8.9 | A | Good | 整体扎实，有问题但紧急度较低。 |
| 5.0 – 6.9 | B | Fair | 需要处理，存在 Medium 风险。 |
| 3.0 – 4.9 | C | Poor | 风险明显，发布前应处理。 |
| 1.0 – 2.9 | D | Bad | 存在 Critical 问题，不应原样发布。 |
| 0.0 – 0.9 | F | Shit Mountain | 高严重度问题普遍存在，需要重大返工。 |

## 分数可视化

每个维度分数渲染为 10 字符 ASCII 条。**分数越高，填充越多，表示越好。**

```text
Score bar: ████████░░
8.0 = ████████░░  （8 个填充，2 个空）
3.5 = ███░░░░░░░  （3.5 个填充，6.5 个空）
```

填充块：`█`（使用 `\u2588`）  
空块：`░`（使用 `\u2591`）

填充块数量 = floor(score)。小数部分 ≥ 0.5 时向上补 1 格。

### 示例评分面板

```text
Security        ████████░░  8.0  A   WS 缺少认证，配置中存在硬编码 secret
Stability       ██████░░░░  6.0  B   热路径有 3 处 unwrap，数据库失败没有重试
Performance     ██████████  10.0 S   未发现问题
Testing         ████░░░░░░  4.0  C   集成测试有价值，但单元测试薄弱
Maintainability ███████░░░  7.0  A   3 个文件超过 800 行，2 个模块违反 SRP
Design          █████░░░░░  5.0  B   DRY 多处违反，API 边界缺少 fail-fast
Release         ██████░░░░  6.0  B   Windows 无 CI，缺少回滚计划
────────────────────────────────────
Overall         ██████░░░░  6.6  B
```

## 规则

1. **每个分数必须在评分面板中有一句话理由。** 理由总结最强证据；如果 coverage confidence 限制结论，也要说明。
2. **不要平均 finding 严重程度。** 如果只有 1 个 Critical 问题，需要判断它是系统性的还是孤立且易修复的。
3. **考虑强度和密度。** 同一文件中 10 个低严重度问题，可能比分散且易修的 1 个 Critical 问题更应该拉低分数。
4. **考虑上下文。** CLI 工具中的 600 行文件和安全关键库中的 600 行文件含义不同。要按项目类型和规模调整。
5. **如果某维度零发现，只有 coverage 为 High 时才可给 10.0（Excellent）。** 如果 coverage 为 Medium 或 Low，必须解释限制，避免把该维度描述成完全干净。
6. **不要为了好看而四舍五入改变等级。** 5.9 就写 5.9，不要写 6.0；如果确实是 6.0，才写 6.0。
7. **Executive Summary 必须包含 score dashboard**，每个维度附一句理由。
8. **Focused audit modes**（例如 security-only）：只评分相关维度。其他维度写明 not assessed。
9. **重要方向：**10.0 = 最好（clean），0.0 = 最差（shit mountain）。不要反向解释。
10. **Coverage interaction：** 使用 `rubrics/coverage.md`。Not assessed 维度排除在 overall score 之外；Low coverage 不应支撑高置信度的“干净”结论。
