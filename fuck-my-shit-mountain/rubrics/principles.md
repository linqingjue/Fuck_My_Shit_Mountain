# Software Engineering Principles Rubric（软件工程原则标准）

本文件是审计时用于对照的软件工程原则目录。每条原则说明要观察什么、如何识别违反、以及默认严重程度如何映射。原则不是用来制造“教条式批评”的；只有当违反原则会造成真实工程风险时才报告。

---

## 1. Structure & Size Discipline（结构与规模纪律）

### 1.1 Single Responsibility（SRP）
> 模块 / 类 / 函数应当只有一个变化原因。

- **Detection:** 统计一个文件/函数中的不同职责。如果一个函数同时解析输入、验证权限、查询数据库、格式化响应，就违反 SRP。
- **Threshold:** 单个函数 >1 个清晰职责；单个类/模块 >3 个职责。
- **Severity:** Medium —— 增加耦合，隐藏依赖，使测试更困难。
- **Example:** 400 行 `handleRequest` 同时处理认证、路由、业务逻辑、序列化和日志。

### 1.2 File Size Limit（文件大小限制）
> 源文件不应超过合理规模。

- **Detection:** 统计代码行数（排除空行和 import）。
- **Threshold:** >500 行标记；>1000 行通常为 High。
- **Severity:** Low（>500）/ Medium（>1000）—— 大文件容易隐藏多个职责并降低可导航性。
- **Exception:** 生成代码、数据表、枚举定义。

### 1.3 Function/Method Size（函数/方法大小）
> 函数应尽量能在一屏内读完。

- **Detection:** 统计函数体行数。
- **Threshold:** >50 行标记；>100 行通常为 High。
- **Severity:** Low（>50）/ Medium（>100）—— 长函数隐藏分支复杂度，使测试困难。

### 1.4 Parameter Count（参数数量）
> 函数参数应尽量少。

- **Detection:** 统计形式参数。
- **Threshold:** >4 个参数标记；>7 个通常为 High。
- **Severity:** Low（>4）/ Medium（>7）—— 通常说明缺少参数对象或抽象。
- **Exception:** 构造函数依赖注入。

### 1.5 Nesting Depth（嵌套深度）
> 深层嵌套通常意味着复杂度过高。

- **Detection:** 测量函数内最大缩进层级。
- **Threshold:** >4 层标记；>7 层通常为 High。
- **Severity:** Medium（>4）—— 深层嵌套难读、难测、难维护。

### 1.6 Cyclomatic Complexity（圈复杂度）
> 函数中的独立路径过多会提高缺陷概率。

- **Detection:** 统计分支：if、else、case、循环、catch、逻辑运算符等。
- **Threshold:** >10 标记；>20 通常为 High。
- **Severity:** Medium（>10）—— 需要更多测试用例，bug 概率更高。

---

## 2. Coupling & Cohesion（耦合与内聚）

### 2.1 Low Coupling（低耦合）
> 模块应依赖较少模块，并尽量依赖稳定模块。

- **Detection:** 统计引用项目内部模块的 import/require/include。
- **Threshold:** >10 个内部 import 标记。
- **Severity:** Medium —— 高耦合使系统僵硬，修改一个模块容易破坏多个模块。
- **Form:** `utils/helpers` 被 30+ 文件导入。

### 2.2 High Cohesion（高内聚）
> 模块内部元素应在功能上相关。

- **Detection:** 模块包含无关功能，例如字符串工具 + 网络调用 + 配置解析。
- **Threshold:** 模块名无法描述其全部内容。
- **Severity:** Medium —— 低内聚会分散相关逻辑，使查找和修改困难。

### 2.3 Law of Demeter（最少知识原则）
> 一个单元不应了解其依赖对象的内部结构。

- **Detection:** 链式属性/方法访问，例如 `a.b.c.d()` 或 `getX().getY().doZ()`。
- **Threshold:** 接收者之后 >2 个点（排除 fluent API 和 builder）。
- **Severity:** Low —— 增加对中间类型的耦合，内部重构时易破坏。

### 2.4 Dependency Inversion（依赖倒置，DIP）
> 依赖抽象，而不是依赖具体实现。

