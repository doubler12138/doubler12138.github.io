# 知识点掌握 Checklist

> 对标：阿里 P6–P7 / 字节 2-2～2-3 · 终端 + AI 复合型
> 生成日期：2026-08-03 · 配套文档：[核心能力图谱.md](核心能力图谱.md)
>
> **使用说明**：掌握一个知识点后将 `[ ]` 改为 `[x]`，可顺手在条目后追加日期如 `（2026-08-15）`。标注「了解」的条目知道思想即可，不必深钻；标注「手写/能讲」的条目以能输出为准。汇报进度时我会同步更新此表。

## 进度总览

| 能力域 | 条目数 | 已掌握 | 进度 |
|--------|--------|--------|------|
| 一、编程语言深度（Swift/OC） | 41 | 0 | ░░░░░░░░░░ 0% |
| 二、计算机基础（算法/网络/OS/模式） | 64 | 0 | ░░░░░░░░░░ 0% |
| 三、iOS 专业技术 | 31 | 0 | ░░░░░░░░░░ 0% |
| 四、底层原理与性能优化 | 26 | 0 | ░░░░░░░░░░ 0% |
| 五、架构设计与工程化 | 22 | 0 | ░░░░░░░░░░ 0% |
| 六、AI 基础与大模型原理 | 33 | 0 | ░░░░░░░░░░ 0% |
| 七、AI 应用工程（Agent/RAG/端侧） | 40 | 0 | ░░░░░░░░░░ 0% |
| 八、项目经验与工程素养 | 13 | 0 | ░░░░░░░░░░ 0% |
| **合计** | **270** | **0** | **0%** |

---

## 一、编程语言深度（Swift / Objective-C）

### Swift 语言核心

- [ ] 值类型 vs 引用类型：struct/class/enum 的内存差异与选型判断
- [ ] 枚举与关联值、模式匹配（switch / if case / guard case）
- [ ] 协议基础：定义、遵循、协议扩展与默认实现
- [ ] 关联类型与 Self 约束、PAT（带关联类型协议）不能直接作类型的问题
- [ ] 条件一致性：extension where 子句
- [ ] 协议组合（`some P1 & P2`）与类型擦除（能手写 AnyXxx Eraser）
- [ ] 泛型函数与泛型类型、泛型约束
- [ ] 闭包语法与尾随闭包、从上下文推断类型
- [ ] 闭包捕获语义：捕获列表 `[weak self]`/`[unowned]`、引用捕获 vs 值捕获
- [ ] 逃逸闭包（@escaping）与循环引用、内存影响
- [ ] @autoclosure 与惰性求值
- [ ] ARC 在 Swift 中的行为、循环引用排查
- [ ] Copy-on-Write 机制（能手写 isKnownUniquelyReferenced 判断）
- [ ] 所有权模型：~Copyable、consuming/borrowing（了解）
- [ ] 属性包装器 @propertyWrapper（能手写一个）
- [ ] Result Builder（理解 SwiftUI DSL 的实现原理）
- [ ] KeyPath 与 @dynamicMemberLookup
- [ ] 宏 Macro 基础概念（了解）
- [ ] async/await 基本用法与执行语义
- [ ] Actor 隔离与 @MainActor
- [ ] Sendable 协议与数据竞争检查
- [ ] 结构化并发：TaskGroup / async let / Task 取消与优先级
- [ ] 分布式 Actor（了解）
- [ ] 错误处理：throws/try/catch 与 Result 类型
- [ ] typed throws（了解）
- [ ] some 不透明类型 vs any 存在类型：区别、性能差异与选型
- [ ] Swift 与 C/C++ 互操作（了解）
- [ ] Swift 编译流程：源码 → AST → SIL → IR（了解）

### Objective-C 核心

- [ ] Runtime 数据结构：isa / class / method / protocol 结构体能画出来
- [ ] Method、IMP、SEL 的概念与关系
- [ ] objc_msgSend 消息发送流程（快速查找 → 慢速查找）
- [ ] 消息转发三阶段（能手写一个转发示例）
- [ ] Category 实现原理、与 Extension 的区别
- [ ] Associated Object 实现原理（哈希表存储）
- [ ] Block 三种类型及其迁移（栈 → 堆）
- [ ] __block 捕获原理、Block 循环引用与解决
- [ ] KVO 实现原理（isa-swizzling）、手动触发 KVO
- [ ] KVC 查找流程
- [ ] Property attribute 语义（atomic/copy/strong/nonatomic）与 setter 生成
- [ ] Method Swizzling 实践、load vs initialize 时机与风险
- [ ] OC 与 Swift 混编：桥接头文件、NS_SWIFT_NAME、CF 类型桥接

