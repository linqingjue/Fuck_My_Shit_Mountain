# Coverage Confidence Rubric（覆盖置信度标准）

覆盖置信度描述某个已选审计维度被检查得有多完整。它与 finding confidence 分开：即使某个维度整体覆盖为 Medium，单条发现仍然可以是 High confidence。

## 等级

### High

- 已用项目感知搜索建立文件清单。
- 已检查与所选维度相关的入口点、关键流程、边界文件、配置、测试、发布/依赖制品。
- 重要的生成/vendor/build 路径已被有意排除并记录原因。
- 在可行时运行了相关静态检查、测试命令或定向搜索。
- 所选维度没有跳过重要区域。

### Medium

- 已检查主流程和代表性文件。
- 一些次要路径、环境特定配置、生成制品或仅运行时行为未能完整检查。
- 审计仍有足够证据识别可能的系统性风险。
- 如果没有发现问题，这个结论只限于已检查范围。

### Low

- 审计主要依赖抽样、元数据、命名或窄范围搜索。
- 关键运行时行为、部署配置、数据存储或外部接口无法检查。
- 已报告发现仍可能有效，但“未发现问题”的证明力很弱。
- 评分应保守，并必须说明覆盖限制。

### Not Assessed

- 该维度不在所选模式内、不适用于项目，或无法访问。
- 不要为该维度评分。
- 如果报告模板需要该行，标记为 `not assessed` 并说明原因。

## 报告必填字段

每份报告必须包含 coverage matrix：

| Dimension | Coverage | Evidence inspected | Exclusions / limits |
|-----------|----------|--------------------|---------------------|

每个维度章节必须包含：

- Coverage: High / Medium / Low / Not assessed
- Inspected evidence: 已检查的文件、命令、模式或运行时表面
- Exclusions / limits: 未检查内容及原因

## 与评分的关系

- 零发现维度只有在 coverage 为 High 时才能给 10.0。
- Medium coverage 仍可支持较好分数，但评分理由必须说明限制。
- Low coverage 不应产生高置信度的“干净”结论。
- Not assessed 维度排除在 overall score 之外。
- 不要用 coverage confidence 淡化已确认发现；它只限定完整性。