- **Detection:** 高层模块直接实例化或引用低层具体实现。
- **Threshold:** 业务逻辑直接使用数据库驱动、文件系统或网络库。
- **Severity:** Medium —— 难以替换实现，也更难测试。

### 2.5 Interface Segregation（接口隔离，ISP）
> 客户端不应依赖自己不用的方法。

- **Detection:** 结构体/类实现接口方法，但方法中 `NotImplemented`、`panic!` 或 `return null`。
- **Threshold:** >1 个方法为空实现或直接抛出。
- **Severity:** Low —— 表明接口过胖；若接口稳定，风险较低。

### 2.6 Stable Dependencies Principle（稳定依赖原则）
> 依赖方向应指向更稳定的模块。

- **Detection:** 一个经常变化的不稳定模块被很多其他模块依赖。
- **Threshold:** 模块有 >5 个依赖方，且最近一个月 >10 次修改。
- **Severity:** High —— 修改该模块会破坏大量下游消费者。

---

## 3. Naming & Abstraction（命名与抽象）

### 3.1 Principle of Least Surprise（最小惊讶原则）
> 代码行为应符合其名称和签名暗示。

- **Detection:** `getX()` 修改状态；`save()` 同时发邮件；`isValid()` 有副作用。
- **Threshold:** 任何明确违反。
- **Severity:** High —— 调用者按常规语义理解时会产生隐蔽 bug。

### 3.2 Command-Query Separation（命令查询分离，CQS）
> 函数要么是命令（修改状态，返回 void），要么是查询（返回数据，无副作用），不要混合。

- **Detection:** 函数既修改状态又返回值（`pop()` 等通用模式除外）。
- **Threshold:** 任何明确违反。
- **Severity:** Medium —— 增加认知负担，使状态推理更困难。

### 3.3 Tell, Don't Ask
> 告诉对象做什么，而不是取出对象数据后在外部操作。

- **Detection:** 调用 getter 取出对象数据，然后在外部条件判断或计算。
- **Threshold:** 同类模式在 >3 处重复出现。
- **Severity:** Low —— 表明过程式风格，增加对数据结构的耦合。

### 3.4 Meaningful Names（有意义的命名）
> 名称应揭示意图，避免误导性缩写。

- **Detection:** 单字母名称（循环计数除外）、晦涩缩写（`d`, `tmp`, `data`, `val`, `handleStuff`）、误导性名称（变量叫 `fileName` 实际存 URL）。
- **Threshold:** 明显误导性命名直接标记；公共 API 中无信息量命名也标记。
- **Severity:** Low（私有）/ Medium（公共 API）—— 误导性命名会导致 bug，无信息命名会降低速度。

### 3.5 Boolean Trap（布尔陷阱）
> 布尔参数会制造难读的调用点。

- **Detection:** `bool` 参数控制行为分支。
- **Threshold:** 不是简单优化/缓存开关的任何 `bool` 参数。
- **Severity:** Low —— `process(true, false)` 不可读；优先使用 enum 或拆成两个函数。
- **Fix:** 替换为 enum 或拆分函数。

---

## 4. Code Quality（代码质量）

### 4.1 DRY（Don't Repeat Yourself）
> 每份知识应有单一、明确的表达。

- **Detection:** 超过 3 行的相同或近似代码块出现在多处。
- **Threshold:** 相同逻辑 >2 处。
- **Severity:** Medium —— 重复意味着 bug 可能只在一处修复，其他地方遗漏。
- **Important:** 区分偶然重复与本质重复。只报告会造成行为分叉的重复。

### 4.2 YAGNI（You Ain't Gonna Need It）
> 只有确实需要时才添加功能。

- **Detection:** 未使用参数、dead code、只有一个实现的抽象层、为不存在功能准备的通用配置。
- **Threshold:** 已编译/发布但从未使用的代码。
- **Severity:** Low —— 增加认知负担；除非造成混乱，否则优先级较低。

### 4.3 KISS（Keep It Simple, Stupid）
> 简单方案优于复杂方案。

- **Detection:** 不必要设计模式（一个实现却用 Factory，两个类型却用 Visitor）、过度抽象（一行逻辑包成类层级）、简单行为使用复杂配置 DSL。
- **Threshold:** 存在明显更简单替代方案，且可减少 >30% 代码。
- **Severity:** Medium —— 过度工程增加维护负担却没有收益。

