# 平台逐命令能力矩阵

图例：

- `✓` 默认（headless）可用；
- `H` 需要 `--headed`，否则 `POLICY_DENIED` 或等价拒绝；
- `△` 有条件可用，见备注；
- `✕` 关闭或该平台不支持，返回 `ACTION_NOT_SUPPORTED` / `PLATFORM_NOT_SUPPORTED` / `INVALID_ARGS`。

矩阵来自各平台文档与 skill 的能力说明；Linux 分 X11 与 Wayland 两列。

## 观察与截图

| 命令 | macOS | Windows | Linux X11 | Linux Wayland | 备注 |
| --- | --- | --- | --- | --- | --- |
| `snapshot` | ✓ | ✓ | ✓ | ✓ | Chromium 首次读取可能只有部分树，需更长 `--timeout-ms` |
| `find` | ✓ | ✓ | ✓ | ✓ | 只在选定的 surface / root 内搜索 |
| `get` | ✓ | ✓ | ✓ | ✓ | |
| `is` | ✓ | ✓ | ✓ | ✓ | |
| `list-surfaces` | ✓ | ✓ | ✓ | ✓ | |
| `list-displays` | ✓ | ✓ | ✓ | △ | Wayland 的显示器枚举受限 |
| `screenshot` | ✓ | ✓ | △ | △ | macOS 需屏幕录制权限；X11 合成窗口管理器下显示器截图不支持，可截单窗口；Wayland 窗口目标不支持 |
| `open-system-surface` | △ | H | H | H | macOS 仅 `dock` / `menu-bar-extras`；Windows 需 headed；Linux 仅部分 shell kind |

## 引用动作

| 命令 | macOS | Windows | Linux X11 | Linux Wayland | 备注 |
| --- | --- | --- | --- | --- | --- |
| `click` | ✓ | ✓ | ✓ | ✓ | Linux 元素必须暴露 AT-SPI 动作；否则拒绝而不回退物理输入 |
| `double-click` | △ | H | H | ✕ | macOS 仅在元素提供 `AXOpen` 时 headless |
| `triple-click` | H | H | H | ✕ | |
| `right-click` | ✓ | H | ✓ | ✓ | macOS headless 可用 `AXShowMenu` |
| `type` | ✓ | H | ✓ | ✓ | Windows headless 用 `set-value` 代替；Linux 终端文本节点需 headed |
| `set-value` | ✓ | ✓ | ✓ | ✓ | |
| `clear` | ✓ | ✓ | ✓ | ✓ | |
| `focus` | ✓ | H | H | H | |
| `select` | ✓ | ✓ | △ | △ | 部分 Qt / libadwaita 组合需要 headed |
| `toggle` | ✓ | ✓ | ✓ | ✓ | |
| `check` | ✓ | ✓ | ✓ | ✓ | |
| `uncheck` | ✓ | ✓ | ✓ | ✓ | |
| `expand` | ✓ | ✓ | ✓ | ✓ | |
| `collapse` | ✓ | ✓ | ✓ | ✓ | |
| `scroll` | ✓ | ✓ | H | ✕ | Linux 的 headless `scroll` 被 `POLICY_DENIED` |
| `scroll-to` | ✓ | ✓ | ✓ | ✓ | |

## 指针与键盘

| 命令 | macOS | Windows | Linux X11 | Linux Wayland | 备注 |
| --- | --- | --- | --- | --- | --- |
| `press` | ✓ | ✓ | ✓ | △ | Linux 的 scoped headless `press` 需要目标窗口已激活；Wayland 依赖 Mutter RemoteDesktop |
| `key-down` | ✕ | ✕ | ✕ | ✕ | 四个 held-input 命令在所有平台 fail closed |
| `key-up` | ✕ | ✕ | ✕ | ✕ | |
| `mouse-down` | ✕ | ✕ | ✕ | ✕ | |
| `mouse-up` | ✕ | ✕ | ✕ | ✕ | |
| `hover` | H | H | H | ✕ | |
| `drag` | H | H | H | ✕ | |
| `mouse-move` | H | H | H | ✕ | |
| `mouse-click` | H | H | H | ✕ | |
| `mouse-wheel` | H | H | H | ✕ | Linux 滚轮滚动仅 X11 |

