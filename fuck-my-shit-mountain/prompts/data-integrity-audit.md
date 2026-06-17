# Data Integrity Audit Prompt（数据完整性审计提示词）

使用 fuck-my-shit-mountain skill 的 **data-integrity mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 聚焦范围

检查系统是否能保护关键数据不丢失、不重复、不冲突、不违反业务不变量。只报告会造成真实数据错误的风险。

## 审计区域

- TransactionBoundary：多步写入是否有事务或原子性保障。
- Idempotency：重试、回调、消息消费、支付/订单/任务是否能防重复。
- ConcurrencyConsistency：并发写是否有锁、版本检查、CAS 或唯一约束。
- MigrationSafety：迁移是否可重复、可回滚、可恢复、可观测。
- InvariantValidation：业务不变量是否在边界和持久化层被校验。
- BackupRestore：备份是否真的能恢复，恢复路径是否测试过。
- Reconciliation：异步系统、缓存、索引、外部系统是否有对账/修复机制。

## 规则

1. 每条发现必须说明会破坏哪类数据不变量。
2. 不要只说“应该加事务”；要指出具体写入序列和失败点。
3. 对幂等、并发、迁移问题必须给出触发条件。
4. 每条发现都要包含验证方式，例如并发测试、重复请求测试、迁移重跑测试或恢复演练。