### 4.4 Fail-Fast
> 尽早、清楚地失败。

- **Detection:** 函数接受非法输入并继续向深层传递；公共 API 边界缺少 null/empty/validity 检查；静默 fallback 掩盖错误。
- **Threshold:** 非法输入传播 >3 层后才被发现。
- **Severity:** High —— 延迟失败产生混乱错误信息，增加调试难度。

### 4.5 Defensive Programming — Appropriate Level（适度防御式编程）
> 要验证输入，但不要对内部一致性过度防御。

- **Detection:** 每个调用点都断言内部不变量（说明代码互不信任）；或外部输入缺少验证。
- **Threshold:** 外部输入缺少验证标记为 High；冗余内部检查标记为 Low。
- **Severity:** 取决于上下文：缺少外部验证 → High；过度内部检查 → Low。

### 4.6 Principle of Least Privilege（最小权限原则）
> 代码只应拥有它需要的权限。

- **Detection:** 所有模块都能访问全局可变状态；函数接收超过所需的数据；API 暴露过宽（`pub` everything、`export *`）。
- **Threshold:** 任何不被外部消费者需要的公共 API 表面。
- **Severity:** Medium —— 增加安全表面和耦合。

---

## 5. State & Side Effects（状态与副作用）

### 5.1 Immutability Preference（优先不可变）
> 优先使用不可变数据结构。

- **Detection:** 可变 global/static 状态；函数修改参数（除非修改就是函数目的）。
- **Threshold:** 任何未明确同步的共享可变状态。
- **Severity:** High —— 共享可变状态是并发 bug 的主要来源。

### 5.2 Pure Functions Where Possible（尽可能使用纯函数）
> 无副作用函数更容易测试和推理。

- **Detection:** 函数读写文件、网络、数据库、全局状态或随机源，却没有清楚文档。
- **Threshold:** 看起来像纯函数但实际不纯。
- **Severity:** Medium —— 增加测试难度和隐藏耦合。

### 5.3 No Hidden Side Effects（无隐藏副作用）
> 副作用必须能从函数签名或命名中看出来。

- **Detection:** 函数执行 I/O、修改全局状态或抛错，但名称、类型签名或文档没有提示。
- **Threshold:** 任何隐藏副作用。
- **Severity:** High —— 调用者不读实现就无法推理行为。

### 5.4 No Shared Mutable State Without Synchronization（共享可变状态必须同步）
> 共享可变状态必须被保护。

- **Detection:** 全局/static 变量、闭包/回调捕获可变变量、跨线程/goroutine 共享引用但没有 mutex/atomic/channel。
- **Threshold:** 任何未同步共享可变状态。
- **Severity:** Critical —— race condition 是最难调试的 bug 之一。

---

## 6. Error Handling（错误处理）

### 6.1 Don't Swallow Errors（不要吞错）
> 错误必须被处理或传播，不能静默忽略。

- **Detection:** 空 catch；`// ignore error` 注释；忽略返回错误的函数返回值；对会返回错误的表达式做 `void` cast。
- **Threshold:** 任何吞错。
- **Severity:** High —— 静默失败会造成数据损坏、不一致和难诊断 bug。

### 6.2 Don't Lose Error Context（不要丢失错误上下文）
> 错误应携带足够上下文，便于诊断和修复。

- **Detection:** 使用泛型错误（`Box<dyn Error>`、`Exception`、字符串错误）但不包装上下文；捕获后原样抛出；只记录错误消息，不记录触发值。
- **Threshold:** 任何丢失根因或上下文的错误。
- **Severity:** Medium —— 增加调试时间和生产事故处理时间。

### 6.3 Handle All Branches（处理所有分支）
> 条件分支、match arm 或 switch case 都必须明确处理。

- **Detection:** default/else 分支为空、只记录 “unexpected” 后继续、无解释地 panic；允许不完整 pattern match 的语言中存在遗漏。
- **Threshold:** match/switch/if-else 链中任何未处理分支。
- **Severity:** Medium —— 未处理分支是常见逻辑 bug 来源。

