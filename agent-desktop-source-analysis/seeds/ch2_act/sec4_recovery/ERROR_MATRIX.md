# 错误码与恢复速查表

先读 `error.code`，再读 `error.disposition.retry`，最后才看有没有 `error.recovery`。不要根据 `message` 或 `suggestion` 的措辞做分支。

## 16 个错误码

| code | 含义 | 是否带 recovery | 怎么处理 |
| --- | --- | --- | --- |
| `PERM_DENIED` | 缺少系统权限（辅助功能、屏幕录制等） | 否 | 运行 `permissions` 并授权；重试无效 |
| `ELEMENT_NOT_FOUND` | ref 或查询没有命中；或请求的 surface 不存在 | 否 | 读 `details.kind` / `details.surface`；菜单先打开再观察 |
| `APP_NOT_FOUND` | 没有匹配的应用进程 | 否 | `list-apps` 确认名称；必要时 `launch` |
| `ACTION_FAILED` | 动作被拒绝、阻塞，或观察结果与请求矛盾 | 否 | 读 `details` 与 `disposition.retry`；不要盲目重试 |
| `ACTION_NOT_SUPPORTED` | 目标或平台不支持该动作 | 否 | 换命令或换元素；重复同样失败 |
| `STALE_REF` | ref 不再匹配存活元素 | 是 | 刷新快照后重试；若 `details.process_state` 为 `exited`，改为先检查/重启应用 |
| `AMBIGUOUS_TARGET` | 多个元素、窗口或应用实例匹配，或捕获时就不唯一 | 否 | 刷新快照；用 `find`、`--root`、`--window-id` 收窄 |
| `WINDOW_NOT_FOUND` | 窗口不存在，或 `--window-id` 没有匹配 | 否 | `list-windows --app <name>` 后使用返回的 id |
| `PLATFORM_NOT_SUPPORTED` | 当前平台的 adapter 没实现该方法 | 否 | 换方案；平台页列出各自能力 |
| `TIMEOUT` | 等待超时、目标始终不可操作，或读取一直不完整 | 是 | 读 `details.kind`；`retry` 为 `safe` 才重试 |
| `INVALID_ARGS` | 参数非法；裸 `@eN`、已结束的 session 也归此类 | 否 | 修正输入；原样重试必然再失败 |
| `NOTIFICATION_NOT_FOUND` | 通知索引或身份已失效 | 否 | 重新 `list-notifications` |
| `SNAPSHOT_NOT_FOUND` | 快照 id 缺失、被清理，或属于另一个 session 命名空间 | 是 | 在同一 session 作用域重新快照 |
| `POLICY_DENIED` | headless 模式拦截了物理/headed 路径 | 是 | 只有人显式选择策略后才重试；不要自行切 headed |
| `APP_UNRESPONSIVE` | 应用停止响应 | 是 | 观察、等待 `retry_after_ms`，`retry` 为 `safe` 才重试 |
| `INTERNAL` | 未预期错误 | 否 | 读 `message`；`retry` 为 `safe` 时最多重试一次 |

## delivery → retry

| delivery | retry | 含义 |
| --- | --- | --- |
| `not_delivered` | `safe` | 输入没有到达应用，重复不会产生第二次效果 |
| `delivery_uncertain` | `unsafe` | 可能已经到达，但无法证明 |
| `delivered_unverified` | `unsafe` | 已派发，效果未确认 |
| `delivered_verified` | `unsafe` | 已派发且读回效果，重复会应用两次 |
| `unknown` | `unknown` | 没有建立任何投递结论 |

只有 `not_delivered` 是 `safe`。`unsafe` 或 `unknown` 时先观察，再决定是否继续。

## strategy → codes

| strategy | 适用错误码 | 调用方做什么 |
| --- | --- | --- |
| `refresh_snapshot_then_retry_original` | `STALE_REF`（进程未退出）、`SNAPSHOT_NOT_FOUND` | 重新快照或 `find`，定位同一元素，用新的限定 ref 重跑原命令 |
| `inspect_state_then_retry_original` | `TIMEOUT`、`APP_UNRESPONSIVE`、`STALE_REF` 且进程已退出 | 先 `list-apps`、快照或 `wait` 观察；有 `retry_after_ms` 就等待；进程退出则重启并重新快照 |
| `request_explicit_policy_then_retry_original` | `POLICY_DENIED` | 只有人明确选择所需策略后才重试 |

## 三个附加条件

1. `recovery` 只在 `disposition.retry` 为 `safe` 时出现；
2. `retry_after_ms` 只给 `TIMEOUT` 与 `APP_UNRESPONSIVE`，本版本是 250 ms；
3. adapter 可以标记 `details.retryable: false`，即使错误码本来有策略，也会抑制 `recovery`。

`ACTION_FAILED`、`INVALID_ARGS`、`AMBIGUOUS_TARGET` 等永远不带 `recovery`，按"怎么处理"一列修复。
