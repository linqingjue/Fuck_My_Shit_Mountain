# Full Audit Prompt（完整审计提示词）

使用 fuck-my-shit-mountain skill 的 **full mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 审计范围

Full 模式覆盖所有维度：Architecture、Security、Stability、Performance、Testing、Maintainability、Design、Release、Documentation、Observability、Configuration、Data Integrity、Privacy、Accessibility、Supply Chain、Cost、AI Safety、Fallback、Testing Authenticity、Type Safety、Frontend State、Backend API、Dependency Weight、Code Consistency、Comment Coverage。

## 工作要求

1. 先建立项目地图，再写发现。
2. 每个维度都要给出覆盖置信度：High / Medium / Low / Not assessed。
3. 不适用于当前项目的维度不要硬编问题，标记为 Not assessed，并说明证据。
4. 每条发现都必须有文件、函数/模块、相关行为、现实失败场景、最小修复、回归测试建议和工作量估计。
5. 区分 Confirmed 与 Suspected；证据不足就写 Suspected 或不写。
6. 避免泛泛建议，所有建议必须能追溯到代码、配置、测试、文档或依赖证据。
7. 使用 `rubrics/scoring.md` 生成评分面板；分数越高越好。
8. 使用 `rubrics/principles.md` 输出原则合规分析。

## 输出重点

- 执行摘要：说明整体健康度、最高风险和修复优先级。
- 覆盖矩阵：每个维度一行，诚实说明未检查项。
- 详细发现：按严重程度和修复优先级组织。
- 维度章节：每个维度都要有覆盖说明、发现表或无发现说明。
- 修复顺序：区分立即修复、稳定发布前修复、后续排期和暂时忽略。
- Quick Wins：只列低成本且确实能降低真实风险的事项。
