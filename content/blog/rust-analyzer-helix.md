---
title: "Rust tips #1: Custom target directory for rust-analyzer"
description:
  How to speed up builds when using `rust-analyzer` by specifying a custom
  target directory
date: 2025-05-28
tags: [rust, rust-analyzer, helix, tip]
---

Recently, while working on
[lancedb/lance#3572](https://github.com/lancedb/lance/pull/3572), I noticed that
builds were taking much longer than usual. Simply changing a single character in
one of the unit tests required 100+ crates to be recompiled when running
`cargo test`. While waiting for tests to compile, the following message would
appear:

```txt
Blocking waiting for file lock on build directory.
```

The culprit behind this message is
[`rust-analyzer`](https://rust-analyzer.github.io/), which I run as an LSP in
[Helix](https://helix-editor.com/). `rust-analyzer` compiles with different
settings to `cargo test`, but they both build to the same target directory.
Fortunately, we can easily direct `rust-analyzer` to build to a different target
directory by editing `~/.config/helix/languages.toml`:

```toml
[language-server.rust-analyzer.environment]
CARGO_TARGET_DIR = "target/analyzer"
```

See the
[rust-analyzer docs](https://rust-analyzer.github.io/book/configuration.html#cargo.targetDir)
for more information.
