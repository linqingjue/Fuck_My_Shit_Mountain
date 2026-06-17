# Privacy and Data Governance Audit Prompt（隐私与数据治理审计提示词）

使用 fuck-my-shit-mountain skill 的 **privacy mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 聚焦范围

检查项目是否正确识别、最小化、保护、保留、删除和导出个人数据或敏感数据。只报告有证据支撑的数据治理风险。

## 审计区域

- DataInventory：是否能识别 PII、敏感数据、派生数据和日志数据。
- Minimization：是否收集或存储了不必要的数据。
- AccessBoundary：访问控制、审计日志、内部工具权限是否过宽。
- Retention：是否有保留期限，以及是否真正执行。
- Deletion：删除是否覆盖缓存、索引、备份、派生数据和外部系统。
- Export：导出是否完整、边界清晰、不会泄露他人数据。
- TelemetryPrivacy：日志、指标、trace、错误上报是否脱敏。

## 规则

1. 每条隐私发现必须说明涉及的数据类别和流转位置。
2. 不要臆测法规适用性；如涉及合规，只描述工程风险和需要法务确认的点。
3. 不要打印完整敏感数据，只引用路径、字段名和脱敏证据。
4. 每条发现都要包含具体缓解措施和验证方式。
