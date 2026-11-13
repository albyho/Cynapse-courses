# 简介 · 通过例子学 Rust（中文版）

[Rust](https://www.rust-lang.org/) 是一门注重安全（safety）、速度（speed）和并发（concurrency）的现代系统编程语言。Rust 通过内存安全来实现以上目标，但不使用垃圾回收机制（garbage collection, GC）。

《通过例子学 Rust》（Rust By Example, RBE）内容由一系列可运行的实例组成，通过这些例子阐明了各种 Rust 的概念和基本库。想获取这些例子外的更多内容，不要忘了[安装 Rust 到本地](https://www.rust-lang.org/tools/install)并查阅[官方标准库文档](https://rustwiki.org/zh-CN/std/)。另外为了满足您的好奇心，您还可以[查阅本网站的源代码](https://github.com/rust-lang-cn/rust-by-example-cn)。

现在让我们开始学习吧！

## 全书目录

- 第 1 章：[Hello World](https://rustwiki.org/zh-CN/rust-by-example/hello.html) - 从经典的 “Hello World” 程序开始学习。
- 第 2 章：[原生类型](https://rustwiki.org/zh-CN/rust-by-example/primitives.html) - 学习有符号整型，无符号整型和其他原生类型。
- 第 3 章：[自定义类型](https://rustwiki.org/zh-CN/rust-by-example/custom_types.html) - 结构体 `struct` 和 枚举 `enum`。
- 第 4 章：[变量绑定](https://rustwiki.org/zh-CN/rust-by-example/variable_bindings.html) - 变量绑定，作用域，变量遮蔽。
- 第 5 章：[类型系统](https://rustwiki.org/zh-CN/rust-by-example/types.html) - 学习改变和定义类型。
- 第 6 章：[类型转换](https://rustwiki.org/zh-CN/rust-by-example/conversion.html)
- 第 7 章：[表达式](https://rustwiki.org/zh-CN/rust-by-example/expression.html)
- 第 8 章：[流程控制](https://rustwiki.org/zh-CN/rust-by-example/flow_control.html) - `if`/`else`，`for`，以及其他流程控制有关内容。
- 第 9 章：[函数](https://rustwiki.org/zh-CN/rust-by-example/fn.html) - 学习方法、闭包和高阶函数。
- 第 10 章：[模块](https://rustwiki.org/zh-CN/rust-by-example/mod.html) - 使用模块来组织代码。
- 第 11 章：[Crate](https://rustwiki.org/zh-CN/rust-by-example/crates.html) - crate 是 Rust 中的编译单元。学习创建一个库。
- 第 12 章：[Cargo](https://rustwiki.org/zh-CN/rust-by-example/cargo.html) - 学习官方的 Rust 包管理工具的一些基本功能。
- 第 13 章：[属性](https://rustwiki.org/zh-CN/rust-by-example/attribute.html) - 属性是应用于某些模块、crate 或项的元数据（metadata）。
- 第 14 章：[泛型](https://rustwiki.org/zh-CN/rust-by-example/generics.html) - 学习编写能够适用于多种类型参数的函数或数据类型。
- 第 15 章：[作用域规则](https://rustwiki.org/zh-CN/rust-by-example/scope.html) - 作用域在所有权（ownership）、借用（borrowing）和生命周期（lifetime）中起着重要作用。
- 第 16 章：[特性 trait](https://rustwiki.org/zh-CN/rust-by-example/trait.html) - trait 是对未知类型（`Self`）定义的方法集。
- 第 17 章：[宏](https://rustwiki.org/zh-CN/rust-by-example/macros.html)
- 第 18 章：[错误处理](https://rustwiki.org/zh-CN/rust-by-example/error.html) - 学习 Rust 语言处理失败的方式。
- 第 19 章：[标准库类型](https://rustwiki.org/zh-CN/rust-by-example/std.html) - 学习 `std` 标准库提供的一些自定义类型。
- 第 20 章：[标准库更多介绍](https://rustwiki.org/zh-CN/rust-by-example/std_misc.html) - 更多关于文件处理、线程的自定义类型。
- 第 21 章：[测试](https://rustwiki.org/zh-CN/rust-by-example/testing.html) - Rust 语言的各种测试手段。
- 第 22 章：[不安全操作](https://rustwiki.org/zh-CN/rust-by-example/unsafe.html)
- 第 23 章：[兼容性](https://rustwiki.org/zh-CN/rust-by-example/compatibility.html)
- 第 24 章：[补充](https://rustwiki.org/zh-CN/rust-by-example/meta.html) - 文档和基准测试
