---
name: block-curl-pipe-sh
description: "Best-effort guard against piping a network download straight into a shell or script interpreter: curl|wget|iwr ... | sh/bash/zsh/python/perl/ruby/node (bare, path-qualified, or QUOTED (\"sh\", 'bash'), including subshell groups and transparent wrappers -- sudo/exec/command/env/xargs/nohup/timeout/nice/stdbuf/ionice/setsid -- in front of it, bash -c \"$(curl ...)\", bash <(curl ...), and the PowerShell iwr ... | iex form. Downloading to a file, or piping a fetch into a non-interpreter tool (jq, tar, grep), stays allowed. Known bypass classes include aliases, variable indirection, base64/obfuscated payloads, env-var-prefixed interpreters, and non-standard fetch clients. This is friction, not a security boundary."
metadata:
  chock.artifact: rule
  chock.enforcement: advise
  chock.hooks: hooks.json
---

# Block Curl-Pipe-Shell

Best-effort guard against piping a network download straight into a shell or script interpreter: curl|wget|iwr ... | sh/bash/zsh/python/perl/ruby/node (bare, path-qualified, or QUOTED ("sh", 'bash'), including subshell groups and transparent wrappers -- sudo/exec/command/env/xargs/nohup/timeout/nice/stdbuf/ionice/setsid -- in front of it, bash -c "$(curl ...)", bash <(curl ...), and the PowerShell iwr ... | iex form. Downloading to a file, or piping a fetch into a non-interpreter tool (jq, tar, grep), stays allowed. Known bypass classes include aliases, variable indirection, base64/obfuscated payloads, env-var-prefixed interpreters, and non-standard fetch clients. This is friction, not a security boundary.

```
block(remote_exec): fetch(curl|wget|iwr|irm) piped/substituted into interpreter(sh|bash|python|perl|node|iex)
allow: download_to_file, fetch|non_interpreter(jq|tar); prefer: curl -o file; read; run
```

This policy ships a PreToolUse hook in this plugin's hooks.json, best-effort in Devin: fail-open by the vendor's own design, subject to the fail conditions stated in the plugin description. Repo-wide git-hook and CI coverage still needs `chock sync`. See https://github.com/open-coder-ai/chock
