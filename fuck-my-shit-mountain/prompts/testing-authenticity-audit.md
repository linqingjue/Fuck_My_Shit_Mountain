# Testing Authenticity Audit Prompt（测试真实性审计提示词）

使用 fuck-my-shit-mountain skill 的 **testing-authenticity mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 聚焦范围

判断测试是否真的能防止回归，而不是制造“看起来有测试”的虚假安全感。

## 审计区域

- Valuable Tests：真正覆盖关键行为、边界、错误路径和集成边界的测试。
- Suspicious Tests：过度 mock、只测实现细节、只测 happy path、断言无意义、依赖执行顺序的测试。
- Missing Tests：关键风险路径没有测试覆盖。
- False Confidence：覆盖率高但关键行为未被验证。

## 规则

1. 每条发现必须说明现有测试为什么不能防住某类真实 bug。
2. 不要嘲笑测试；客观说明它提供了什么信心、缺了什么信心。
3. 对每个可疑测试给出 Keep / Rewrite / Delete 建议。
4. 每条发现都要包含具体替代测试建议。
