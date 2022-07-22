# 背景 和 概念
***WebAssembly(wasm - 网页汇编)*** 是一个简单的机器模型，和拥有一个 [extensive specification](https://webassembly.github.io/spec/) 的可执行格式。被设计为 可移植的、精炼的 并且以/接近 本地速度执行。      
作为一种编程语言，WebAssembly 由两种表示相同结构的格式组成，尽管方式不同：
1. `.wat` 文本格式(暨 **W**eb**A**ssembly **T**ext 的缩写)，使用 [S-expressions](https://en.wikipedia.org/wiki/S-expression)，并且与***Scheme*** 和***Clojure*** 等 ***Lisp*** 语言家族有一些相似之处；
2. `.wasm` 二进制格式，更 low-level，用于 wasm 虚拟机直接使用。它在概念上类似于 ***ELF*** 和 ***Mach-O***

## 线性内存
WebAssembly 拥有一个非常简单的 [memory model](https://webassembly.github.io/spec/core/syntax/modules.html#syntax-mem)。一个 wasm 模块可以访问单个“线性内存”，它本质上是一个扁平的字节数组。该 [memory can be grown](https://webassembly.github.io/spec/core/syntax/instructions.html#syntax-instr-memory)，通过按页面大小（64K）的倍数增长，它不能收缩。

## 仅用于 Web？
尽管 wasm 目前在 ***JavaScript*** 和 Web 社区中受到了普遍关注，但它并没有对其主机环境做出任何假设。因此，有理由推测 wasm 将成为一种“可移植可执行”格式，在未来的各种环境中使用。然而，到目前为止，wasm 主要与 ***JavaScript***相关，后者有多种风格(包括 Web 和 ***Node.JS***)


# 为什么使用 ***Rust*** 和 WebAssembly
## low-level 控制 与 high-level 人机工程学
***JavaScript*** Web 应用程序很难获得并保持可靠的性能。***JavaScript*** 的动态类型系统和垃圾收集器暂停没有帮助。如果您不小心偏离了 **JIT** 的快乐之路，看似很小的代码更改可能会导致性能急剧下降。   
***Rust*** 为程序员提供了低阶控制和可靠的性能。它没有困扰 ***JavaScript*** 的不确定性垃圾收集暂停。程序员可以控制间接寻址、单态化和内存布局。

## 小 `.wasm` 尺寸
代码大小非常重要，自从 `.wasm` 必须通过网络下载。***Rust*** 缺少一个运行时，支持小型 `.wasm` 尺寸，是因为没有像垃圾收集器那样额外的内容。您只需为实际使用的函数“付费”(按代码大小)。

## *不用* 将所有东西重写
现有的代码库不需要丢弃。您可以首先将性能最敏感的 ***JavaScript*** 函数 移植到 ***Rust***，以立即获得好处。如果你愿意，你甚至可以停在那里。

## 和其他技术玩得来(兼容性很好)
***Rust*** 和 WebAssembly 与现有 ***JavaScript*** 工具集成。它支持 ***ECMAScript*** 模块，您可以继续使用您已经喜欢的工具，如 ***npm*** 和 ***Webpack***.

## 你期望的便利
***Rust*** 拥有程序员期望的现代便利，如：
- 强大的包管理器 - `cargo` 
- 表达(和零成本)抽象
- 和一个好客的社区