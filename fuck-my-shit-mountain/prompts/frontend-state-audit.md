# Frontend State Audit Prompt（前端状态审计提示词）

使用 fuck-my-shit-mountain skill 的 **frontend-state mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 聚焦范围

检查前端状态是否清晰、单一、可预测，并且不会造成 UI 与真实业务状态不一致。若项目没有前端界面，应标记 Not assessed。

## 审计区域

- ComponentSize：组件是否过大，混合渲染、数据获取、业务规则和副作用。
- StateDuplication：同一状态是否在多个位置重复保存。
- PropDrilling：状态传递是否让边界和所有权变得模糊。
- EffectChain：effect 是否形成隐式执行链，导致竞态或难以测试。
- UIBusinessCoupling：业务规则是否散落在 UI 组件中。
- DOMasState：是否把 DOM 当成真实状态来源。
- RequestState：加载、错误、取消、陈旧响应是否处理正确。
- RenderPerf：大列表、频繁渲染、昂贵计算是否缺少边界。

## 规则

1. 每条发现必须说明具体用户流程或状态不一致场景。
2. 不要因为用了某个状态库或没用某个状态库而报问题；关注状态所有权和行为。
3. 修复建议优先包括单一 source of truth、状态提升/下沉、请求状态机、取消与去重。
4. 每条发现都要包含组件测试、交互测试或状态机测试建议。
