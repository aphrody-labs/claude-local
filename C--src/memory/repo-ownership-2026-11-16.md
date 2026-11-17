---
name: repo-ownership-2026-11-16
description: "Owner 2026-11-16 - new monorepo aphrody-dev/ay (checkout $WORKSPACE_ROOT/ay-next, product-first, feature matrix) replaces aphrody-labs/ay (to archive); aphrody-dev/aphrody ($WORKSPACE_ROOT/aphrody, Zed fork) owns all native UI - GPUI, Craft GUI, COSMIC, iced, desktop shell"
metadata:
  node_type: memory
  type: project
  originSessionId: aae12588-4a11-4aae-b28f-f3d886683cbb
  modified: 2026-11-16T19:01:04.571Z
---

- **aphrody-dev/ay** (checkout `$WORKSPACE_ROOT/ay-next` until the swap) is the new monorepo, rebuilt from zero.
  - Plan: `~/.claude/plans/serene-honking-hummingbird.md`.
  - The scope is `docs/features.toml`; `bun run check` refuses a "done" row without proof.
  - The Drive folder "ay" mirrors the spec and the matrix.
- **aphrody-labs/ay** (`$WORKSPACE_ROOT/ay`) is to be archived read-only. Before that:
  - port `edge.yml` (the aphrody-sites deploy) and `device/cloudflare/projects.toml`;
  - repoint the fleet remotes (src-a8 is holding this until cloud/ is ported);
  - relocate the relay and services.
- **aphrody-dev/aphrody** (`$WORKSPACE_ROOT/aphrody`, Zed fork, agy's work) owns all native UI: GPUI and gpui-kit, COSMIC, iced, the desktop shell (`gpui_app_shell`) and Craft's native UI (`os_craft`). ay consumes it as submodule `third_party/aphrody`; never write GPUI or COSMIC code in ay.

**Why:** the owner said "$WORKSPACE_ROOT/aphrody own craft gpui, cosmic etc." and approved the ay rebuild plan.
**How to apply:** native UI work goes to the aphrody repo; ay holds the web products, the OS distribution data, crates and packages.