### 6.4 Structured Error Types（结构化错误类型）
> 公共 API 应使用自定义错误类型，而不是泛型错误。

- **Detection:** 公共 API 返回 `Result<_, String>`、`Either<_, Exception>` 或类似泛型错误。
- **Threshold:** 公共 API 返回泛型错误类型。
- **Severity:** Low —— 调用者无法匹配具体错误，增加错误处理耦合。

---

## 7. Architecture（架构）

### 7.1 Dependency Rule（分层架构依赖规则）
> 源码依赖必须指向内层：外层依赖内层，内层不能依赖外层。

- **Detection:** UI/transport 层直接导入数据库驱动；业务逻辑导入 HTTP 库；持久化层导入 view/UI 类型。
- **Threshold:** 任何内层依赖外层的情况。
- **Severity:** High —— 制造循环依赖，使层无法独立测试。

### 7.2 Business Logic Independence（业务逻辑独立性）
> 业务逻辑应独立于框架、数据库和 UI。

- **Detection:** 业务逻辑文件导入框架特定类型；SQL 嵌入业务逻辑；UI 渲染逻辑与数据处理混合。
- **Threshold:** 任何这类混合。
- **Severity:** Medium —— 把软件核心价值耦合到基础设施选择上，增加变更成本。

### 7.3 Explicit Dependencies Over Implicit/Global（显式依赖优于隐式/全局依赖）
> 依赖应被显式传入或声明，而不是从全局状态获取。

- **Detection:** Singleton、static/service locator、全局变量、ambient context、thread-local storage 用于解析依赖。
- **Threshold:** 任何使用全局状态解析依赖的情况。
- **Severity:** Medium —— 依赖在类型系统中不可见，隐藏耦合，增加测试难度。

### 7.4 Composition Over Inheritance（组合优于继承）
> 优先组合小行为单元，而不是深继承层级。

- **Detection:** 继承链 >2 层（排除语言级基类）；子类重写父类大多数方法。
- **Threshold:** 继承 >2 层；子类重写 >50% 父类方法。
- **Severity:** Low —— 深继承会导致父类变化产生涟漪效应。

### 7.5 Open for Extension, Closed for Modification（开闭原则，OCP）
> 模块应对扩展开放，对修改关闭。

- **Detection:** 新增功能必须修改既有代码，而不是添加新代码；使用 flag/switch 行为分支代替多态/策略。
- **Threshold:** 功能新增总是修改既有文件而不是新增实现。
- **Severity:** Medium —— 说明设计僵硬，每个新功能都有破坏既有行为的风险。

---

## 8. Testing（测试）

### 8.1 Test Behavior, Not Implementation（测试行为，不测试实现）
> 测试应验证可观察行为，而不是内部实现细节。

- **Detection:** 测试断言某方法被调用，而不是断言结果；测试访问私有成员；不改变行为的重构会导致测试失败。
- **Threshold:** 每个测试文件中 >2 个实现细节断言测试。
- **Severity:** Medium —— 脆弱测试降低重构信心。

### 8.2 Arrange-Act-Assert（AAA 模式）
> 测试应组织成清晰的准备、执行、断言阶段。

- **Detection:** setup 与 assertion 交错；没有清晰 act 步骤；act 前就 assertion。
- **Threshold:** 读者无法轻易识别三个阶段。
- **Severity:** Low —— 可读性问题，增加测试维护成本。

### 8.3 One Logical Assertion Per Test（每个测试验证一个逻辑行为）
> 每个测试应验证一个行为。

- **Detection:** 一个测试中有多个不相关断言，覆盖多个独立场景。
- **Threshold:** 一个测试中 >3 个关于不同结果的断言。
- **Severity:** Low —— 第一个失败会掩盖后续问题，诊断更困难。

### 8.4 Don't Mock What You Don't Own（不要 mock 不属于你的东西）
> mock 外部边界，而不是第三方库内部。

- **Detection:** mock 标准库类型、框架内部类型、第三方库类型（除非该库本身是抽象边界）。
- **Threshold:** 任何对项目不拥有类型的 mock。
- **Severity:** Medium —— 对外部库内部的 mock 会强耦合库实现，库升级时测试破裂。

### 8.5 Test One Failure Mode at a Time（一次只测一个失败模式）
> 每个测试应验证一个具体失败场景。

