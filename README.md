# Cynapse Courses

English | [简体中文](README.zh-CN.md)

Clean, runtime-only [Cynapse](https://github.com/albyho/Cynapse-release) Course
Packages. Each directory is self-contained and can be opened directly by the
Cynapse Client; authoring notes, tests, source maps, and local runtime state are
not published here.

## Courses

| Course | Version | Package |
| --- | --- | --- |
| 通过例子学 Rust（中文版） | `0.20.15` | [`rust-by-example/`](rust-by-example/) |

## Using a course

1. Install [Cynapse Client](https://github.com/albyho/Cynapse-release/releases)
   and the Cynapse VS Code Adapter.
2. Open the course directory in the Client.
3. Keep each package's `workspace/` directory local. It contains runtime state
   and is intentionally not tracked by this repository.

`rust-by-example` also requires Rust `1.85.0` or newer (`rustup` is recommended).

## Package layout

```text
<course>/
├── manifest.json      Course Tree and asset registry
├── steps/             Step JSON artifacts
├── seeds/             Unit starting files
├── assets/narration/  Shared Ogg narration tracks
└── workspace/         Runtime Workspace (empty in this repository)
```

## Adding a course

Publish a source package with the helper from the Cynapse source repository:

```powershell
python courses/publish_course.py courses/<course> D:\dev\alby\Cynapse-courses
```

The helper copies only runtime content and skips `authoring/`, `tests/`,
generated narration reports, caches, and `workspace/` state.

## Source and license

The Rust course is adapted from
[Rust by Example (Chinese)](https://github.com/rust-lang-cn/rust-by-example-cn),
observed on 2026-11-03, and is distributed under the MIT OR Apache-2.0 license.
See [`NOTICE`](NOTICE), [`LICENSE-MIT`](LICENSE-MIT), and
[`LICENSE-APACHE`](LICENSE-APACHE).
