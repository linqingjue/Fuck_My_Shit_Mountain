# Configuration Audit Prompt（配置审计提示词）

使用 fuck-my-shit-mountain skill 的 **configuration mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 聚焦范围

检查配置是否可验证、可解释、可隔离，并且不会因为默认值、环境差异或 secret 处理不当造成事故。

## 审计区域

- Schema validation：配置是否在启动时校验类型、范围、必填项和组合约束。
- Unsafe default：默认值是否会在生产环境造成安全或稳定性风险。
- Environment separation：开发、测试、预发、生产是否隔离清楚。
- Secret config：secret 是否被硬编码、打印、提交或以不安全默认值存在。
- Feature flag：开关是否有 owner、过期时间、测试和审计记录。
- Config docs：配置说明、示例、迁移说明是否与实际代码一致。

## 规则

1. 每条配置发现必须说明错误配置如何变成真实故障。
2. 不要因为“没有复杂配置系统”而报问题；关注项目实际风险。
3. 优先建议启动期 fail-fast、显式 schema、环境隔离、secret store、示例同步。
4. 每条发现都要包含验证方式，例如错误配置测试、启动失败测试或环境矩阵检查。
