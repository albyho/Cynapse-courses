# 第 5 章 可观测与恢复：trace、回放与会话

## 本章要回答什么

一个动作失败以后，怎么知道当时到底发生了什么？进程崩溃、终端断开、ref 过期之后，怎么恢复？

三个问题：

1. trace 记录了哪些东西，写到哪里，哪些内容会被脱敏？
2. 回放是怎么把多个 segment 合成一条时间线的？
3. 会话、租约和状态目录怎么保证"同一时间只有一个动作在操作真实输入"？

前四章讲系统怎么工作；这一章讲它怎么被观察、怎么从故障里回来。

## trace 记录什么

trace 不是日志字符串，而是结构化事件：

- 命令、动作、渲染后的资源；
- 派发前后的状态、校验结果；
- 截图、refmap 等 artifact；
- 每一步的时间与序号。

写入前会经过 `sanitize_trace_value`：敏感字段递归脱敏，普通字段保持原样。trace 文件有大小上限，超出会截断而不是无限增长。

## 回放怎么工作

一次会话可能产生多个 trace segment。读取时：

1. 按文件名解析 segment 顺序；
2. 合并成一条时间线；
3. 标注来源、检测未配对的命令；
4. 按需截断尾部、按事件前缀过滤。

导出 HTML 时，viewer 负责把事件渲染成可浏览的页面：点击某一行，详情面板显示对应事件。

## 会话与恢复

会话不是"打开一个进程"，而是一组持久状态：

- `session.json` 记录 id、创建/结束时间、trace 模式、artifact 模式；
- 会话目录保存 trace、截图、refmap；
- GC 清理已经结束的会话；
- `InteractionLease` 保证桌面变更串行化：拿不到租约就等待或失败，而不是并发乱点。

恢复的边界也在这里：进程崩溃后，manifest 和租约能判断哪些会话还活着；过期的 ref 则按第 1 章的规则拒绝。

## 三节的分工

| 小节 | 回答的问题 | 主要代码 |
| --- | --- | --- |
| 5.1 记录 | trace 写什么、怎么脱敏、artifact 怎么存 | `trace/mod.rs`、`trace_sanitize.rs`、`trace_artifacts.rs` |
| 5.2 回放 | segment 怎么合并、HTML 怎么导出 | `trace/trace_read/`、`viewer.js` |
| 5.3 会话 | manifest、GC、交互租约 | `session/mod.rs`、`manifest.rs`、`interaction_lease.rs` |

## 阅读策略

- 先看 trace 的写入入口，再看读取/合并，最后看会话与租约；
- 不逐行读 viewer.js，先看它如何接收事件、如何选中一行；
- 看到 `sanitize`、`budget`、`lease` 时，先问"它在防止哪种事故"。

## 读完之后

你应该能：

- 说清 trace 事件与 artifact 的区别；
- 解释 segment 合并与 HTML 导出的过程；
- 说明 session manifest 与交互租约各自保证什么；
- 判断一次故障属于"没有记录"、"记录不可信"还是"状态未恢复"。