---

## 二、计算机基础

### 数据结构与算法

- [ ] 数组与字符串：常用操作与复杂度
- [ ] 链表：反转、环检测（快慢指针）、合并（能手写）
- [ ] 栈与队列：实现与经典应用（单调栈、单调队列）
- [ ] 哈希表：实现原理、冲突解决（拉链法/开放寻址）
- [ ] 二叉树：前中后序遍历（递归+迭代）、BST 性质
- [ ] AVL / 红黑树：平衡思想（红黑树细节了解）
- [ ] 堆：实现与 Top-K 问题
- [ ] 图的表示（邻接表/矩阵）与 BFS/DFS 模板
- [ ] Trie 树及应用场景
- [ ] 快排手写（partition 过程、复杂度与最坏情况）
- [ ] 归并排序手写（稳定性、逆序对应用）
- [ ] 堆排序手写
- [ ] 二分查找及变体（左右边界）
- [ ] 复杂度分析：时间/空间/均摊
- [ ] 对撞指针与快慢指针（模板题熟练）
- [ ] 滑动窗口（定长/变长模板）
- [ ] 回溯框架（排列/组合/子集）
- [ ] 贪心思想与经典题
- [ ] 分治与递归式分析
- [ ] DP 基础：状态定义与转移方程（爬楼梯/打家劫舍/LIS）
- [ ] 背包问题：01 背包与完全背包
- [ ] 区间 DP / 树形 DP（了解 + 经典题）
- [ ] 最短路：Dijkstra 手写、Floyd（了解）
- [ ] 拓扑排序与并查集
- [ ] 综合实战：LeetCode Hot 100 级别 30 分钟 bug-free

### 计算机网络

- [ ] OSI 七层 / TCP/IP 四层模型与各层职责
- [ ] TCP 三次握手：每次握手的作用（能讲清为什么是三次）
- [ ] TCP 四次挥手与 TIME_WAIT（为什么是 2MSL）
- [ ] TCP 滑动窗口与流量控制
- [ ] TCP 拥塞控制：慢启动/拥塞避免/快重传/快恢复
- [ ] TCP 粘包与解决方案
- [ ] UDP 特点与适用场景（直播/DNS/QUIC 底层）
- [ ] HTTP/1.1：keep-alive、管线化问题
- [ ] HTTPS 握手流程（TLS 1.2/1.3）与证书作用
- [ ] HTTP/2：多路复用、头部压缩
- [ ] HTTP/3 与 QUIC：解决队头阻塞、0-RTT
- [ ] DNS 解析流程、HTTPDNS、DNS 劫持防护
- [ ] 对称/非对称加密、数字签名、证书链
- [ ] Certificate Pinning 与中间人攻击
- [ ] 弱网优化：超时/重试/降级/预连接策略
- [ ] 长连接与心跳机制设计
- [ ] URLSession 底层与网络库设计思路
- [ ] WebSocket 与 SSE（AI 流式输出场景）

### 操作系统

- [ ] 进程 vs 线程 vs 协程
- [ ] 虚拟内存与分页机制
- [ ] mmap 原理与应用
- [ ] malloc 实现（iOS libmalloc zone）
- [ ] dirty/clean page 与内存占用统计口径
- [ ] 互斥锁/读写锁/自旋锁/信号量：区别与选型
- [ ] iOS 锁实践：os_unfair_lock / pthread_mutex / dispatch_semaphore
- [ ] 死锁四个必要条件与预防
- [ ] 阻塞/非阻塞 I/O、select/poll/kqueue
- [ ] iOS QoS 与线程优先级、GCD 队列与线程池的关系
- [ ] 沙盒与文件系统、文件读写性能

### 设计模式与架构思维

