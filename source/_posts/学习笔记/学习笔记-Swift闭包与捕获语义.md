---
title: 学习笔记——Swift闭包与捕获语义
date: 2026-08-03T22:30:00
author: DoubleR
img: /images/学习笔记-Swift闭包与捕获语义/cover.jpg
top: false
hide: false
cover: true
toc: true
mathjax: false
summary: 主题 01「Swift 基础进阶」S1 节次学习笔记，梳理闭包表达式、逃逸闭包 @escaping、自动闭包 @autoclosure 与捕获列表的语义，并从 ARC 视角理解循环引用的形成与破解。
categories: 学习笔记
tags:
  - 学习笔记
  - Swift
  - 闭包
  - 捕获列表
  - 逃逸闭包
---

<!--
写作提示（发布前可整体删除本注释）
- 本文是 S1 节次的交付物，进度追踪 Skill 会按「独立小节 + 原理解释/代码示例/推导 + 小结归纳」判定是否勾选 Checklist
- 因此每个 h1 下至少要有：一段原理性解释 + 一段可运行代码
- 标注「能手写」的条目（如 @autoclosure 日志函数）必须给出完整可运行代码，否则只算「部分覆盖」
-->

## 本节定位

| 项目 | 内容 |
|------|------|
| 所属单元 | A1 Swift 语言进阶 |
| 所属节次 | 主题 01 · S1（工作日档 30-45min） |
| 主资料 | [The Swift Programming Language 中文版 ·「闭包」](https://doc.swiftgg.team/documentation/the-swift-programming-language/closures/) |
| 本节产出 | Playground 三段验证代码 + 本篇笔记 |

**对应 Checklist 条目**（覆盖后即可勾选）：

- 闭包语法与尾随闭包、从上下文推断类型
- 闭包捕获语义：捕获列表 `[weak self]`/`[unowned]`、引用捕获 vs 值捕获
- 逃逸闭包（@escaping）与循环引用、内存影响
- @autoclosure 与惰性求值

<!-- 引言：用两三句话交代「为什么闭包是 Swift 抽象能力的根基」，以及本节想解决的疑问 -->

# 闭包表达式：语法回顾

<!-- 目标：确认语法无障碍，5 min 快速过。原理点在于「闭包是可以捕获上下文的匿名函数」 -->

## 完整语法与简写规则

<!-- 从完整写法逐步简写到 $0，说明每一步省略了什么、编译器凭什么推断 -->

## 尾随闭包

<!-- 尾随闭包语法糖、多尾随闭包写法；说明 SwiftUI 的 DSL 观感从何而来 -->

## 从上下文推断类型

<!-- 参数类型/返回值/单表达式隐式返回的推断规则 -->

# 逃逸闭包 @escaping

<!-- 本节重点，面试高频点，约 10 min -->

## 什么是逃逸：闭包生命周期超出函数调用

<!-- 原理：非逃逸闭包可栈上分配、编译器可优化；逃逸闭包需堆分配并延长生命周期 -->

## 为什么异步回调必须逃逸

<!-- 以网络请求 / DispatchQueue.asyncAfter 为例说明 -->

## 逃逸闭包中 self 的显式引用要求

<!-- 编译器强制写 self 的用意：提醒你此处正在延长 self 的生命周期 -->

## 逃逸闭包与内存影响

<!-- 堆分配开销、被属性持有后的引用关系 -->

# 自动闭包 @autoclosure

<!-- 了解层级，约 5 min -->

## 惰性求值：表达式为什么没被执行

<!-- 说明 @autoclosure 把表达式包装成无参闭包，延迟到调用点才求值 -->

## 典型场景：assert 与日志

<!-- 结合标准库 assert / precondition 的签名说明设计意图 -->

# 捕获值与捕获列表

<!-- 本节最核心，5-10 min -->

## 闭包如何捕获外部变量：引用捕获

<!-- 关键结论：默认捕获的是变量本身（引用），不是调用时刻的值快照。用计数器例子验证 -->

## 捕获列表与值捕获

<!-- [x] 形式在闭包创建时拷贝当前值；对比引用捕获的行为差异 -->

## [weak self] 与 [unowned self]

<!-- 语义差异（Optional vs 非 Optional）、崩溃风险、各自适用场景与选择依据 -->

## ARC 视角：循环引用如何形成与打破

<!-- 画出引用关系：self → 闭包属性 → 捕获 self；说明 [weak self] 在哪一环断开 -->

# 验证理解：三段 Playground 代码

<!-- 把 S1 要求的三段验证代码贴进来，每段附「预期输出 / 实际观察」 -->

## 强引用 vs [weak self]：观察 deinit 是否调用

<!-- class 持有闭包属性，两种捕获方式对比，用 deinit 打印验证 -->

## @escaping 场景：体会编译器为何要求显式 self

<!-- DispatchQueue.main.asyncAfter 示例 -->

## 用 @autoclosure 实现 log(level:_:)

<!-- 完整可运行实现，并验证参数只在需要时才求值（在表达式里打印以证明） -->

# 小结

<!--
用自己的话归纳，每个知识点一句话。参考基准：
「闭包捕获的是变量的引用而非值；逃逸闭包 + 引用类型 self 是循环引用的温床，[weak self] 是标准解法」
再补一条：还没弄明白 / 需要下一节回头看的问题
-->

# 扩展阅读

- [The Swift Programming Language 中文版 ·「闭包」](https://doc.swiftgg.team/documentation/the-swift-programming-language/closures/)
- [The Swift Programming Language 中文版 ·「自动引用计数」](https://doc.swiftgg.team/documentation/the-swift-programming-language/automaticreferencecounting/)
- [Swift 进阶（Advanced Swift）· Functions / Closures](https://objccn.io/products/advanced-swift/)

<!-- 为 S2 打前置：闭包是「行为抽象」，下一节的协议是「接口抽象」，思考两者的互补关系 -->
