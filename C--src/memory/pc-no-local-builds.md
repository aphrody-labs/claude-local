---
name: pc-no-local-builds
description: No cargo/rustc/ninja builds or tests on the PC; rust-analyzer LSP must stay passive; compile on fr1/fr2
metadata:
  node_type: memory
  type: feedback
  originSessionId: aae12588-4a11-4aae-b28f-f3d886683cbb
  modified: 2026-11-17T04:09:57.029Z
---

Never run cargo, rustc, ninja, clang or any heavy build/test on the PC, and never let a language server do it in the background. All compiles and Rust tests go to fr1 (builds) / fr2 (tests) over ssh. Put this rule in every agent brief that touches Rust.

**Why:** 2026-11-17 the owner wrote "stop tout les cargo et build locaux tu freeze le pc ... trouve d'où ça vient et fix tout". Cause found: the official `rust-analyzer-lsp` plugin started rust-analyzer with defaults at cwd `$WORKSPACE_ROOT`, so it discovered every repo under it and ran `cargo check --workspace --all-targets` on each after every `.rs` edit by me or my agents (109 cargo/rustc/cl/link processes, one rustc at 5.5 GB, with `-Zthreads=8` and one job per core). My interpretation: the earlier "process restart" that killed 8 agents the same night was probably the same overload.

**How to apply:**
- Fix in place (keep it): `~/.claude/packages/aphrody/.lsp.json` declares rust-analyzer with `checkOnSave=false`, build scripts, proc macros and cache priming off; `rust-analyzer-lsp@claude-plugins-official` is `false` in settings.json; `%APPDATA%\rust-analyzer\rust-analyzer.toml` holds the same switches (verified: after it, a `.rs` save spawns only `cargo metadata`); `~/.cargo/config.toml` has `[build] jobs = 6`.
- rust-analyzer therefore gives navigation and syntax errors only; type errors come from `cargo check` on fr1 (clone under `/srv/xbuild/<name>`, see [[fleet-and-relay]], [[fleet-disks]]).
- If the PC is slow: list cargo/rustc/rust-analyzer with parent chains first (PowerShell `Get-CimInstance Win32_Process`), kill those by PID; never kill `bun.exe` by image name ([[push-checks-and-shared-writers]]).