- [ ] 单例（Swift 正确写法与线程安全）
- [ ] 工厂与抽象工厂
- [ ] Builder 模式
- [ ] 适配器、装饰器、代理（Proxy）
- [ ] 外观（Facade）与组合模式
- [ ] 观察者模式（对应 Delegate/KVO/NotificationCenter）
- [ ] 策略、责任链、命令、迭代器、状态模式
- [ ] iOS 实践：Delegate vs Block、Target-Action、响应链即责任链
- [ ] SOLID 原则（能结合 iOS 代码举例）
- [ ] DRY/KISS/YAGNI、关注点分离、依赖倒置

---

## 三、iOS 专业技术

### UI 与渲染

- [ ] UIView / UIViewController 生命周期
- [ ] 事件响应链与 hitTest 命中测试
- [ ] 手势冲突与解决策略
- [ ] Auto Layout 约束求解原理与性能优化
- [ ] CALayer 与隐式动画、显式动画
- [ ] 渲染管线：CPU/GPU 协作与 commit transaction 流程
- [ ] 离屏渲染：触发条件、检测与规避
- [ ] 卡顿成因与检测方法
- [ ] 列表性能：预排版/复用/预加载（AsyncDisplayKit 思想）
- [ ] SwiftUI 声明式思想与 View 更新机制（diff）
- [ ] SwiftUI 状态管理：@State/@Binding/@Observable/@Environment
- [ ] SwiftUI 数据流架构与 NavigationStack
- [ ] SwiftUI 与 UIKit 混编（UIHostingController/UIHostingConfiguration）
- [ ] 动态化：热更新原理与合规风险、动态布局方案

### 并发编程

- [ ] GCD 队列（串行/并发/主）与 dispatch_async/sync 区别
- [ ] DispatchGroup、dispatch_semaphore、栅栏（barrier）
- [ ] dispatch_once 原理与线程安全单例
- [ ] RunLoop 数据结构与运行流程
- [ ] RunLoop 应用：卡顿检测、任务调度、AutoreleasePool 时机
- [ ] Operation：依赖、取消、自定义并发 Operation
- [ ] async/await 底层：协程与 Continuation
- [ ] Actor 隔离原理、Actor 重入与死锁规避
- [ ] 结构化并发实践：TaskGroup、async let
- [ ] Task 取消、优先级与优先级反转
- [ ] TSan 数据竞争检测、Sendable 检查、@MainActor

### 存储与数据

- [ ] 持久化选型：UserDefaults / 文件 / SQLite / Core Data
- [ ] SQLite：索引、WAL、线程安全（FMDB/GRDB）
- [ ] 缓存设计：NSCache、LRU 手写、多级缓存（内存-磁盘）
- [ ] 离线优先与冲突解决（了解）

### 多媒体与设备能力（按业务方向选学）

- [ ] AVFoundation 播放/录制管线与 AudioSession
- [ ] Widget / App Intents / Live Activity 集成

---

## 四、底层原理与性能优化

### 编译、链接与二进制

- [ ] OC 编译流程：Clang 前端 → LLVM IR → 机器码
- [ ] Swift 编译流程与 SIL 优化（了解）
- [ ] Mach-O 结构：Header / Load Commands / Segment / Section
- [ ] 静态库 vs 动态库、符号解析与 bind / lazy bind
- [ ] dyld 加载流程：load → rebase → bind → ObjC setup → initializers
- [ ] 包大小分析：Link Map、dead code stripping

### App 启动优化

- [ ] 冷启动全阶段：dyld → runtime → main() → 首帧渲染
- [ ] pre-main 优化：减少动态库、合并 Category、+load 治理
- [ ] 二进制重排：SanitizerCoverage 采集 + Page Order
- [ ] main() 后优化：任务分级、延迟加载、预加载
- [ ] 启动度量：线上监控、分位数统计、劣化防控

### 卡顿与渲染优化

- [ ] 一帧 16.67ms 的预算分配（CA commit → Render Server）
- [ ] 卡顿检测：RunLoop 超时 / 子线程 ping / CADisplayLink
- [ ] Instruments：Time Profiler / System Trace / Animation Hitches
- [ ] 优化手段：主线程减负、预计算/预排版、异步渲染

### 内存优化

- [ ] iOS 内存模型：虚拟内存、Jetsam 机制、内存水位线
- [ ] OOM 归因：FOOM / BOOM 区分
- [ ] Memory Graph 与内存快照对比分析
- [ ] 优化手段：AutoreleasePool、图片降采样/解码、缓存上限、循环引用治理
- [ ] 线上内存监控体系设计

