---
name: build-host-github-access
description: "How fr1 reaches GitHub (ssh key, no gh, no https credentials) and where aysh work lives there"
metadata:
  node_type: memory
  type: reference
  originSessionId: 6f181943-0def-4110-8bab-928b529374a8
  modified: 2026-11-17T02:39:03.101Z
---

On the build host `fr1` (verified 2026-11-17):

- GitHub access is by ssh key only, authenticated as `aphrody-dev`: clone and push with `git@github.com:<owner>/<repo>.git`. `https://` remotes can fetch public repos but cannot push (no credential helper), and `gh` (pinned 2.104.0 from the toolchain) is not logged in, so API calls (fork, compare, merge) run from the PC. The same fact is recorded for every project in [[fleet-and-relay]].
- aysh work directory: `/srv/xbuild/aysh` — `src/` (tree copied by `bun tools/remote.ts`, replaced on every run), `vendor/` (pinned forks fetched by that tool), `target/<name>` (build dirs), `forks/<name>` (full clones used to merge upstream into a fork), `spike/` (throwaway experiments).
- Windows code paths are type-checked from Linux with plain `cargo check --target x86_64-pc-windows-msvc` (target installed; a check links nothing, so no MSVC SDK is needed). `cargo xwin` hung for 10+ minutes at "Downloading MSVC CRT" in two separate agents on 2026-11-17 while sharing `~/.cache/cargo-xwin`: use it only to link, one run at a time.
- Toolchains installed: stable 1.100.0, nightly-2026-11-14 (the aysh pin), nightly-2026-09-15 (host default).

**How to apply:** give fork-patching agents the ssh remote form up front; do API-side fork operations (create, fast-forward, `merges` endpoint) from the PC with `gh api`.
