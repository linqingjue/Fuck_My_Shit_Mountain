# Backend API Audit Prompt（后端 API 审计提示词）

使用 fuck-my-shit-mountain skill 的 **backend-api mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 聚焦范围

检查 API 契约、校验、认证授权、数据访问、错误响应和业务边界是否可靠。只报告会导致真实安全、正确性、性能或维护风险的问题。

## 审计区域

- ApiConsistency：路由、方法、状态码、响应结构是否一致。
- Validation：输入校验、边界转换、schema、错误提示是否明确。
- Auth：认证授权是否覆盖所有受保护资源。
- NplusOne：数据访问是否有 N+1、重复查询、缺分页。
- Caching：缓存键、失效、权限边界是否正确。
- ErrorResponse：错误是否稳定、可诊断、不泄露内部信息。
- BusinessLogic：业务规则是否在正确边界执行，而不是散落在 handler。
- DataFlow：请求到持久化/外部系统的数据流是否清楚。

## 规则

1. 每条 API 发现必须说明具体 endpoint 或调用路径。
2. 不要只讨论风格；必须连接到安全、正确性、性能或演进风险。
3. 修复建议优先包括 schema 校验、统一错误契约、授权中间件、分页/缓存边界、服务层收口。
4. 每条发现都要包含 API 测试、契约测试或集成测试建议。
