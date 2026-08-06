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

闭包可以捕获和存储其所在上下文中任意常量和变量的引用。这个过程可以看作是将这些常量和变量**包含**在闭包的作用域内。

# 闭包表达式：语法回顾
<!-- 目标：确认语法无障碍，5 min 快速过。原理点在于「闭包是可以捕获上下文的匿名函数」 -->
闭包表达式语法一般形式如下：
```Swift
{ (<#parameters#>) -> <#return type#> in
  <#statements#>
}
```
闭包表达式语法中的**参数**可以是in-out参数，但不能具有默认值。如果你命名了可变参数，也可以使用可变参数。
```Swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in 
  return s1 > s2
})
```
闭包函数主体以in关键字开始。这个关键字表示闭包的参数和返回类型定义已经结束，接下来是闭包的实际内容。

当闭包作为內联表达式传递给函数或方法时，Swift通常能够推断出参数类型和返回类型。这意味着，在使用闭包作为函数或方法参数时，可以不写出完整的闭包形式
```Swift
reversedNames = names.sorted(by: {s1, s2 in return s1 > s2})
```

单表达式闭包可以省略return关键字
```Swift
reversedNames = names.sorted(by: {s1, s2 in s1 > s2})
```

Swift自动为内联闭包提供了简写参数名称功能，你可以直接通过$0,$1等来引用闭包参数的值
```Swift
reversedNames = names.sorted(by: { $0 > $1 })
```

## 尾随闭包
<!-- 尾随闭包语法糖、多尾随闭包写法；说明 SwiftUI 的 DSL 观感从何而来 -->
如果你需要将闭包表达式作为函数的最后一个参数传递给函数，并且闭包表达式很长，则将其编写为 **尾随闭包** 的形式
```Swift
reversedNames = names.sorted() { $0 > $1 }
```

如果一个闭包表达式是函数或方法的唯一参数，则写作尾随闭包时可以省略括号
```Swift
reversedNames = names.sorted { $0 > $1 }
```

# 逃逸闭包 @escaping
<!-- 本节重点，面试高频点，约 10 min -->
当闭包作为参数传递给函数，但是这个闭包在函数返回之后才被执行，该闭包被称为 **逃逸闭包**。当你声明一个将闭包作为其参数之一的函数时，你可以通过在参数类型之前写@escaping，以表示这个闭包是允许逃逸的

闭包逃逸的一种常见方式是将其存储在函数外部定义的变量中
```Swift
var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
  completionHandlers.append(completionHandler)
}
```

## 为什么异步回调必须逃逸
<!-- 以网络请求 / DispatchQueue.asyncAfter 为例说明 -->
@escaping不是一个“权限开关”，而是一个**关于生命周期的契约声明**。非逃逸闭包的契约是：函数返回之前，这个闭包一定被执行完或者被丢弃。异步回调天生违背这个契约——它的执行必定延迟，因此必须逃逸

非逃逸不只是个标注，编译器会据此做优化：
* 闭包的捕获上下文（context）可以放在栈上分配，不用走堆
* 捕获的引用类型不需要额外的retain/release，反正闭包执行的时候调用方那边的引用类型变量还活着

如果一个异步回调被当成非逃逸使用，等它真正执行时，那块栈内存早已被后续调用覆盖——捕获的变量变成了悬垂指针，直接是内存安全问题。所以这不是"要不要写 @escaping"的风格问题，而是编译器必须知道要不要把上下文装箱到堆上、要不要 retain 捕获物。

## 逃逸闭包中 self 的显式引用要求
<!-- 编译器强制写 self 的用意：提醒你此处正在延长 self 的生命周期 -->
在逃逸闭包中引用self时，如果self指向一个类的实例，就需要特别注意。在逃逸闭包中捕获self很容易意外创建强引用循环，即实例self持有闭包，闭包捕获self，导致循环引用。

