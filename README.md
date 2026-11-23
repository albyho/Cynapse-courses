# Cynapse Courses

English | [简体中文](README.zh-CN.md)

Clean, runtime-only [Cynapse](https://github.com/albyho/Cynapse) Course
Packages. Authoring notes, tests, source maps, and local runtime state are not
published here. Course resources may be normal files or pinned git submodules.

## Courses

| Course | Version | Package | Based on |
| --- | --- | --- | --- |
| Rust by Example (Chinese) | `0.20.21` | [zip](https://github.com/albyho/Cynapse-courses/releases/tag/rust-by-example-zh-cn-0.20.21) | [Rust by Example (Chinese)](https://rustwiki.org/zh-CN/rust-by-example/) |
| agent-desktop Source Analysis (Chinese) | `0.39.0` | [zip](https://github.com/albyho/Cynapse-courses/releases/tag/agent-desktop-source-analysis-zh-cn-0.39.0) | [agent-desktop](https://github.com/vercel-labs/agent-desktop) |
| Signal Processing Roadmap (Chinese) | `0.2.4` | [zip](https://github.com/albyho/Cynapse-courses/releases/tag/signal-processing-roadmap-zh-cn-0.2.4) | [Signal processing roadmap](https://github.com/minthanthtoo/signal-processing-roadmap) |
| Signal Processing Roadmap (English) | `0.2.4` | [zip](https://github.com/albyho/Cynapse-courses/releases/tag/signal-processing-roadmap-0.2.4) | [Signal processing roadmap](https://github.com/minthanthtoo/signal-processing-roadmap) |

> `rust-by-example-zh-cn` is an in-progress adaptation. It currently covers the
> introduction and chapters 1–4 only, not the complete Rust by Example book.
>
> `signal-processing-roadmap-zh-cn` is an in-progress adaptation. It currently
> covers the introduction and Week 1 only, not the complete roadmap.
>
> `signal-processing-roadmap` is the English edition of the same course: one
> Course Tree, Step and Cell ID set, and narration timing, with English copy and
> an English voice track. It also covers only the introduction and Week 1. The
> two packages keep separate progress because their root `manifest.id` differs.

## Using a course

Install [Cynapse Client](https://github.com/albyho/Cynapse/releases) and
the matching Cynapse Adapter (VS Code or JupyterLab) first. Keep each package's
`workspace/` directory local; it contains runtime state and is intentionally not
tracked here.

Each course also has a downloadable zip linked from the Package column above.
Extract it to get a `<course-name>/` directory and open that directory in the
Cynapse Client. Cloning this repository is only needed if you want the full set
or the pinned resource submodules.

### rust-by-example-zh-cn

No submodules are required. Clone the repository and open the course directory:

```powershell
git clone https://github.com/albyho/Cynapse-courses.git
```

Open `Cynapse-courses/rust-by-example-zh-cn` in the `Cynapse Client`. It requires
Rust `1.85.0` or newer (`rustup` is recommended).

### agent-desktop-source-analysis-zh-cn

This course reads the official
[`vercel-labs/agent-desktop`](https://github.com/vercel-labs/agent-desktop)
source through a pinned submodule. Clone with submodules:

```powershell
git clone --recurse-submodules https://github.com/albyho/Cynapse-courses.git
```

If the repository is already cloned, initialize the submodule:

```powershell
git submodule update --init --recursive
```

Open `Cynapse-courses/agent-desktop-source-analysis-zh-cn` in the `Cynapse Client`.
The submodule is pinned to commit `b43ac4f6e892ef608f8851428613c4009f0e84c7`; you
may instead download that revision yourself and place it at
`agent-desktop-source-analysis-zh-cn/resources/agent-desktop`.

### signal-processing-roadmap-zh-cn

This course runs in JupyterLab and requires the Cynapse JupyterLab Adapter. No
submodules are required. Open
`Cynapse-courses/signal-processing-roadmap-zh-cn` in the `Cynapse Client`; each
Unit keeps its own Notebook, Kernel, and Workspace. It is adapted from
[Signal processing roadmap](https://github.com/minthanthtoo/signal-processing-roadmap)
and currently covers the introduction and Week 1 only.

### signal-processing-roadmap

The English edition of `signal-processing-roadmap-zh-cn`. It runs in JupyterLab
and requires the Cynapse JupyterLab Adapter; no submodules are required. Open
`Cynapse-courses/signal-processing-roadmap` in the `Cynapse Client`. Node Paths,
`step_id`s, Cell IDs and narration timing match the Chinese package, so a Step
sits at the same place in both; only the copy, the code comments and the voice
track differ. Progress is tracked per package, so switching editions does not
carry progress across.

### Updating

```sh
git pull --ff-only
git submodule sync --recursive
git submodule update --init --recursive
```

Verify the agent-desktop revision before opening the course:

```sh
git -C agent-desktop-source-analysis-zh-cn/resources/agent-desktop rev-parse HEAD
# b43ac4f6e892ef608f8851428613c4009f0e84c7
```

## Package layout

```text
<course>/
├── manifest.json      Course Tree and asset registry
├── steps/             Step JSON artifacts
├── seeds/             Unit starting files
├── assets/narration/  Shared Ogg narration tracks
├── resources/         Optional resources (files or pinned submodules)
└── workspace/         Runtime Workspace (empty in this repository)
```

## Source and license

The Rust course is adapted from
[Rust by Example (Chinese)](https://rustwiki.org/zh-CN/rust-by-example/),
observed on 2026-11-03. Its upstream source and license are
[rust-lang-cn/rust-by-example-cn](https://github.com/rust-lang-cn/rust-by-example-cn)
(MIT OR Apache-2.0).
The agent-desktop course links its upstream source as a pinned submodule and
keeps that repository's Apache-2.0 license. The signal-processing courses
(Chinese and English) reference
[Signal processing roadmap](https://github.com/minthanthtoo/signal-processing-roadmap)
for its chapter structure and topics; its lessons and notebooks are original.
See [`NOTICE`](NOTICE), [`LICENSE-MIT`](LICENSE-MIT), and
[`LICENSE-APACHE`](LICENSE-APACHE).
