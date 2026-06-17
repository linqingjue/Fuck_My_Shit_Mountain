# Type Safety Audit Prompt（类型安全审计提示词）

使用 fuck-my-shit-mountain skill 的 **type-safety mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 聚焦范围

检查类型系统是否被绕过，或边界类型是否不足以表达真实约束。只报告会造成运行时错误、数据错误或维护风险的类型问题。

## 审计区域

- UnsafeBlock：unsafe、裸指针、手动内存、跨 FFI 边界是否被隔离和证明安全。
- TypeAssertion：强制类型断言、cast、any/unknown 滥用是否掩盖错误。
- InputBoundary：外部输入是否从不可信类型转为可信类型前经过校验。
- OutputLeak：内部类型是否泄露到 API 或外部契约。
- BooleanTrap：布尔参数是否造成调用歧义。
- StringlyTyped：关键概念是否用裸 string/number 表示，导致误传。
- ErrorType：错误类型是否可区分、可处理、可测试。

## 规则

1. 每条类型安全发现必须说明类型系统没能阻止的具体错误。
2. 不要把所有 any/cast 都视为问题；只有跨边界、关键逻辑或重复出现才报告。
3. 修复建议优先使用边界解析、窄化类型、枚举/联合类型、newtype、显式错误类型。
4. 每条发现都要包含能证明类型保护有效的测试或编译检查建议。