因此，通常情况下，闭包会通过在其体内使用变量来隐式捕获这些变量，但使用self时，必须显式进行捕获，提醒检测是否存在循环引用
```Swift
someFunctionWithEscapingClosure { [self] in x = 100} 
```

# 自动闭包 @autoclosure
<!-- 了解层级，约 5 min -->
**自动闭包**是一种自动创建的闭包，用于包装作为参数传递给函数的表达式。它不接受任何参数，当它被调用时，它返回包裹在其中的表达式的值

## 惰性求值：表达式为什么没被执行
<!-- 说明 @autoclosure 把表达式包装成无参闭包，延迟到调用点才求值 -->
自动闭包允许延迟计算，在调用闭包之前，内部代码不会运行。延迟计算对于有副作用或高计算成本的代码非常有用
```Swift
var customersInLine = ["Chris", "Alex", "Eva", "Barry", "Daniella"]
print(customersInLine.count) // 5

let customerProvider = { customersInLine.remove(at: 0) }

print(customersInLine.count) // 5

print("Now serving \(customerProvider())!")
// Now serving Chris!
print(customersInLine.count) // 4
```

# 捕获值与捕获列表
<!-- 本节最核心，5-10 min -->
闭包可以从定义它的环境上下文中捕获常量和变量。即使定义这些常量和变量的原作用域已经不存在，闭包仍可以在闭包函数体内引用和修改这些值

## 闭包如何捕获外部变量：引用捕获
<!-- 关键结论：默认捕获的是变量本身（引用），不是调用时刻的值快照。用计数器例子验证 -->
默认情况下，闭包捕获的是**变量本身**，而不是闭包创建那一刻的值快照。所以外部变量后续被修改，闭包看到的是最新值
```Swift
var counter = 0
let printCounter = { print(counter) }
counter = 10
printCounter() // 10，不是 0
```

更能说明问题的是嵌套函数的计数器例子。`runningTotal` 是 `makeIncrementer` 的局部变量，按常理函数返回后就该销毁，但因为被 `incrementer` 捕获，Swift 会把它从栈上挪到堆上装箱，其生命周期改为跟随闭包
```Swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
  var runningTotal = 0
  func incrementer() -> Int {
    runningTotal += amount
    return runningTotal
  }
  return incrementer
}

let incrementByTen = makeIncrementer(forIncrement: 10)
incrementByTen() // 10
incrementByTen() // 20
incrementByTen() // 30

let incrementBySeven = makeIncrementer(forIncrement: 7)
incrementBySeven() // 7，与 incrementByTen 各自持有独立的 runningTotal
incrementByTen()   // 40，互不干扰
```

两个关键结论：
* 闭包能**读写**捕获的变量，说明捕获的不是值拷贝而是存储位置
* 每次调用 `makeIncrementer` 都会新建一个盒子，闭包与它捕获的变量构成一个独立的「状态单元」

## 捕获列表与值捕获
<!-- [x] 形式在闭包创建时拷贝当前值；对比引用捕获的行为差异 -->
如果希望捕获的是「此刻的值」，就要用捕获列表把变量显式列出来。`[counter]` 是 `[counter = counter]` 的简写，它在闭包**创建时**求值并把结果拷贝一份存进闭包，之后外部怎么改都与闭包无关
```Swift
var counter = 0
let byReference = { print("引用捕获: \(counter)") }
let byValue = { [counter] in print("值捕获: \(counter)") }

counter = 10
byReference() // 引用捕获: 10
byValue()     // 值捕获: 0
```

注意值捕获拿到的是**常量**，闭包内不能对它赋值。

还有一个容易踩的点：对引用类型来说，值捕获拷贝的只是「引用」这个值（指针），指向的对象依然是共享的
```Swift
class Box { var value = 0 }

var box = Box()
let closure = { [box] in print(box.value) }

box.value = 42
closure()   // 42：拷贝的是指针，对象还是同一个

box = Box() // 让外部变量指向新对象
closure()   // 42：闭包仍持有旧对象的强引用
```

