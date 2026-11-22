# Cynapse 课程

[English](README.md) | 简体中文

面向 [Cynapse](https://github.com/albyho/Cynapse-release) 的干净、仅运行时
Course Package。作者工具、测试、Source Map 和本地运行状态不会发布到这里；
课程资源可以是普通文件，也可以是固定提交的 git submodule。

## 课程

| 课程 | 版本 | 课程包 | 来源 |
| --- | --- | --- | --- |
| 通过例子学 Rust（中文版） | `0.20.21` | [zip](https://github.com/albyho/Cynapse-courses/releases/tag/rust-by-example-zh-cn-0.20.21) | [Rust by Example（中文版）](https://github.com/rust-lang-cn/rust-by-example-cn) |
| agent-desktop 源码分析（中文版） | `0.39.0` | [zip](https://github.com/albyho/Cynapse-courses/releases/tag/agent-desktop-source-analysis-zh-cn-0.39.0) | [agent-desktop](https://github.com/vercel-labs/agent-desktop) |
| 信号处理路线图（中文版） | `0.2.4` | [zip](https://github.com/albyho/Cynapse-courses/releases/tag/signal-processing-roadmap-zh-cn-0.2.4) | [Signal processing roadmap](https://github.com/minthanthtoo/signal-processing-roadmap) |
| 信号处理路线图（英文版） | `0.2.4` | [zip](https://github.com/albyho/Cynapse-courses/releases/tag/signal-processing-roadmap-0.2.4) | [Signal processing roadmap](https://github.com/minthanthtoo/signal-processing-roadmap) |

> `rust-by-example-zh-cn` 目前只包含简介和第 1–4 章，并非完整的《通过例子学 Rust》；
> 其余章节尚未发布。

> `signal-processing-roadmap-zh-cn` 目前只包含简介和第 1 周（章），尚未覆盖完整路线图。

> `signal-processing-roadmap` 是同一门课的英文版：Course Tree、Step 与 Cell ID、
> 旁白区间都与中文包一致，只换了文案、代码注释和英文音色。它同样只覆盖简介和
> 第 1 周；两个包的根 `manifest.id` 不同，进度互不影响。

## 使用课程

先安装 [Cynapse Client](https://github.com/albyho/Cynapse-release/releases) 和
对应的 Cynapse Adapter（VS Code 或 JupyterLab）。每个课程包的 `workspace/` 保留在本地，
它保存运行状态，本仓库不会跟踪其中的内容。

上表「课程包」列提供每门课的 zip 下载。解压后得到 `<课程名>/` 目录，在 Cynapse
Client 中打开该目录即可。只有需要全部课程或固定资源 submodule 时才需要克隆本仓库。

### rust-by-example-zh-cn

不需要 submodule。克隆仓库后直接打开课程目录：

```powershell
git clone https://github.com/albyho/Cynapse-courses.git
```

在 Cynapse Client 中打开 `Cynapse-courses/rust-by-example-zh-cn`。还需要 Rust
`1.85.0` 或更高版本（推荐使用 `rustup`）。

### agent-desktop-source-analysis-zh-cn

本课程通过固定 submodule 读取
[`vercel-labs/agent-desktop`](https://github.com/vercel-labs/agent-desktop) 的
官方源码。克隆时初始化 submodule：

```powershell
git clone --recurse-submodules https://github.com/albyho/Cynapse-courses.git
```

已经克隆仓库时执行：

```powershell
git submodule update --init --recursive
```

在 Cynapse Client 中打开 `Cynapse-courses/agent-desktop-source-analysis-zh-cn`。
submodule 固定到提交 `b43ac4f6e892ef608f8851428613c4009f0e84c7`；也可以自行下载
该版本并放到 `agent-desktop-source-analysis-zh-cn/resources/agent-desktop`。

### signal-processing-roadmap-zh-cn

本课程在 JupyterLab 中运行，需要 Cynapse JupyterLab Adapter；不需要 submodule。
在 Cynapse Client 中打开 `Cynapse-courses/signal-processing-roadmap-zh-cn`。每个 Unit
使用独立的 Notebook、Kernel 和 Workspace。本课程改编自
[Signal processing roadmap](https://github.com/minthanthtoo/signal-processing-roadmap)，
目前只包含简介和第 1 周（章）。

### signal-processing-roadmap

`signal-processing-roadmap-zh-cn` 的英文版。在 JupyterLab 中运行，需要 Cynapse
JupyterLab Adapter；不需要 submodule。在 Cynapse Client 中打开
`Cynapse-courses/signal-processing-roadmap`。Node Path、`step_id`、Cell ID 和旁白
区间与中文包一一对应，同一个 Step 在两个包里位置相同，只有文案、代码注释和音色
不同。进度按课程包分别记录，切换语言不会带过去。

### 更新

```sh
git pull --ff-only
git submodule sync --recursive
git submodule update --init --recursive
```

打开课程前可以核对 agent-desktop 的提交：

```sh
git -C agent-desktop-source-analysis-zh-cn/resources/agent-desktop rev-parse HEAD
# b43ac4f6e892ef608f8851428613c4009f0e84c7
```

## 课程包结构

```text
<课程>/
├── manifest.json      课程树与 Asset 登记
├── steps/             Step JSON
├── seeds/             Unit 初始文件
├── assets/narration/  共享 Ogg 旁白音轨
├── resources/         可选资源（普通文件或固定 submodule）
└── workspace/         运行时 Workspace（本仓库中为空）
```

## 添加课程

在 Cynapse 源码仓库中使用辅助脚本发布源课程包：

```powershell
python courses/publish_course.py courses/<课程> D:\dev\alby\Cynapse-courses
```

脚本只复制运行时内容，跳过 `authoring/`、`tests/`、生成的字幕报告、缓存和
`workspace/` 运行状态。`resources/` 下的普通文件会正常复制；Git 仓库不会被复制，
脚本会打印其远程地址和固定提交，供后续以 submodule 方式引用。

## 来源与许可

Rust 课程改编自
[Rust by Example（中文版）](https://github.com/rust-lang-cn/rust-by-example-cn)，
页面观察日期为 2026-11-03，按 MIT OR Apache-2.0 许可分发。agent-desktop 课程以
固定 submodule 引用上游源码，并保留其 Apache-2.0 许可。信号处理课程参考
[Signal processing roadmap](https://github.com/minthanthtoo/signal-processing-roadmap)
的章节骨架和主题，讲解、Notebook 和练习为原创。详见
[`NOTICE`](NOTICE)、[`LICENSE-MIT`](LICENSE-MIT) 和
[`LICENSE-APACHE`](LICENSE-APACHE)。