- **Detection:** 一个测试合并多个失败条件；测试成功路径却命名成错误测试。
- **Threshold:** 任何同时测试多个失败模式的测试。
- **Severity:** Low —— 难以定位到底哪个失败场景触发了 bug。

---

## 9. Configuration & Environment（配置与环境）

### 9.1 Configuration Over Hardcoding（配置优于硬编码）
> 部署间会变化的值应放在配置中，而不是代码中。

- **Detection:** 源码中硬编码 URL、端口、timeout、feature flag、credentials。
- **Threshold:** 任何随部署变化的值被硬编码。
- **Severity:** High —— 无法不改代码部署到不同环境。

### 9.2 Fail on Missing Configuration（缺失配置应失败）
> 必需配置缺失时应启动失败，而不是运行时才失败。

- **Detection:** 生产环境静默启用 fallback 默认值；使用 `||` 连接开发默认值；必需配置解析失败后返回 null。
- **Threshold:** 任何必需配置存在静默默认值。
- **Severity:** Critical —— 会造成生产误配置且不被发现。

### 9.3 Environment Separation（环境隔离）
> 开发与生产环境应只通过配置值不同，而不是代码路径不同。

- **Detection:** 业务逻辑中出现 `if (isDev) { ... }` 或类似环境判断；开发专用路径混入生产逻辑。
- **Threshold:** 生产代码中任何环境判断。
- **Severity:** Medium —— 未测试的 dev-only 路径可能引入生产 bug。

---

## 10. Concurrency & Resource Management（并发与资源管理）

### 10.1 No Blocking Calls in Async Context（异步上下文不要阻塞调用）
> 异步代码中的阻塞 I/O 会破坏 async 的意义。

- **Detection:** async 函数中出现 `std::thread::sleep()`、`time.sleep()`、同步 I/O。
- **Threshold:** 任何出现。
- **Severity:** High —— 造成线程池饥饿和意外延迟。

### 10.2 Unbounded Resources Must Not Grow Forever（无界资源不能无限增长）
> 集合、队列、缓存和 buffer 必须有大小限制。

- **Detection:** 无界 `Vec`/`List`/channel 作为累积 buffer；缓存没有淘汰策略；内存中的事件/请求日志随每次请求增长。
- **Threshold:** 正常运行下会无界增长的任何集合。
- **Severity:** High —— 中等负载下可能 OOM 并重启服务。

### 10.3 Cancel Safety（取消安全）
> 异步操作必须能被安全取消。

- **Detection:** 获取资源后 await，再释放；持有 mutex 跨 await；取消时发生部分写入。
- **Threshold:** 任何 mutex 跨 await；任何资源获取后取消时没有 cleanup。
- **Severity:** High —— 取消可能让状态损坏并泄露资源。

### 10.4 Timeout Every External Call（外部调用必须有超时）
> 每次调用外部系统都必须有 timeout。

- **Detection:** HTTP client 无 timeout；数据库查询无 statement timeout；网络文件系统上的文件操作无 timeout。
- **Threshold:** 任何外部调用没有显式 timeout。
- **Severity:** High —— 慢外部依赖可能无限挂住整个服务。

---

## 使用本 Rubric 的方法

1. **审计过程中：** 对照这些原则检查代码。
2. **发现违反：** 映射到相应维度（Maintainability、Security、Stability 等）。
3. **严重程度映射：** 先使用每条原则列出的默认严重程度，再根据上下文上下调整：出现次数、是否核心路径、造成真实失败的概率。
4. **证据要求：** 每条违反都要引用被违反的原则，并说明为什么违反，同时附具体代码证据。
5. **不要报告每个轻微违反** —— 只关注会造成真实工程风险的问题。
6. **示例格式：**

```text
### Finding: SRP violation in UserService — 3 responsibilities in one class

- Severity: Medium
- Confidence: High
- Category: Maintainability
- Status: Confirmed
- Principle violated: Single Responsibility (1.1)
- Evidence:
  - File: src/services/user.rs:1-250
  - Relevant behavior: UserService 同时处理数据库查询、邮件发送和权限检查。
- Why it matters: 每个职责有不同变化原因和不同测试要求。
- ...
```