这条性质正是 `[weak self]` 能生效的基础——捕获列表操作的是「怎么持有这个引用」，而不是「拷贝一份对象」。

## [weak self] 与 [unowned self]
<!-- 语义差异（Optional vs 非 Optional）、崩溃风险、各自适用场景与选择依据 -->
两者都**不增加**引用计数，差别在对象已释放之后的行为

| 对比项 | `[weak self]` | `[unowned self]` |
|--------|---------------|------------------|
| 闭包内 self 的类型 | `Self?`（Optional） | `Self`（非 Optional） |
| 是否增加引用计数 | 否 | 否 |
| 对象已释放后访问 | 得到 `nil`，可安全提前返回 | 悬垂引用，访问即崩溃 |
| 运行时开销 | 需要维护 / 查询弱引用表 | 几乎为零 |

```Swift
class ViewModel {
  var onFinish: (() -> Void)?

  func load() {
    fetch { [weak self] result in
      guard let self else { return }  // Swift 5.7+ 可省略 = self
      self.handle(result)
    }
  }
}
```

选择依据只有一条：能否**从生命周期上证明**闭包执行时 `self` 一定还活着。
* 异步回调、存成属性的长生命周期闭包、通知/定时器回调 → 一律 `[weak self]`
* 只有当闭包与 `self` 同生共死（比如闭包由 `self` 独占持有且随 `self` 一起销毁），才值得用 `[unowned self]` 换掉那次可选解包

拿不准就用 `weak`：weak 的代价是一次弱引用表查询，unowned 的代价是崩溃。

## ARC 视角：循环引用如何形成与打破
<!-- 画出引用关系：self → 闭包属性 → 捕获 self；说明 [weak self] 在哪一环断开 -->
循环引用的本质是两条强引用首尾相接，让引用计数永远降不到 0
```
┌──────────────────────────────────────────────┐
│                                              │
│   ViewModel 实例 ──strong──▶ onFinish 闭包    │
│         ▲                        │            │
│         └────strong（捕获 self）──┘            │
│                                              │
└──────────────────────────────────────────────┘
```

* `self` 持有闭包：闭包被存进了 `onFinish` 属性
* 闭包持有 `self`：闭包体内用到了 `self`，于是默认强引用捕获

结果是即使外部所有变量都不再指向这个实例，它的引用计数仍是 1，`deinit` 永远不会执行，内存泄漏成立。

打破环有两个下手处，实践中只推荐第一个：
1. **改「闭包 → self」这一环**：用 `[weak self]` 让这条边变成弱引用，不参与引用计数。外部最后一个强引用消失时实例正常析构，闭包内的 `self` 随即变为 `nil`
2. 改「self → 闭包」这一环：在合适时机手动 `onFinish = nil`。但这依赖调用方记得清理，一旦漏掉又回到泄漏状态，不如捕获列表可靠

判断口诀：**闭包被 self 直接或间接持有（存成属性、或存进 self 的子对象），并且闭包里用到了 self** → 存在循环引用风险。反过来，像 `map`、`sorted` 这种立即执行的非逃逸闭包不会形成循环，因为函数返回时闭包就已经释放了——这也解释了为什么编译器只在逃逸闭包里强制要求显式写 `self`。

# 验证理解：三段 Playground 代码
<!-- 把 S1 要求的三段验证代码贴进来，每段附「预期输出 / 实际观察」 -->
以下三段代码在 Playground 中可直接运行。涉及异步的那段需要先打开 `PlaygroundPage.current.needsIndefiniteExecution = true`，否则主线程执行完就退出，看不到延迟回调。

