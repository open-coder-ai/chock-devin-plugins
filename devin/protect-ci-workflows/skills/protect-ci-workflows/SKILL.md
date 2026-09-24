---
name: protect-ci-workflows
description: "Guard against an agent weakening the automated checks that review its own work. CI/CD workflow files (.github/workflows/), the composite actions they call (.github/actions/) and the dependency-update automation (.github/dependabot.yml) define what must pass before a change lands -- so rewriting or deleting them is the agent removing the gate that would catch it. The guard refuses shell write-commands targeting those paths; reads pass, and tool-driven regeneration (chock sync) passes. Best-effort and deliberately coarse: a write signal (a `>`/`>>` redirect, or a writer verb like rm/cp/sed -i/git checkout) is scoped to its own clause of the command line, so it must actually target the protected path, not merely appear alongside it; a glued-on separator or a non-redirect writer's own operand are not resolved that finely. The 'chock: approved-config-change' marker is friction plus an audit trail, not authentication; the check an agent cannot self-approve is server-side branch protection."
metadata:
  chock.artifact: rule
  chock.enforcement: advise
  chock.hooks: hooks.json
---

# Protect CI Workflows

Guard against an agent weakening the automated checks that review its own work. CI/CD workflow files (.github/workflows/), the composite actions they call (.github/actions/) and the dependency-update automation (.github/dependabot.yml) define what must pass before a change lands -- so rewriting or deleting them is the agent removing the gate that would catch it. The guard refuses shell write-commands targeting those paths; reads pass, and tool-driven regeneration (chock sync) passes. Best-effort and deliberately coarse: a write signal (a `>`/`>>` redirect, or a writer verb like rm/cp/sed -i/git checkout) is scoped to its own clause of the command line, so it must actually target the protected path, not merely appear alongside it; a glued-on separator or a non-redirect writer's own operand are not resolved that finely. The 'chock: approved-config-change' marker is friction plus an audit trail, not authentication; the check an agent cannot self-approve is server-side branch protection.

```
ci_config(.github/workflows|.github/actions|.github/dependabot.yml): never(shell_edit|delete); propose_to_human
if(ci_change_needed): open PR; await(review)  # an agent must not disarm the checks on its own work
```

This policy ships a PreToolUse hook in this plugin's hooks.json, best-effort in Devin: fail-open by the vendor's own design, subject to the fail conditions stated in the plugin description. Repo-wide git-hook and CI coverage still needs `chock sync`. See https://github.com/open-coder-ai/chock
