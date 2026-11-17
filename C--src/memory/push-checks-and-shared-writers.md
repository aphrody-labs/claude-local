---
name: push-checks-and-shared-writers
description: "Owner 2026-11-16 said the whole session was full of errors - run the repo's own checks before pushing, keep low-disk build flags, check other writers before editing shared config, deliver the approved plan"
metadata:
  node_type: memory
  type: feedback
  originSessionId: aae12588-4a11-4aae-b28f-f3d886683cbb
  modified: 2026-11-16T18:02:49.206Z
---

Mistakes from the 2026-11-16 session (craft-anything in the Bun fork, the bun submodule, aphrody-git, memory consolidation), and the rule for each:

- **Pushed to the Bun fork's main without its checks.** It went red for several commits: the source lints scanned `src/craft`, the merged `bun migrate` code broke them, and Cargo.lock was stale. Before a push, run the repo's own gates: `bun test test/internal/source-lints/*`, the affected `cargo check`, and the CI status of the previous push.
- **Filled fr1's disk** with a debug `cargo test` (incremental cache and debug info). Set `CARGO_INCREMENTAL=0` and `CARGO_PROFILE_DEV_DEBUG=0`, check `df` first, and remove the target dir when done.
- **Edited the memory folder and `~/.claude/CLAUDE.md` while another session was editing them.** Then I blamed the wrong session. Before editing shared config, `ListAgents` and `git log`/mtime on the file, then coordinate. Name the culprit only from evidence (commit author or session).
- **Approved plan not delivered.** The plan was `bun craft`: CLI and MCP wired into the Bun binary, dedup onto Bun crates, end-to-end run. I absorbed the trees, then drifted to other requests. When a request moves on, say plainly what of the approved plan is still open.
- **Brittle shell edits** (sed with quotes and backslashes, a Python that isn't there) wasted turns. Use the Edit tool, or a script file, for anything with special characters.
- **Scripts reported results on the wrong signal** ("sync refused" read from empty stdout). Decide on exit codes.
- **Rewrote AGENTS.md rules another session owned** (the main freeze, the sources-of-truth rule) right after saying I'd keep them. When a rule belongs to someone else's active work, change it only on the owner's explicit word, and say so.

- **A purge agent deleted other writers' uncommitted edits** (nie `docs/ie`, `docs/re`, untracked `replica/screens`, 2026-11-16). Every brief that deletes or restructures a tree must say: run `git status` first, leave foreign modified/untracked paths in place, and report them instead of removing them.

- **`taskkill /F /IM bun.exe` kills every session's Bun** (MCP servers, hooks, other agents' servers), 2026-11-17. Stop only the PID you spawned (`proc.kill()`, `Stop-Process -Id`).
- **Restored legacy code is unreviewed code** (2026-11-17). Crates and packages restored from crates.io, npm and the legacy repo (ay-svg, ay-patch, bun-x, ai-os env-kit) drew 13 security findings after they were pushed: SVG sanitizer bypasses, usvg external image resolution, path traversal in patch application, bearer token following redirects, `.env` located from the current directory, a CI gate that exited 0 when its source was missing. Run the `security-review` skill on the restore diff before the first push, and treat "fetches a URL", "writes a path from input", "sanitizes markup" and "loads env" as the four things to read by hand.

Related: [[agent-policy]], [[autonomy-nhitl]].