## 强引用 vs [weak self]：观察 deinit 是否调用
<!-- class 持有闭包属性，两种捕获方式对比，用 deinit 打印验证 -->
```Swift
class Downloader {
  let name: String
  var onComplete: (() -> Void)?

  init(name: String) {
    self.name = name
    print("\(name) init")
  }

  func setupStrong() {
    onComplete = { print("\(self.name) 完成（强引用捕获）") }
  }

  func setupWeak() {
    onComplete = { [weak self] in
      guard let self else { return }
      print("\(self.name) 完成（weak 捕获）")
    }
  }

  deinit { print("\(name) deinit") }
}

do {
  let a = Downloader(name: "A-强引用")
  a.setupStrong()
  a.onComplete?()
}

do {
  let b = Downloader(name: "B-weak")
  b.setupWeak()
  b.onComplete?()
}
```

**预期输出**
```
A-强引用 init
A-强引用 完成（强引用捕获）
B-weak init
B-weak 完成（weak 捕获）
B-weak deinit
```

**实际观察**：与预期一致。关键是 `A-强引用 deinit` 从头到尾没有出现——离开 `do` 作用域后局部变量 `a` 已经销毁，但实例的引用计数仍为 1（被自己的 `onComplete` 闭包持有），泄漏就发生在这里。B 因为闭包只持有弱引用，作用域结束即正常析构。

## @escaping 场景：体会编译器为何要求显式 self
<!-- DispatchQueue.main.asyncAfter 示例 -->
```Swift
import Foundation
import PlaygroundSupport

PlaygroundPage.current.needsIndefiniteExecution = true

class DelayedGreeter {
  var name = "DoubleR"

  // 非逃逸：函数返回前一定执行完，闭包里可以直接写 name
  func greetNow(_ body: (String) -> Void) {
    body(name)
  }

  // 逃逸：执行时机在函数返回之后，必须显式捕获 self
  func greetLater() {
    DispatchQueue.main.asyncAfter(deadline: .now() + 1) { [weak self] in
      guard let self else {
        print("实例已释放，跳过问候")
        return
      }
      print("Hello, \(self.name)")
    }
    print("greetLater 已返回")
  }

  deinit { print("DelayedGreeter deinit") }
}

var greeter: DelayedGreeter? = DelayedGreeter()
greeter?.greetLater()
greeter = nil   // 在回调触发之前就释放
```

**预期输出**
```
greetLater 已返回
DelayedGreeter deinit
实例已释放，跳过问候
```

**实际观察**：三点值得记下来。
* `greetLater 已返回` 先于回调打印，直观印证「逃逸 = 执行时机晚于函数返回」
* 把 `[weak self]` 整个去掉、直接在闭包里写 `name`，编译器会报 `Reference to property 'name' in closure requires explicit use of 'self'`。它并不是在为难你，而是逼你意识到「这里正在延长 self 的生命周期」
* 改成 `[unowned self]` 同样能通过编译，但 1 秒后访问已释放的实例会直接崩溃。同一份代码，`weak` 打印一行日志，`unowned` 直接挂掉——这就是前面说的「拿不准就用 weak」

对比 `greetNow`：它是非逃逸闭包，函数返回前必然执行完，所以既不用写 `self` 也不可能形成循环引用。

## 用 @autoclosure 实现 log(level:_:)
<!-- 完整可运行实现，并验证参数只在需要时才求值（在表达式里打印以证明） -->
```Swift
enum LogLevel: Int, Comparable {
  case debug = 0, info, warning, error

  static func < (lhs: LogLevel, rhs: LogLevel) -> Bool {
    lhs.rawValue < rhs.rawValue
  }
}

var minimumLevel: LogLevel = .warning

func log(level: LogLevel, _ message: @autoclosure () -> String) {
  guard level >= minimumLevel else { return }
  print("[\(level)] \(message())")
}

func expensiveDescription() -> String {
  print("  ⚠️ expensiveDescription 被求值了")
  return "一次昂贵的字符串拼接结果"
}

log(level: .debug, expensiveDescription())   // 被等级过滤，表达式不该求值
log(level: .error, expensiveDescription())   // 通过过滤，表达式才求值
```

