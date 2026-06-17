# Supply Chain and Reproducibility Audit Prompt（供应链与可复现性审计提示词）

使用 fuck-my-shit-mountain skill 的 **supply-chain mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

## 聚焦范围

检查依赖、构建、CI、制品和发布链路是否可信、可复现、可追溯。只报告有现实攻击面或发布风险的问题。

## 审计区域

- DependencyProvenance：依赖来源是否可信，是否锁定版本和 registry。
- Reproducibility：构建是否可复现，工具链是否固定。
- CIIntegrity：workflow 权限、secret 暴露、PR 执行策略是否安全。
- ArtifactProvenance：制品是否有校验、签名、SBOM、来源记录。
- RegistryHygiene：包内容、发布权限、维护者、命名是否安全。

## 规则

1. 每条发现必须说明攻击路径或发布破坏路径。
2. 不要报告没有证据的理论供应链风险。
3. 修复建议优先包括 lock、pin、权限最小化、可复现构建、签名和制品校验。
4. 每条发现都要包含验证方式，例如 clean build、CI 权限检查、制品校验或依赖来源核对。
