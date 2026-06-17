# AI and LLM Safety Audit Prompt（AI 与 LLM 安全审计提示词）

使用 fuck-my-shit-mountain skill 的 **ai-safety mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 聚焦范围

检查项目中与 LLM、Agent、RAG、工具调用、模型输出和自动化决策相关的安全边界。只报告有实际攻击路径、泄露路径或成本滥用路径的问题。

## 审计区域

- PromptInjection：不可信内容是否能改变系统指令、工具调用或安全策略。
- ToolAuthorization：模型是否能绕过确定性权限检查直接调用工具。
- RAGLeakage：检索是否按用户权限过滤，是否可能泄露跨租户/私有数据。
- ModelFallback：fallback 是否显式、有边界、可观测，是否静默改变安全等级。
- OutputValidation：结构化输出、代码、SQL、URL、文件路径是否经过校验。
- EvalGap：是否缺少安全、策略、越权、注入、回归 eval。
- AbuseCost：token、模型、并发、工具调用是否有预算和限流。

## 规则

1. 每条 AI 安全发现都必须说明不可信输入如何跨越边界。
2. 不要把“用了 AI”本身当作风险；必须有具体数据流或工具边界证据。
3. 权限必须由确定性代码执行，不能只靠 prompt 约束。
4. 每条发现都要包含缓解措施和 eval/测试建议。