### 稳定性建设

- [ ] Crash 收集原理：Mach 异常 / Unix 信号 / NSException（KSCrash 架构）
- [ ] dSYM 符号化流程（离线/在线）
- [ ] 死锁 / Watchdog / ANR 检测方案
- [ ] APM 体系：采集 → 聚合 → 归因 → 报警 → 防劣化

### 电量与包大小

- [ ] 耗电归因与 Energy Organizer
- [ ] App Thinning / On-Demand Resources

---

## 五、架构设计与工程化

### App 架构模式

- [ ] Apple 式 MVC 的问题与 Massive ViewController
- [ ] MVVM：ViewModel 职责与数据绑定（Combine/闭包/@Observable）
- [ ] VIPER 与 Clean Architecture 的映射
- [ ] TCA：单向数据流、State/Action/Reducer/Effect
- [ ] 架构选型判断：按团队规模与业务复杂度取舍（能讲出理由）

### 组件化与模块化

- [ ] 组件化方案：CocoaPods 私有库 / Framework / SPM
- [ ] 模块间通信：URL Router / Protocol / Target-Action
- [ ] 依赖管理：依赖方向控制、循环依赖治理、依赖注入
- [ ] 大型 App 工程：多 Target/Scheme、编译加速

### 工程化与质量

- [ ] CI/CD：Fastlane / Xcode Cloud 流水线搭建
- [ ] 单元测试：XCTest / Swift Testing、TDD 实践
- [ ] UI 测试与快照测试
- [ ] SwiftLint / 静态分析 / Code Review 机制
- [ ] Git Flow、AB 实验、灰度发布

### 终端系统设计（P7 面试高频，要求能白板设计）

- [ ] 设计一个图片库（加载/缓存/解码/降采样）
- [ ] 设计一个网络层（请求抽象/拦截器/重试/弱网策略）
- [ ] 设计一个路由框架 / 组件化方案
- [ ] 设计一个 APM / 监控系统
- [ ] 设计一个 AI 对话模块（流式渲染/上下文管理/降级策略）
- [ ] 跨端理解：Flutter（Skia 渲染）架构（了解）
- [ ] 跨端理解：React Native（Bridge/Fabric）架构（了解）
- [ ] 跨端选型判断：Flutter / RN / KMP 适用边界

---

## 六、AI 基础与大模型原理

### 数学与机器学习基础

- [ ] 线性代数：向量/矩阵运算、点积的几何意义
- [ ] 概率：条件概率、贝叶斯、期望/方差
- [ ] 微积分直觉：梯度、链式法则、损失曲面
- [ ] 监督 / 无监督 / 强化学习的分类与例子
- [ ] 过拟合与正则化（L1/L2/Dropout）
- [ ] 交叉验证与评估指标：Precision / Recall / F1 / AUC
- [ ] 经典模型思想：线性回归 / 逻辑回归 / 决策树（了解）

### 深度学习

- [ ] 感知机 → 多层网络、激活函数（ReLU/GELU/Sigmoid）
- [ ] 前向传播与损失函数
- [ ] 反向传播（能推导一个简单例子并讲清）
- [ ] 梯度下降：SGD / Adam 与学习率
- [ ] CNN 原理（卷积/池化）
- [ ] RNN/LSTM 及其局限
- [ ] Attention 动机：为什么取代 RNN
- [ ] Dropout / BatchNorm / LayerNorm
- [ ] PyTorch 基础：Tensor / autograd，能跑通一段训练代码

### Transformer 与大语言模型

- [ ] Self-Attention：Q/K/V 的含义与计算过程（能手推一个小例子）
- [ ] 为什么除以 √d_k
- [ ] Multi-Head Attention 的作用
- [ ] 位置编码：正弦编码 → RoPE
- [ ] GPT Decoder-Only 架构与因果 Mask
- [ ] 自回归生成过程
- [ ] Tokenizer：BPE 分词算法
- [ ] 预训练目标与 Scaling Law
- [ ] 涌现能力（了解）
- [ ] 推理两阶段：Prefill 与 Decode、KV Cache
- [ ] 采样策略：Temperature / Top-p / Top-k

### 微调与对齐

