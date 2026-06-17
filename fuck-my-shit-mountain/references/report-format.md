# 共享报告格式规则

当 prompt 要求使用共享设置/报告规则时，加载本文件。

## 审计前的必要上下文

读取代码前，先确认 audit mode(s)、report language 和 output format 是否已知。若有缺失，只用一条简洁消息询问缺失项，并等待用户回答。若用户或调用方已经提供这些信息，直接继续，不要重复追问。

## 报告模板约束

报告必须遵循 skill 自身模板：

- 发现项使用 `templates/issue-card.md`。
- Markdown 报告使用 `templates/audit-report.md`。
- HTML 报告使用 `templates/audit-report.html`。
- 不要复制被审计项目内部 markdown 文件的格式、标题风格或结构。
- 被审计项目自己的 README、docs 或注释只能作为证据，不能作为报告模板。

## HTML 输出规则

HTML 输出必须满足：

- 读取 `templates/audit-report.html`。
- 生成完整、自包含的 HTML。
- 精确保留模板中的 CSS、section 结构、class 和排序。
- 只包含与所选模式相关的评分项和维度章节；`full` 例外，它覆盖所有维度，并将不适用维度标记为 Not assessed。
- 每个维度章节必须包含覆盖说明、发现表或“无发现”卡片，以及已验证清单。
- 侧边栏导航必须包含每个已生成章节的链接。
- 不要留下占位变量或示例数据。

## 覆盖率规则

每份报告必须包含：

- coverage matrix，每个所选维度一行。
- 每个维度的覆盖程度：High / Medium / Low / Not assessed。
- 已检查证据：文件、命令、搜索、运行时表面或已检查模式。
- 排除项/限制：没有检查什么，以及为什么没有检查。

使用 `rubrics/coverage.md` 赋予覆盖置信度。

## Lint 规则

对于生成到文件的报告，运行：

```bash
python3 <skill-dir>/scripts/report_lint.py --modes <selected-modes> <report-file>
```

交付前修复所有 lint 失败。对于 `stdout`，手动执行同等检查：

- 没有未替换的占位符。
- 必要章节存在。
- 所选维度章节存在。
- Markdown finding 字段完整。
- 严重程度统计与详细发现一致。
- 没有未脱敏的 secrets 或 private keys。