**预期输出**
```
  ⚠️ expensiveDescription 被求值了
[error] 一次昂贵的字符串拼接结果
```

**实际观察**：第一次 `log` 完全没有触发 `expensiveDescription` 的打印。原因是 `expensiveDescription()` 这个表达式被 `@autoclosure` 包装成了 `() -> String`，`guard` 提前 `return`，闭包根本没被调用。

把参数类型改回普通的 `String` 再跑一遍，两次调用都会先求值再进函数，输出里会多出一行被过滤掉却仍然付出了代价的求值日志——这就是 `assert`、`precondition` 以及各类日志 API 都选择 `@autoclosure` 的原因。

还有一个细节：`@autoclosure` 修饰的闭包默认是**非逃逸**的。如果想把它存起来、延迟到函数返回之后再求值，必须写成 `@escaping @autoclosure`。

# 小结

<!--
用自己的话归纳，每个知识点一句话。参考基准：
「闭包捕获的是变量的引用而非值；逃逸闭包 + 引用类型 self 是循环引用的温床，[weak self] 是标准解法」
再补一条：还没弄明白 / 需要下一节回头看的问题
-->

用一句话概括本节：**闭包捕获的是变量的引用而非值；逃逸闭包 + 引用类型 self 是循环引用的温床，`[weak self]` 是标准解法。**

拆开来，每个知识点各一句：

| 知识点 | 一句话总结 |
|--------|-----------|
| 闭包表达式 | 闭包是能捕获上下文的匿名函数，类型推断 + 简写参数名 + 隐式 return 让它逐步退化成 `{ $0 > $1 }` |
| 尾随闭包 | 最后一个闭包参数可以移到括号外，唯一参数时括号能整个省掉，SwiftUI 的 DSL 观感就来自这里 |
| @escaping | 不是权限开关，而是生命周期契约：闭包会活过函数返回，因此上下文必须装箱到堆上并 retain 捕获物 |
| 显式 self | 编译器只在逃逸闭包里强制写 `self`，用意是提醒你此处正在延长 self 的生命周期 |
| @autoclosure | 把实参表达式自动包成无参闭包，求值时机推迟到调用点，代价是可读性（调用方看不出这是闭包） |
| 引用捕获 | 捕获的是存储位置而非快照，被捕获的局部变量会被装箱到堆上，生命周期跟随闭包 |
| 值捕获 | `[x]` 在闭包创建时拷贝一份常量；引用类型拷贝的只是指针，对象依然共享 |
| 循环引用 | self 持有闭包 + 闭包捕获 self = 引用计数降不到 0，`deinit` 永不执行 |
| weak / unowned | 都不增加引用计数；weak 得到 Optional 可安全降级，unowned 非 Optional 但对象释放后访问即崩溃 |

**还没弄明白 / 留给后续回头看的问题**：

* `[unowned self]` 在真实项目里究竟有多少场景值得用？想找一份能证明「生命周期严格同步」的实际用例，而不是教科书式的假设
* 「非逃逸闭包的上下文分配在栈上」是编译器的**保证**还是**优化机会**？想从 SIL 层面验证一次
* Swift 6 严格并发模式下 `Sendable` 闭包的捕获检查，与本节的捕获语义如何叠加——捕获列表是否会成为跨隔离域传值的主要手段

# 扩展阅读

- [The Swift Programming Language 中文版 ·「闭包」](https://doc.swiftgg.team/documentation/the-swift-programming-language/closures/)
- [The Swift Programming Language 中文版 ·「自动引用计数」](https://doc.swiftgg.team/documentation/the-swift-programming-language/automaticreferencecounting/)
- [Swift 进阶（Advanced Swift）· Functions / Closures](https://objccn.io/products/advanced-swift/)

<!-- 为 S2 打前置：闭包是「行为抽象」，下一节的协议是「接口抽象」，思考两者的互补关系 -->
