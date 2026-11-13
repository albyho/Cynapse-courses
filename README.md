# Cynapse Courses

English | [简体中文](README.zh-CN.md)

Clean, runtime-only [Cynapse](https://github.com/albyho/Cynapse-release) Course
Packages. Authoring notes, tests, source maps, and local runtime state are not
published here. External course resources are pinned as git submodules.

## Courses

| Course | Version | Package |
| --- | --- | --- |
| 通过例子学 Rust（中文版） | `0.20.15` | [`rust-by-example/`](rust-by-example/) |
| agent-desktop 源码分析 | `0.39.0` | [`agent-desktop-source-analysis/`](agent-desktop-source-analysis/) |

## Using a course

Install [Cynapse Client](https://github.com/albyho/Cynapse-release/releases) and
the Cynapse VS Code Adapter first. Keep each package's `workspace/` directory
local; it contains runtime state and is intentionally not tracked here.

### rust-by-example

No submodules are required. Clone the repository and open the course directory:

```powershell
git clone https://github.com/albyho/Cynapse-courses.git
```

Open `Cynapse-courses/rust-by-example` in the `Cynapse Client`. It requires
Rust `1.85.0` or newer (`rustup` is recommended).

### agent-desktop-source-analysis

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

Open `Cynapse-courses/agent-desktop-source-analysis` in the `Cynapse Client`.
The submodule is pinned to commit `b43ac4f6e892ef608f8851428613c4009f0e84c7`; you
may instead download that revision yourself and place it at
`agent-desktop-source-analysis/resources/agent-desktop`.

### Updating

```sh
git pull --ff-only
git submodule sync --recursive
git submodule update --init --recursive
```

Verify the agent-desktop revision before opening the course:

```sh
git -C agent-desktop-source-analysis/resources/agent-desktop rev-parse HEAD
# b43ac4f6e892ef608f8851428613c4009f0e84c7
```

## Package layout

```text
<course>/
├── manifest.json      Course Tree and asset registry
├── steps/             Step JSON artifacts
├── seeds/             Unit starting files
├── assets/narration/  Shared Ogg narration tracks
├── resources/         Optional external resources (pinned submodules)
└── workspace/         Runtime Workspace (empty in this repository)
```

## Adding a course

Publish a source package with the helper from the Cynapse source repository:

```powershell
python courses/publish_course.py courses/<course> D:\dev\alby\Cynapse-courses
```

The helper copies only runtime content and skips `authoring/`, `tests/`,
generated narration reports, caches, and `workspace/` state. Git repositories
under `resources/` are left out; the helper prints their remote URL and pinned
commit so they can be added as submodules.

## Source and license

The Rust course is adapted from
[Rust by Example (Chinese)](https://github.com/rust-lang-cn/rust-by-example-cn),
observed on 2026-11-03, and is distributed under the MIT OR Apache-2.0 license.
The agent-desktop course links its upstream source as a pinned submodule and
keeps that repository's Apache-2.0 license. See [`NOTICE`](NOTICE),
[`LICENSE-MIT`](LICENSE-MIT), and [`LICENSE-APACHE`](LICENSE-APACHE).
