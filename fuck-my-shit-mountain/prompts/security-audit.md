# Security Audit Prompt（安全审计提示词）

使用 fuck-my-shit-mountain skill 的 **security mode**。

共享设置、覆盖策略、报告模板、HTML 和 lint 规则位于 `references/report-format.md`；生成报告前必须加载该引用。

只关注与安全相关的真实风险。

## 审计区域（参考原则 4.6：Least Privilege）

### 认证与授权
- 应用如何识别用户？
- 如何判断每个用户能做什么？
- 用户是否能访问不该访问的资源？
- 是否存在硬编码凭据、token 或 API key？
- session 如何管理？token 生命周期是否清楚？

### 输入与输出
- 注入向量：命令、SQL、NoSQL、LDAP、模板注入等。
- 文件操作中的路径穿越。
- URL 拉取、代理功能中的 SSRF。
- CORS 配置。
- CSRF 防护。
- 不可信数据序列化/反序列化。

### Secrets 与配置
- 代码、测试、配置中的硬编码 secret。
- 环境变量中的不安全默认值。
- 日志、错误消息或 debug 输出中的 secret。
- 提交到版本库的敏感配置文件。
- 不安全默认配置。

### 网络与传输
- TLS 配置。
- WebSocket 认证与 origin 检查。
- API 暴露范围：内部 / 外部。
- 速率限制和暴力破解防护。

### 依赖
- 已知漏洞依赖。
- 供应链风险：install script、postinstall hook、构建期代码执行。
- 扩大攻击面的不必要依赖。
- dependency confusion：公开包名遮蔽内部包名。

### 运维安全
- 敏感文件权限。
- 密码、token、PII 等敏感数据日志。
- 遗留 debug endpoint。
- 泄露内部状态的错误处理。
- 响应头或错误页中的信息泄露。

## 规则

1. 每条安全发现都必须包含攻击前提和攻击路径。
2. 不报告没有现实利用路径的纯理论漏洞。
3. 区分 Confirmed 与 Suspected。
4. 每个问题都要包含具体缓解措施和回归测试。

## 态度

1. **必须系统化。** 检查范围内所有 endpoint、认证路径、输入边界和依赖证据。遵循 skill 的覆盖策略，并诚实记录排除项。
2. **不要当 yes-man。** 客观报告安全问题。不要因为项目“只是内部系统”或“没人会攻击”而降低风险判断。

## Finding 格式补充

安全类发现除通用字段外，还要写清：

- Attack precondition: <攻击前提>
- Attack path: <攻击路径>
- Impact: <影响>
- Mitigation: <缓解措施>

## 不要浪费输出在

- 只在本地使用的应用却强行讨论 HTTPS enforcement。
- 没有浏览器 UI 的应用却强行讨论 CSP header。
- 没有证据的理论供应链攻击。
- 明确公开的 endpoint 缺少 auth。
