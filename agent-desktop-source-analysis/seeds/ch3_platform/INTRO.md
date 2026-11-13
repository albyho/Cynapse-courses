# 第 3 章 平台适配：同一份核心，三套后端

## 为什么需要 PlatformAdapter

前两章的代码都在 `crates/core` 里，但 core 从来不直接调用操作系统 API。

它只通过一个接口接触平台：

```text
core  →  PlatformAdapter  →  windows / macos / linux
```

这样做的原因很实际：

- 读树、执行动作、注入输入、拿系统信息，在每个平台上都不同；
- 如果 core 里到处写 `#[cfg(target_os)]`，核心逻辑会被平台细节淹没；
- 测试时可以用一个假 Adapter 驱动整条链路，不需要真实桌面。

## 四个能力接口

`PlatformAdapter` 不是一个大接口，而是四个能力接口的组合：

| 接口 | 负责 | 典型方法 |
| --- | --- | --- |
| `ObservationOps` | 读树、列窗口、解析元素 | `observe_tree`、`list_windows`、`resolve_element_strict` |
| `ActionOps` | 执行语义动作、滚动到可见 | `execute_action`、`scroll_ref_into_view` |
| `InputOps` | 真实鼠标键盘事件 | `mouse_event`、`key_event`、`drag` |
| `SystemOps` | 权限、焦点、光标层、租约 | `permission_report`、`focus_window`、`acquire_interaction_lease` |

`PlatformAdapter` 本身只是一个空 trait，把所有组合起来：

```rust
pub trait PlatformAdapter: ObservationOps + ActionOps + InputOps + SystemOps {}
```

## 三个后端是同构的

打开三个平台 crate，会发现目录几乎一样：

```text
crates/windows/src/  actions  adapter  input  notifications  system  tree
crates/macos/src/    actions  adapter  input  notifications  system  tree
crates/linux/src/    actions  adapter  input  notifications  system  tree
```

这意味着：**读一个后端的方法，可以直接对照另外两个。** 差异在底层 API，不在文件结构：

- Windows：UI Automation；
- macOS：AX API；
- Linux：AT-SPI。

## 差异集中在哪里

| 维度 | Windows | macOS | Linux |
| --- | --- | --- | --- |
| 无障碍 API | UI Automation | AX API | AT-SPI |
| 权限 | 通常无需额外授权 | 辅助功能授权 | 依赖桌面环境 |
| 输入注入 | SendInput | CGEvent | X11 / Wayland |
| 窗口身份 | HWND + 创建时间 | window number + 进程启动时间 | X11 id + 启动时间 |

课程不逐行读三个后端，而是以 Windows 为主线，另外两个只看差异。

## 三节的分工

| 小节 | 回答的问题 |
| --- | --- |
| 3.1 契约 | `PlatformAdapter` 由哪些能力组成，core 依赖到什么程度 |
| 3.2 Windows 端到端 | 一个真实后端如何实现观察与动作 |
| 3.3 差异 | macOS / Linux 在哪些点上必须不同 |

## 阅读策略

- 先看 trait 的方法签名，再看 Windows 的 `impl` 块；
- 不逐行读平台细节，先找"和 core 对接的那几个函数"；
- 看到 `#[cfg(target_os)]` 时，先判断它是平台差异，还是编译期选择。

## 读完之后

你应该能：

- 说出四个能力接口各自负责什么；
- 沿着 Windows 后端，找到 `observe_tree` 和 `execute_action` 的实现；
- 解释为什么窗口身份不能只用句柄；
- 判断一个平台差异属于"接口不同"还是"行为不同"。