## 应用与窗口

| 命令 | macOS | Windows | Linux X11 | Linux Wayland | 备注 |
| --- | --- | --- | --- | --- | --- |
| `launch` | ✓ | ✓ | ✓ | ✓ | Windows 只接受路径或 System32 下的名字，不解析显示名 |
| `close-app` | ✓ | ✓ | ✓ | ✕ | Windows 对 `explorer.exe` 等受保护映像返回 `INVALID_ARGS` |
| `list-apps` | ✓ | ✓ | ✓ | ✓ | |
| `list-windows` | ✓ | ✓ | ✓ | ✓ | |
| `focus-window` | ✓ | ✓ | ✓ | ✕ | Wayland 返回 `ACTION_NOT_SUPPORTED` |
| `resize-window` | ✓ | ✓ | ✓ | ✕ | |
| `move-window` | ✓ | ✓ | ✓ | ✕ | |
| `minimize` | ✓ | ✓ | ✓ | ✕ | |
| `maximize` | ✓ | ✓ | ✓ | ✕ | |
| `restore` | ✓ | ✓ | ✓ | ✕ | |

## 通知、剪贴板、等待与系统

| 命令 | macOS | Windows | Linux X11 | Linux Wayland | 备注 |
| --- | --- | --- | --- | --- | --- |
| `list-notifications` | △ | △ | ✓ | △ | macOS / Windows 仅当通知中心已打开时 headless 可列；Linux 仅 GNOME Shell |
| `dismiss-notification` | H | H | H | △ | Linux 每次 dismiss / action 都要 headed；非 GNOME Shell 不支持 |
| `dismiss-all-notifications` | H | H | H | △ | |
| `notification-action` | H | H | H | △ | |
| `clipboard-get` | ✓ | ✓ | ✓ | ✓ | Linux 缺 helper / `wl-clipboard` 时 `PLATFORM_NOT_SUPPORTED` |
| `clipboard-set` | ✓ | ✓ | ✓ | ✓ | 同上 |
| `clipboard-clear` | ✓ | ✓ | ✓ | ✓ | |
| `wait` | ✓ | ✓ | ✓ | ✓ | `--notification`：macOS 仅已打开的通知中心；Windows 关闭时需 headed |
| `status` | ✓ | ✓ | ✓ | ✓ | |
| `permissions` | ✓ | ✓ | ✓ | ✓ | Windows 的 `--guide` 返回 `PLATFORM_NOT_SUPPORTED` |
| `version` | ✓ | ✓ | ✓ | ✓ | |
| `batch` | ✓ | ✓ | ✓ | ✓ | 语义与平台无关；条目能力仍按本表 |
| `skills` | ✓ | ✓ | ✓ | ✓ | |
| `session` | ✓ | ✓ | ✓ | ✓ | |
| `cursor-overlay` | ✓ | ✓ | ✓ | △ | Wayland 保存后 `rendered: false` |
| `trace` | ✓ | ✓ | ✓ | ✓ | |

## 怎么读这张表

1. 先看命令所在组：观察类基本全平台；动作类受交互策略限制；窗口控制受 Wayland 限制。
2. `H` 不是"更可靠"，而是"允许物理输入或抢焦点"；不要为了绕过错误自行加 `--headed`。
3. `✕` 是设计上的 fail closed，不是缺陷；换命令或换平台，而不是重试。
4. Linux 的 `△` 多半取决于元素是否暴露 AT-SPI 能力、会话是 X11 还是 Wayland、桌面是否为 GNOME。
