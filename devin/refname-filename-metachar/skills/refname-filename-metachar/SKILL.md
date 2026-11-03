---
name: refname-filename-metachar
description: "Refuses names a shell, CI step or git can misread. Paths a change adds or renames into (commit, agent writes), and branches and tags pushed (pre-push), with command substitution, an IFS expansion, a backtick, ; & | < >, a double quote, a backslash, a control or bidi character, a base64-decode shape, a leading dash or trailing space or dot in a segment, or a '..' segment; a ref also with a full commit id's shape. A guard refuses git and file commands creating such names. No waiver. Friction, not a boundary."
metadata:
  chock.artifact: hook
  chock.enforcement: block
  chock.hooks: hooks.json
---

# Block Risky Characters in File and Branch Names

Refuses names a shell, CI step or git can misread. Paths a change adds or renames into (commit, agent writes), and branches and tags pushed (pre-push), with command substitution, an IFS expansion, a backtick, ; & | < >, a double quote, a backslash, a control or bidi character, a base64-decode shape, a leading dash or trailing space or dot in a segment, or a '..' segment; a ref also with a full commit id's shape. A guard refuses git and file commands creating such names. No waiver. Friction, not a boundary.

```
on(commit|tool_use): block(script) script=refname-filename-metachar-gate.py
A path this change adds or renames into has a name a shell, a CI step or git can misread: command substitution, an IFS expansion, a backtick, a shell operator, a double quote, a backslash, a control or bidi character, a base64-decode shape, a segment starting with '-' or ending in a space or '.', or a '..' segment. Rename it with letters, digits, spaces inside, '.', '_', '-' and '/'. No waiver: if the name is truly required, a person creates it from their own shell.
```

This policy ships a PreToolUse hook in this plugin's hooks.json, best-effort in Devin: fail-open by the vendor's own design, subject to the fail conditions stated in the plugin description. Its write gate is enforced in this client by the Stop hook installed with the plugin: the write itself is not judged here, because the client records no write-tool vocabulary, and what the turn left on disk is judged at its end instead. Subject to the fail conditions stated in the plugin description. Repo-wide enforcement across every commit and in CI still needs `chock sync`. See https://github.com/open-coder-ai/chock
