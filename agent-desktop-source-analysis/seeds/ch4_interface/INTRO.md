# 第 4 章 对外接口：CLI、C ABI 与 MCP

## 本章要回答什么

前两章讲的是 core 内部；这一章讲它怎么被外面调用。

三个问题：

1. CLI 怎么解析参数、选择命令、输出 JSON？
2. C ABI 怎么把同一套能力暴露给其他语言？
3. MCP 怎么把每个命令变成 agent 可调用的 tool？

三者的共同点是：**同一套命令、同一个 JSON envelope**。差别只在传输和调用方式。

## 三种形态，一个内核

```text
             ┌── CLI（argv / stdout）
core 命令 ───┼── C ABI（cdylib / FFI）
             └── MCP（stdio / JSON-RPC）
```

- **CLI**：人调试、脚本调用；
- **C ABI**：Python / Swift / Go / Node / C++ 在进程内调用；
- **MCP**：编码 agent 通过 tool 调用，每次调用仍然落到 CLI 命令上。

## CLI 的路径

`src/main.rs` 的主线很短：

1. 处理平台特殊的子进程入口（光标层、权限 helper）；
2. `root_command()` 构建命令树，`mcp` 子命令单独处理；
3. `Cli::from_arg_matches` 解析；
4. `execute` 执行命令；
5. `finish` 输出 envelope 并决定退出码。

命令行参数、stdin、JSON 输出都在这一层收口。

## C ABI 的边界

C ABI 不是"再写一套命令"，而是把 adapter 暴露成 C 函数：

- `ad_init(expected_major)` 校验 ABI 主版本；
- `ad_adapter_create()` 创建 adapter，`ad_adapter_destroy()` 释放；
- 所有 `AdResult` 失败都写线程本地 last-error；
- 每个入口都有 `trap_panic`，panic 不跨过 C 边界。

三条硬规则：ABI 主版本不一致就不要继续调用；element handle 是线程绑定的；桌面变更由跨进程租约串行化。

## MCP 的路径

MCP 是 stdio 上的 JSON-RPC：

- 每个叶子命令对应一个同名 tool，例如 `desktop_snapshot`；
- 调用时把参数换算成 CLI 参数，再走同一条命令；
- 全局标志（`--session`、`--headed`、`--trace`）由启动方式固定，不能按调用修改；
- 支持取消：`notifications/cancelled` 会终止正在运行的子进程。

## 三节的分工

| 小节 | 回答的问题 | 主要代码 |
| --- | --- | --- |
| 4.1 CLI | 参数怎么变成命令、结果怎么变成 envelope | `src/main.rs` |
| 4.2 C ABI | 同一套能力怎么暴露给 C 调用方 | `crates/ffi/src/lib.rs`、`abi_version.rs`、`adapter.rs` |
| 4.3 MCP | 命令怎么变成 tool、怎么取消 | `src/mcp/mod.rs`、`catalog.rs`、`calls.rs` |

## 阅读策略

- 先看 CLI 的主线，再看 FFI 的边界规则，最后看 MCP 的协议层；
- 不逐行读 FFI 的每个导出函数，先看 ABI 版本与错误模型；
- 看到 `trap_panic`、`AdResult`、`last_error` 时，先问"失败怎么告诉调用方"。

## 读完之后

你应该能：

- 说清 CLI 从参数到 envelope 的路径；
- 解释 ABI 主版本校验和 last-error 的作用；
- 说明 MCP tool 调用最终落到哪里；
- 判断一个接口问题属于"传输层"还是"命令层"。
