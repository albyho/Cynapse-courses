# FFI API 速查表

库文件：macOS `libagent_desktop_ffi.dylib`、Linux `libagent_desktop_ffi.so`、Windows `agent_desktop_ffi.dll`。头文件：`crates/ffi/include/agent_desktop.h`。

## 两层入口

| 层 | 函数 | 返回 | 释放 |
| --- | --- | --- | --- |
| JSON | `ad_snapshot` | `char **out`，成功与命令级失败都写 envelope | `ad_free_string(out)` |
| JSON | `ad_execute_by_ref`、`ad_execute_by_ref_timeout` | 同上 | `ad_free_string(out)` |
| JSON | `ad_wait` | 同上 | `ad_free_string(out)` |
| JSON | `ad_status`、`ad_version` | 同上 | `ad_free_string(out)` |
| JSON | `ad_trace_show`、`ad_trace_export` | 同上 | `ad_free_string(out)` |
| 结构体 | `ad_list_windows_exact` | `AdExactWindowInfo` 列表 | `ad_exact_window_list_free(list)` |
| 结构体 | `ad_list_surfaces_exact` | surface 列表 | `ad_exact_surface_list_free(list)` |
| 结构体 | `ad_launch_app_exact` | 结构体里的字符串 | `ad_release_exact_window_fields(&out)` |
| 结构体 | `ad_get_tree_exact` | 完整树 | `ad_free_tree(&out)` |
| 结构体 | `ad_find_exact` | 原生句柄 | `ad_free_handle(adapter, &handle)` |
| 结构体 | `ad_execute_action`、`ad_execute_action_with_policy` | 动作结果 | `ad_free_action_result(&out)` |
| 结构体 | `ad_notification_action` | 动作结果 | `ad_free_action_result(&out)` |

其余 `*_exact` 家族（窗口、树、查找、权限、剪贴板、鼠标、显示器、通知、截图）遵循同一模式：调用方提供 out 结构体，结构体内的字符串归结构体所有，用对应的 release/free 一次释放。

## 列表与缓冲

| 分配函数 | 释放函数 |
| --- | --- |
| `ad_list_apps` | `ad_app_list_free(list)` |
| `ad_list_displays` | `ad_display_list_free(list)` |
| `ad_list_notifications` | `ad_notification_list_free(list)` |
| `ad_dismiss_all_notifications` | `ad_dismiss_all_notifications_free(dismissed, failed)` |
| `ad_screenshot`、`ad_screenshot_window_exact` | `ad_image_buffer_free(buf)` |
| `ad_get`、`ad_get_clipboard` | `ad_free_string(text)` |
| `ad_adapter_create`、`ad_adapter_create_with_session` | `ad_adapter_destroy(adapter)` |

## 错误访问器

| 访问器 | 返回 |
| --- | --- |
| `ad_last_error_code()` | 最近一次失败的代码，没有失败时为 `AD_RESULT_OK` |
| `ad_last_error_message()` | 描述，或 null |
| `ad_last_error_suggestion()` | 恢复建议，或 null |
| `ad_last_error_platform_detail()` | 平台原生错误文本，或 null |
| `ad_last_error_details()` | 结构化 JSON，或 null；可能包含屏幕文本 |
| `ad_last_error_delivery_semantics(&sem)` | 写入最近失败的 delivery / retry；没有记录时都是 `UNKNOWN` |

这些字符串是借用指针，跨越后续成功调用仍然有效，只有下一次失败才会替换；不要释放。

## 所有权与线程规则

- 所有由 Rust 分配的指针都不能用 C 的 `free()`；每个都有配对的释放函数。
- 所有释放函数接受 null；重复释放 list 或 image buffer 是未定义行为，释放后把指针置为 `NULL`。
- `ad_free_handle` 会把句柄清零，第二次调用是无操作；句柄必须在销毁产生它的 adapter 之前释放。
- 结构体内的字符串归结构体所有，不要逐个释放。
- 每个可失败函数在校验参数前先把 out 参数清零，所以失败后调用配对释放函数是安全的。
- 任意宿主线程都可以调用入口函数；原生句柄必须在解析它的同一线程、用同一个 adapter 使用和释放，否则返回 `AD_RESULT_ERR_INVALID_ARGS`。
- 读与读可以并发；读与变更可以重叠，读结果是某一时刻的观察；变更与变更由交互锁串行化，跨线程、跨进程、同一用户共享。
- `ad_adapter_destroy` 之后，已经在飞行中的调用会完成，新调用返回 `AD_RESULT_ERR_INVALID_ARGS`。

## ABI

`AD_ABI_VERSION_MAJOR` 当前是 7。删除符号、修改 `repr(C)` 布局、或让原本成功的调用开始失败，才升主版本；新增函数和错误码是加法。加载后调用一次 `ad_init(AD_ABI_VERSION_MAJOR)`，不匹配返回 `AD_RESULT_ERR_INVALID_ARGS`，此后不应再调用任何函数。
