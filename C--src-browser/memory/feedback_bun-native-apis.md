---
name: feedback-bun-native-apis
description: Tools in this repo use Bun's own APIs everywhere; node:* only where Bun has no equivalent (node:path string functions)
metadata:
  type: feedback
---

Write every tool with native Bun APIs: Bun.file / Bun.write, Bun.$ (mkdir, rm, mv, hostname), Bun.Glob, Bun.spawn, Bun.connect (TLS, certificate via getPeerCertificate), Bun.inflateSync, bun:sqlite, RedisClient, Bun.cron, Bun.serve, Bun.TOML, crypto.timingSafeEqual (global), navigator.hardwareConcurrency. Only `node:path` stays (no Bun equivalent for path strings).

**Why:** the user said "use ALLL native bun api" after a first version of tools/acceptance.ts imported node:fs, node:os, node:tls, node:util parseArgs, node:zlib and node:crypto.
**How to apply:** helpers live in tools/acceptance/sys.ts (exists, mkdirp, rmrf, list, parseFlags, sampleRss ...); reuse them, do not add node:fs. Facts checked on Bun 1.4.5-canary: Bun.connect sockets have no getProtocol (use getCipher().version) and report ALPN as a boolean; Bun.file(dir).exists() is false for directories (use .stat()); Bun.Image has no raw() in that build.
