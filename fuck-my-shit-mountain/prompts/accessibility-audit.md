# Accessibility and UX Correctness Audit Prompt（可访问性与 UX 正确性审计提示词）

使用 fuck-my-shit-mountain skill 的 **accessibility mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 聚焦范围

检查界面是否能被键盘、屏幕阅读器、不同尺寸设备和异常状态下的用户正确使用。只报告会影响真实可用性或正确性的风险。

## 审计区域

- SemanticStructure：按钮、表单、导航、标题层级是否语义正确。
- KeyboardFocus：键盘可达性、焦点顺序、焦点陷阱、快捷键冲突。
- ResponsiveVisual：响应式布局、触控目标、对比度、溢出和遮挡。
- ErrorState：错误是否与输入关联，文案是否可理解。
- LoadingState：加载、禁用、重复提交、陈旧数据是否处理正确。
- UXStateCorrectness：UI 状态是否与服务端状态一致。

## 规则

1. 每条发现必须说明具体用户流程和受影响用户。
2. 不要只列 WCAG 名词；必须联系代码或 UI 行为。
3. 对无浏览器 UI 的项目，应标记 Not assessed，而不是硬编问题。
4. 每条发现给出可执行修复和验证方式，例如键盘路径、屏幕阅读器检查、响应式断点或组件测试。
