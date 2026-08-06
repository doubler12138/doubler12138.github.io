# 主题 01：Swift 基础进阶（iOS）

| 项目 | 内容 |
|------|------|
| 对应单元 | A1 Swift 语言进阶 |
| 状态 | 进行中 |
| 启动日期 | 2026-08-03 |
| 里程碑 | 掌握闭包捕获/协议/泛型/some-any 四大核心，产出一份总结笔记 + 一个协议泛型综合 demo |

**主题内知识点流（层层延续）**：闭包与捕获语义 → 协议与协议导向编程 → 泛型与关联类型 → some/any 与类型擦除 → 综合动手

## 学习节次（自定节奏，逐节推进）

| 节次 | 建议档位 | 知识点（沿主题层层推进） | 推荐资料（带链接，可替换） | 状态 |
|------|----------|--------------------------|---------------------------|------|
| S1 | 工作日档(30-45min) | 闭包与捕获语义：捕获列表、逃逸闭包、@autoclosure | 见下方「S1 资料详解」 | [x] |
| S2 | 工作日档(30-45min) | 协议与协议导向编程：协议扩展、条件一致性、协议组合 | [SwiftGG 协议章](https://doc.swiftgg.team/documentation/the-swift-programming-language/protocols/) | [ ] |
| S3 | 工作日档(30-45min) | 泛型与关联类型：泛型约束、where 子句、PAT 问题 | [SwiftGG 泛型章](https://doc.swiftgg.team/documentation/the-swift-programming-language/generics/) | [ ] |
| S4 | 工作日档(30-45min) | some/any 与类型擦除：不透明类型、存在类型、AnyEraser | [SwiftGG 不透明类型章](https://doc.swiftgg.team/documentation/the-swift-programming-language/opaquetypes/) + [SwiftGG 泛型章](https://doc.swiftgg.team/documentation/the-swift-programming-language/generics/) | [ ] |
| S5 | 大块档(2-3h) | 综合动手：用协议+泛型实现一个类型安全的小型框架 | 自建 demo（选题见下） + [Swift 进阶](https://objccn.io/products/advanced-swift/) 回顾 | [ ] |

> 📌 延续性说明：闭包是 Swift 函数式能力的根基 → 协议是 Swift 抽象的核心手段 → 泛型让协议具备类型安全 → some/any 解决"协议作为类型"时的表达力问题 → 最后综合动手把四者串成完整能力闭环。

## 本主题里程碑（明确产出）

- [ ] 一份笔记：「Swift 类型系统与抽象机制」一页总结（闭包捕获/协议/泛型/some-any 各一段话 + 一个代码片段）
- [ ] 一个可运行 demo（S5 三选一）：① 类型安全的事件总线 ② 轻量网络请求抽象层 ③ 协议导向的 Repository 模式
- [ ] 达成后更新 progress.md 对应单元状态/掌握度

## 资料主线（本主题固定，可自由替换）

- [The Swift Programming Language 中文版（SwiftGG）](https://doc.swiftgg.team/documentation/the-swift-programming-language/) —— 语言规范级讲解，本主题主资料
- [Swift 进阶（objc.io Advanced Swift 中文版）](https://objccn.io/products/advanced-swift/) —— 进阶补充，讲"为什么这样设计"
- [Stanford CS193p 2025（B站中字）](https://www.bilibili.com/video/BV123SyB4EoE/) —— 视频辅助，Swift 语言部分可挑看

---

## S1 资料详解 —— 闭包与捕获语义（工作日档 / 30-45 分钟）

### 方案一（推荐）：[The Swift Programming Language 中文版 ·「闭包」](https://doc.swiftgg.team/documentation/the-swift-programming-language/closures/)

按以下小节顺序阅读，每个小节对应一个知识子点：

- **「闭包表达式」** —— 闭包语法回顾、尾随闭包写法、从上下文推断类型。快速过一遍即可（5 min），确认语法无障碍。
- **「逃逸闭包」** —— 重点。理解 @escaping 的含义、为什么异步回调必须逃逸、逃逸闭包中 self 的显式引用要求。这是面试高频点（10 min）。
- **「自动闭包」** —— @autoclosure 的惰性求值特性、适用场景（如 assert、日志）。了解即可（5 min）。
- **「捕获值」** —— 本节最核心。闭包如何捕获外部变量（引用捕获）、捕获列表 [weak self] / [unowned self] / 值捕获的语义（5-10 min）。

### 方案二（补充加深）：[Swift 进阶（Advanced Swift）](https://objccn.io/products/advanced-swift/) · Functions / Closures 相关章节

- 如果已购此书，阅读 Closures 部分，重点看"值语义下闭包捕获的行为"与"闭包作为一等公民的设计哲学"。

### 验证理解（边读边写，5-10 min）

在 Playground 中写出以下 3 段代码：
1. 一个 class 持有闭包属性，分别用强引用和 `[weak self]` 捕获，观察 deinit 是否调用（理解循环引用）
2. 一个 @escaping 场景（如 DispatchQueue.main.asyncAfter），体会为什么编译器要求显式写 self
3. 用 @autoclosure 实现一个 `log(level:_ message:)` 函数，验证参数只在需要时才求值

### S1 产出

- Playground 中 3 段验证代码（保留，后续可回顾）
- 一句话总结写在笔记里：「闭包捕获的是变量的引用而非值；逃逸闭包 + 引用类型 self 是循环引用的温床，[weak self] 是标准解法」
- 为 S2 打前置：闭包是"行为抽象"，下一节的协议是"接口抽象"，思考两者的互补关系

### S1 实际产出（2026-08-06 完成，耗时 90min）

- 笔记：[学习笔记——Swift闭包与捕获语义](../source/_posts/学习笔记/学习笔记-Swift闭包与捕获语义.md)（线上：https://doubler12138.github.io/2026/08/03/学习笔记/学习笔记-Swift闭包与捕获语义/）
- 3 段验证代码已完成并记录「预期输出 / 实际观察」：强引用 vs `[weak self]` 的 deinit 对比、`asyncAfter` 逃逸场景、`@autoclosure` 版 `log(level:_:)`
- 已勾选 Checklist 4 项：闭包语法与尾随闭包 / 闭包捕获语义 / @escaping / @autoclosure
- 待补（🟡）：「ARC 在 Swift 中的行为、循环引用排查」——笔记仅覆盖闭包场景，缺类实例互持与 Memory Graph / Instruments 排查手段，留到 A2 单元补上
- 笔记里留下的开放问题：`[unowned self]` 的真实适用场景、非逃逸上下文栈分配是保证还是优化（想从 SIL 验证）、Swift 6 下 `Sendable` 闭包捕获检查与本节语义的叠加
