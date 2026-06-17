# Design Principles Audit Prompt（设计原则审计提示词）

使用 fuck-my-shit-mountain skill 的 **design mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 聚焦范围

检查代码是否违反会造成真实工程风险的设计原则。不要为了套原则而套原则；只有当原则违背会带来变更风险、缺陷风险、测试困难或边界混乱时才报告。

## 审计区域

- Single Responsibility：职责是否混杂，导致修改一个需求牵连无关逻辑？
- Open/Closed：新增能力是否只能通过修改核心分支完成？
- Dependency Inversion：高层策略是否依赖底层实现细节？
- Fail Fast：非法输入、错误配置、缺失依赖是否被延迟暴露？
- Explicit Boundaries：模块、API、配置、状态边界是否清楚？
- Least Privilege：权限、能力、作用域是否过宽？
- DRY：重复是否会导致行为分叉，而不只是表面重复？

## 规则

1. 每条发现都必须关联 `rubrics/principles.md` 中的原则。
2. 只报告会产生现实风险的原则违背。
3. 不要把轻微风格争议包装成设计问题。
4. 修复建议应优先选择局部边界调整、职责拆分、显式契约、fail-fast 校验等小步方案。
