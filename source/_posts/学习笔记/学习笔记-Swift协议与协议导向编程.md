---
title: 学习笔记——Swift协议与协议导向编程
date: 2026-08-06T21:30:00
author: DoubleR
img: /images/学习笔记-Swift协议与协议导向编程/cover.jpg
top: false
hide: false
cover: true
toc: true
mathjax: false
summary: 主题 01「Swift 基础进阶」S2 节次学习笔记，梳理协议的定义与遵循、协议扩展与默认实现、条件一致性 extension where、协议组合与协议继承，并从「继承 vs 组合」的角度理解协议导向编程的取舍。
categories: 学习笔记
tags:
  - 学习笔记
  - Swift
  - 协议
  - 协议扩展
  - 协议导向编程
---

<!--
写作提示（发布前可整体删除本注释）
- 本文是 S2 节次的交付物，进度追踪 Skill 会按「独立小节 + 原理解释/代码示例/推导 + 小结归纳」判定是否勾选 Checklist
- 因此每个 h1 下至少要有：一段原理性解释 + 一段可运行代码
- 标注「能手写」的条目（本节的「协议组合与类型擦除」含 AnyXxx Eraser）必须给出完整可运行代码，否则只算「部分覆盖」
- 上一节（S1 闭包）的写法可作参照：先讲清「为什么这样设计」，再用 Playground 代码验证，最后用一句话归纳
-->

## 本节定位

