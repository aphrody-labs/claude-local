---
name: reference-fleet-file-transfer
description: scp.exe fails from Git Bash on the PC; copy to fr1/fr2 with tar or cat over ssh stdin
metadata:
  type: reference
---

`scp` from the Bash tool fails here (`CreateProcessW failed error:2 ... posix_spawn: No such file or directory`). Use instead:
- a tree: `tar cf - dir file | ssh -o BatchMode=yes fr1 'mkdir -p /tmp/x && tar xf - -C /tmp/x'`
- one file: `ssh fr1 'cat > /tmp/f' < local-file`
Quote carefully: a `'` inside an ssh/bun -e one-liner closes the shell quote (write the script to a file with a quoted heredoc instead). `pkill -f` with a pattern also matches your own ssh command line: kill by PID.
Linux hosts: fr1 builds (Chrome for Testing 160 at /srv/xbuild/scratch/base, hyperfine, obscura, agent-browser, scratch /srv/xbuild/scratch), fr2 has valkey-server (not running) and hyperfine; neither host has curl with HTTP/3.
