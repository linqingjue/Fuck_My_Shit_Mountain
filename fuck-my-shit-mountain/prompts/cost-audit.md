# Cost and Resource Economics Audit Prompt（成本与资源经济性审计提示词）

使用 fuck-my-shit-mountain skill 的 **cost mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 聚焦范围

检查资源使用、外部 API、LLM、日志指标和基础设施是否存在可被规模放大的成本风险。只报告有现实成本驱动证据的问题。

## 审计区域

- UnboundedWork：无上限循环、批处理、并发、队列、缓存、文件或查询。
- ExternalApiCost：第三方 API 调用是否有缓存、限额、重试预算和降级策略。
- LLMCost：token、模型、并发、工具调用是否有预算和限制。
- InfrastructureSizing：资源限制、自动扩缩、存储增长是否合理。
- ObservabilityCost：日志、指标、trace 的采样、保留和基数是否受控。
- CostVisibility：是否能按租户、功能、任务或工作流看见成本。

## 规则

1. 每条成本发现必须说明成本驱动项和放大条件。
2. 不要把“可能贵”当作问题；要结合调用频率、输入规模或资源增长路径。
3. 修复建议优先包括上限、缓存、去重、采样、预算、限流、指标和告警。
4. 每条发现都要包含验证方式，例如压力测试、账单指标、token 统计或资源曲线。