- [ ] SFT：指令数据格式与微调流程
- [ ] LoRA / QLoRA 原理与适用场景
- [ ] RLHF 流程：奖励模型 + PPO（理解思想）
- [ ] DPO 思路（了解）
- [ ] 模型评估：MMLU / HumanEval / GSM8K 与人工评估
- [ ] 模型选型判断：开源 vs 闭源、参数量 vs 推理成本、端侧 vs 云端

---

## 七、AI 应用工程（Agent / RAG / 端侧 AI）

### LLM 应用开发基础

- [ ] 模型 API 调用：请求结构、流式输出（SSE）
- [ ] Token 计费与上下文窗口概念
- [ ] Prompt 基础：角色设定、清晰指令、示例
- [ ] Few-shot 与 CoT（Chain-of-Thought）
- [ ] 结构化输出：JSON Mode / Function Calling
- [ ] 上下文工程：对话历史压缩、System Prompt 设计
- [ ] 输出可靠性：JSON Schema 校验、重试与兜底

### RAG（检索增强生成）

- [ ] RAG 全链路流程（能画出 pipeline）
- [ ] 文档加载与 Chunk 切片策略
- [ ] Embedding 原理与模型选型
- [ ] 向量数据库：FAISS / Chroma / Milvus 与索引类型（IVF/HNSW）
- [ ] 相似度计算：余弦 / 点积 / 欧氏
- [ ] 混合检索（向量 + 关键词 BM25）
- [ ] Query 改写与 Reranker 重排
- [ ] RAG 评测：RAGAS 框架与 Bad Case 分析
- [ ] 动手：独立搭建一个可运行的最小 RAG demo

### Agent 架构

- [ ] ReAct 范式（Thought → Action → Observation 循环）
- [ ] Function Calling：工具定义与调用协议
- [ ] 工具调用的错误处理与重试
- [ ] Planning：任务分解、CoT/ToT、Reflection
- [ ] Memory：短期（上下文）vs 长期（向量存储/摘要）
- [ ] 多 Agent 协作：角色分配、通信模式与局限
- [ ] LangChain / LangGraph 核心抽象与实践
- [ ] AutoGen / CrewAI / OpenAI Agents SDK 对比（了解）
- [ ] 动手：实现一个带工具调用的小型 Agent

### Harness 与工程落地

- [ ] Agent Harness 概念与运行时架构
- [ ] 工具沙箱与权限控制
- [ ] Eval 评测体系：任务设计、自动评估、回归测试
- [ ] 可观测性：Trace / Log / 指标
- [ ] Token 成本控制策略
- [ ] Prompt 注入防护与 Guardrails

### 端侧 AI（终端开发者独特优势）

- [ ] Core ML：模型格式（.mlpackage）、加载与推理
- [ ] ANE / GPU 推理加速
- [ ] PyTorch → Core ML 转换流程
- [ ] 量化：INT8 / FP16
- [ ] 模型蒸馏（了解）
- [ ] Apple Foundation Models / Apple Intelligence 集成
- [ ] 端侧 RAG：本地 Embedding + 向量检索
- [ ] AI 功能端侧架构：流式渲染（打字机效果）、超时/降级/重试
- [ ] 端云协同：端侧小模型 + 云端大模型的路由策略

---

## 八、项目经验与工程素养

### 项目深度（面试讲故事的弹药库）

- [ ] 完整大型 App 经验：能用 STAR 讲清背景 → 挑战 → 方案 → 落地 → 量化结果
- [ ] 性能优化专项：至少一个有数据支撑的完整案例（启动/卡顿/内存/包大小/稳定性）
- [ ] AI 落地项目：在 App 中集成 AI 功能的完整经验

### 技术方案能力

- [ ] 需求分析与技术拆解（从模糊需求到可执行方案）
- [ ] 方案对比与选型：能列 2–3 个方案并讲取舍
- [ ] 技术文档 / RFC 写作与方案评审
- [ ] 风险识别与预案

### AI 协作与效率（2026 必备）

- [ ] AI 辅助编程工具高效使用（Cursor / Copilot 等）
- [ ] AI 辅助工作流：代码 Review / 测试生成 / 文档生成 / Debug
- [ ] 能识别 AI 生成代码的局限与风险、能 Review AI 产出

### 技术影响力（P7 要求）

- [ ] 技术分享 / 技术博客
- [ ] 方法论沉淀：把经验变成团队可复用的工具 / 规范 / 流程
- [ ] 开源贡献或内部工具建设
