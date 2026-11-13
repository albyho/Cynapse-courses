# Cynapse 课程

[English](README.md) | 简体中文

面向 [Cynapse](https://github.com/albyho/Cynapse-release) 的干净、仅运行时
Course Package。每个目录都是自包含的，可以直接用 Cynapse Client 打开；作者工具、
测试、Source Map 和本地运行状态不会发布到这里。

## 课程

| 课程 | 版本 | 课程包 |
| --- | --- | --- |
| 通过例子学 Rust（中文版） | `0.20.15` | [`rust-by-example/`](rust-by-example/) |

## 使用课程

1. 安装 [Cynapse Client](https://github.com/albyho/Cynapse-release/releases)
   和 Cynapse VS Code Adapter。
2. 在 Cynapse Client 中打开课程目录。
3. 将每个课程包的 `workspace/` 保留在本地。它保存运行状态，本仓库不会跟踪其中的
   内容。

`rust-by-example` 还需要 Rust `1.85.0` 或更高版本（推荐使用 `rustup`）。

## 课程包结构

```text
<课程>/
├── manifest.json      课程树与 Asset 登记
├── steps/             Step JSON
├── seeds/             Unit 初始文件
├── assets/narration/  共享 Ogg 旁白音轨
└── workspace/         运行时 Workspace（本仓库中为空）
```

## 添加课程

在 Cynapse 源码仓库中使用辅助脚本发布源课程包：

```powershell
python courses/publish_course.py courses/<课程> D:\dev\alby\Cynapse-courses
```

脚本只复制运行时内容，跳过 `authoring/`、`tests/`、生成的字幕报告、缓存和
`workspace/` 运行状态。

## 来源与许可

Rust 课程改编自
[Rust by Example（中文版）](https://github.com/rust-lang-cn/rust-by-example-cn)，
页面观察日期为 2026-11-03，按 MIT OR Apache-2.0 许可分发。详见
[`NOTICE`](NOTICE)、[`LICENSE-MIT`](LICENSE-MIT) 和
[`LICENSE-APACHE`](LICENSE-APACHE)。
