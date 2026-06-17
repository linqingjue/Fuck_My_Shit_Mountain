# Fallback Audit Prompt（Fallback 审计提示词）

使用 fuck-my-shit-mountain skill 的 **fallback mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 聚焦范围

检查 fallback、兼容分支、空 catch、防御性猜测和静默修正是否掩盖真实故障。不是所有 fallback 都有问题；只报告会隐藏错误、产生错误结果或阻碍诊断的路径。

## 审计区域

- SilentFallback：失败后静默切到默认值、旧逻辑、空结果或弱策略。
- EmptyCatch：捕获异常但不记录、不抛出、不恢复。
- CompatibilityBranch：兼容逻辑长期存在且改变真实行为。
- SilentCorrection：自动修正输入/状态但没有记录和验证。
- DefensiveGuess：证据不足时猜测业务含义、格式或权限。

## 规则

1. 每条发现必须说明 fallback 掩盖了什么真实故障。
2. 区分 KeepWithAlert、FailFast、Remove 三种处理建议。
3. 不要机械删除 fallback；如果 fallback 必须存在，应建议告警、指标、日志和测试。
4. 每条发现都要包含触发条件和回归测试。