| 项目 | 内容 |
|------|------|
| 所属单元 | A1 Swift 语言进阶 |
| 所属节次 | 主题 01 · S2（工作日档 30-45min） |
| 主资料 | [The Swift Programming Language 中文版 ·「协议」](https://doc.swiftgg.team/documentation/the-swift-programming-language/protocols/) |
| 本节产出 | Playground 三段验证代码 + 本篇笔记 |

**对应 Checklist 条目**（覆盖后即可勾选）：

- 协议基础：定义、遵循、协议扩展与默认实现
- 条件一致性：extension where 子句
- 协议组合（`some P1 & P2`）与类型擦除（能手写 AnyXxx Eraser）

<!--
说明：「协议组合与类型擦除」这一条横跨 S2 与 S4——本节只覆盖协议组合（some P1 & P2）与协议继承，
类型擦除（AnyXxx Eraser 手写）留到 S4「some/any 与类型擦除」。若本节不写 Eraser 代码，
进度追踪会判 🟡 待补，属预期行为，不必强行在本节补齐。
「关联类型与 Self 约束、PAT」属 S3，本节只在最后一节做前置铺垫，不作为勾选目标。
-->

<!-- 承接上一节：闭包是「行为抽象」——把一段行为当值传递；协议是「接口抽象」——约定类型能做什么。这一句可作为开篇 -->

# 协议基础：定义与遵循
<!-- 目标：确认语法无障碍，5 min 快速过。原理点在于「协议描述的是能力契约，而非实现」 -->

## 属性要求与方法要求
<!-- get/set 的语义、为什么协议里的属性要用 var 声明；mutating 方法要求对值类型的意义 -->

## 协议作为类型使用
<!-- 存在类型（any P）的容器场景，先给结论即可，性能与 some/any 的区别留到 S4 -->

## 遵循协议的两种时机：声明处 vs extension
<!-- 用 extension 让已有类型（含系统类型）追认协议，这是 POP 的入口 -->

# 协议扩展与默认实现
<!-- 本节重点，约 10 min。核心原理：默认实现让协议从「纯契约」变成「可复用行为的载体」 -->

## 默认实现：给契约配一份兜底行为
<!-- 用一个可运行例子：协议里声明要求，extension 里给默认实现，遵循方不实现也能用 -->

## 静态派发的坑：协议要求 vs 仅在扩展中定义
<!-- 本节最容易踩的点，必须有代码 + 输出对比：
     - 方法写进协议声明 → 动态派发，走具体类型的实现
     - 方法只写在 extension 里（协议未声明）→ 静态派发，按变量的编译期类型走默认实现
     建议贴出两段几乎相同的代码，只差「协议里有没有声明」，输出却不同 -->

## 用 where 约束扩展：给部分遵循方定制行为
<!-- extension P where Self: SomeClass / where Element == Int 之类，衔接下一节的条件一致性 -->

# 条件一致性：extension where 子句
<!-- 约 10 min。原理点：「只有当元素满足某条件时，容器才遵循某协议」——让泛型类型的能力随类型参数传导 -->

## 让 Array 在元素可比较时才获得新能力
<!-- 经典例子：extension Array: SomeProtocol where Element: Comparable，可运行代码 -->

## 标准库如何用条件一致性传导 Equatable / Hashable
<!-- 解释 [Int] 可以 == 而 [某不可比较类型] 不行，从这里理解「一致性是有条件的」 -->

# 协议组合与协议继承
<!-- 约 5-10 min -->

## some P1 & P2：多个能力的交集
<!-- 组合语法、作为参数类型与返回类型的写法；说明它表达的是「同时满足多个契约」 -->

## 协议继承与 AnyObject 约束
<!-- protocol A: B、class-only 协议（: AnyObject）在什么场景必须用（如 delegate 需要 weak） -->

## 组合 vs 继承：为什么 Swift 鼓励前者
<!-- 关键论点：类继承是单一的、强耦合的；协议组合是可叠加的。举一个「用继承会爆炸、用组合很自然」的例子 -->

# 协议导向编程：从继承树到能力拼装
<!-- 本节的收束，把前面四节串成一套方法论，约 5 min -->

## 一次重构：把继承层级改写成协议组合
<!-- 给一个小场景（如可缓存/可刷新/可分页的列表），先用继承实现，再用协议 + 默认实现重写，对比两者的扩展成本 -->

## 边界：什么时候协议反而是负担
<!-- 诚实记录负面场景：过度抽象、协议数量爆炸、只有一个实现却先写协议；以及 PAT 带来的限制预告 -->

<!-- 为 S3 打前置：当协议里出现 associatedtype 或 Self 要求，它就不能直接当类型用了——这正是 S3 泛型与关联类型要解决的问题 -->

# 验证理解：三段 Playground 代码
<!-- 每段附「预期输出 / 实际观察」，与 S1 笔记保持同一体例 -->

## 默认实现的派发差异：协议里声明与不声明
<!-- 最有价值的一段。同一份默认实现，因协议是否声明该要求而输出不同；用 any P 变量调用来暴露静态派发 -->

**预期输出**

**实际观察**

## 条件一致性：给 Array 加一个只在 Element: Comparable 时可用的方法
<!-- 验证约束不满足时编译器直接拒绝（可把报错信息贴出来） -->

**预期输出**

**实际观察**

## 协议组合：用 some P1 & P2 收敛函数签名
<!-- 对比「传三个具体类型的重载」与「一个组合约束的泛型函数」 -->

**预期输出**

**实际观察**

# 小结

<!--
用自己的话归纳，每个知识点一句话。可参考基准：
「协议是能力契约，协议扩展让契约自带默认行为；但只写在扩展里的方法是静态派发——协议声明与否决定了派发方式」
再补一条：还没弄明白 / 需要下一节回头看的问题
-->

| 知识点 | 一句话总结 |
|--------|-----------|
| 协议基础 |  |
| 协议扩展与默认实现 |  |
| 派发差异 |  |
| 条件一致性 |  |
| 协议组合与继承 |  |
| 协议导向编程 |  |

**还没弄明白 / 留给后续回头看的问题**：

<!-- 至少写 2-3 条，S1 的三条开放问题是个好参照；本节的候选：
     - 协议默认实现的静态派发在 SIL 层面是怎么决定的
     - any P 的存在容器（existential container）开销到底多大——留到 S4 量化
     - 上一节遗留的 🟡：ARC 循环引用排查（Memory Graph / Instruments），计划在 A2 单元补上 -->

# 扩展阅读

- [The Swift Programming Language 中文版 ·「协议」](https://doc.swiftgg.team/documentation/the-swift-programming-language/protocols/)
- [The Swift Programming Language 中文版 ·「扩展」](https://doc.swiftgg.team/documentation/the-swift-programming-language/extensions/)
- [WWDC15 · Protocol-Oriented Programming in Swift](https://developer.apple.com/videos/play/wwdc2015/408/)
- [Swift 进阶（Advanced Swift）· Protocols](https://objccn.io/products/advanced-swift/)
