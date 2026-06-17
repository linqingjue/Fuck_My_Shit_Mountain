# Evidence Rubric（证据标准）

## 什么算证据

| Type | Example | Weight |
|------|---------|--------|
| Direct code observation | 第 42 行对用户可控值调用 `unwrap()` | Strong |
| Traceable control flow | 请求路径 A → 函数 B → 分支 C 没有错误处理 | Strong |
| Runtime behavior | 输入为空时测试失败 | Strong |
| Config inspection | `app.config.secret = "hardcoded-dev-key"` | Strong |
| Dependency audit | 依赖 v1.2.3 存在 CVE-2024-XXXX 且有公开 exploit | Strong |
| Comment indicating risk | `// TODO: this can deadlock under load` | Strong |
| Pattern inference | 所有 handler 都使用 `String::from_utf8_unchecked` | Medium |
| Structural inference | 文件 3000 行且包含 20 个 public function | Medium |
| Missing pattern | `auth.rs` 没有对应测试文件 | Weak |
| Name-based inference | 函数名 `doStuff()` 暗示职责不清 | Weak |

## 按严重程度的最低证据要求

| Severity | Minimum Evidence |
|----------|-----------------|
| Critical | Direct code observation 或 runtime behavior confirmation |
| High | Direct code observation 或 traceable control flow |
| Medium | Direct code observation 或 pattern inference |
| Low | Pattern inference 或 structural inference |
| Info | 任意证据 |

## 按置信度的最低证据要求

| Confidence | Minimum Evidence |
|------------|-----------------|
| High | Direct code observation 或 traceable control flow |
| Medium | Pattern inference 或 config inspection |
| Low | Structural inference 或 missing patterns |

## 什么不算证据

- 只有“看起来不好”，但没有解释。
- “不够 idiomatic”——单纯风格不是证据。
- “可能很慢”，但没有指出瓶颈。
- “不可扩展”，但没有指出扩展上限。
- 对命名、格式或结构的个人偏好。
- 抱怨代码不符合审查者偏好的范式。
- 对开发者意图的假设。

## 证据格式

每个 evidence block 必须包含：

```text
- File: <带行号的路径>
- Function / Module: <具体函数或模块名>
- Relevant behavior: <代码实际做了什么>
```

建议补充：

```text
- Input / state that triggers the behavior:
- Expected vs actual behavior:
- Test that demonstrates the issue:
```
