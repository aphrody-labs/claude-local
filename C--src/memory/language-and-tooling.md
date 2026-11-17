---
name: language-and-tooling
description: Generalist unrestricted tool/language adoption, Rust/web doc tools, Windows path/regex handling, and isolated sandboxes
type: feedback
---

- **Missing tool = install it and pin it** (owner, 2026-11-17: "toujours installer ce qu'il manque et l'ajouter au aphrody toolchain versionné reproductible façon nix"): never work around a missing CLI. Add it to `~/.claude/toolchain.toml` (exact version, per-platform release URL, sha256 of the asset), then `bun ~/.claude/scripts/toolchain.ts install <name>` on each host; `toolchain.ts check` exits 1 on any drift. Commit the manifest in the config repo. Nix itself is not installed on the PC or fr1, so the manifest plays the derivation role.
- **Flexible languages & tools**: use whichever language and tool fits the task (`Rust`, `C`, `C++`, `Go`, `Bun/TypeScript`, `.NET`, `Python`, etc.). No stack or tool dogma (`bun add -g`, `cargo binstall`, `uv tool`, `winget`, `nix`).
- **Crate & repo research**: use `rustdoc-md` (live docs.rs JSON -> Markdown), `cargo doc-md`, `gitingest`, `Context7`, `gh api` (pipe JSON to `jaq`), `ast-grep`, and `rg`/`fd`.
- **Windows shell & path quirks**:
  - Normalize Windows backslashes to forward slashes or pass `-F` (`--fixed-strings`) to `rg`/`fd`/`sd` so paths like `\x86_64` never trigger regex escape errors.
  - Niubash (`niu`) reads `~/.niubashrc` (not `~/.bashrc`).
  - Write multiline or backslash-heavy scripts to `.ts`/script files rather than shell heredocs/inline quoting.
  - Check/stop stale localhost listeners before restarting Bun servers on Windows.
  - Git Bash `/tmp/x` is `%TEMP%\x`; Bun and other native programs do not resolve `/tmp/...` (ENOENT). Pass `$(cygpath -m /tmp)/x` or a real Windows path.
  - `gh api --paginate` prints one JSON array per page, so the concatenated output is not valid JSON: add `--jq '.[]'` (one object per line), use `--slurp`, or use `gh repo list <owner> --json ...`.
  - Remote edits: `ssh host 'python3 - <<EOF ...'` breaks as soon as the script holds quotes or parentheses (bash eval syntax error). Copy a script file with `scp` and run it, or pipe it on stdin (`ssh host python3 - < file`).
  - Windows `scp.exe` does not resolve Git Bash `/tmp/...` paths and can fail with `CreateProcessW failed error:2`. Ship files with `tar -cf - <files> | ssh <host> 'cd <dir> && tar -xf -'`.
  - Never redirect to `CON` or `NUL` from Git Bash: it creates a real file of that name in the repo (found in rosegriffon and shenron, holding a git credential prompt). Use `/dev/null`, and set `GIT_TERMINAL_PROMPT=0` for unattended fetches.
  - `bun test test` treats a bare word as a name filter and runs every test file; pass `./test` for a directory.
- **Isolated sandboxes**: run third-party doc examples or package tests under `%TEMP%` with separate `package.json`, `HOME`, `BUN_INSTALL`, stripped project bins, and process-tree kill on timeout; never run loose `bun add` inside an app workspace.
- **Clean tree**: leave zero `.bak` files or temp leftovers.
