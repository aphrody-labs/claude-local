---
name: feedback-decide-do-not-ask
description: The user wants decisions made and applied (vendor/third-party/first-party layout, tooling) without questions; rulesets and shared protections are still never bypassed
metadata:
  type: feedback
---

When the user hands over a reorganisation or a choice ("apply ... best practice now and no decision ask", "reorganise like chromium 160"), pick the best-practice option, apply it, verify it, report it. Do not stop to ask between options.

**Why:** the user repeated it in this repo (and global CLAUDE.md says zero questions). I had paused on the linear-history ruleset and the hyperfine import; they answered with "no decision ask".
**How to apply:** choose the option that keeps protections intact (e.g. replay imports linearly instead of relaxing the `main-integrity` ruleset; never use the admin bypass GitHub offers on push), apply it, and state the choice in the report. Heavy work (cargo, imports, big git moves) runs on fr1/fr2, not the PC.
