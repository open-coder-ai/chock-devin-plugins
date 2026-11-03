---
name: block-destructive-commands
description: "Best-effort, on parsed commands (echo ignored); one verdict table shared with rtk-dangerous-actions-blocker. Blocks recursive deletes of root, home, abs paths or .git; mv/chmod/chown -R of system dirs; dd to devices, mkfs; lockouts; force, delete, mirror pushes; history rewrites; switch --discard; IaC destroy; cloud deletes; DB drops. Asks: rm -rf off a safe list, one named kubectl delete, bare lease, stash drop, -auto-approve, drain, prunes, API deletes. Dry runs pass. Pre-push: no non-ff."
metadata:
  chock.artifact: rule
  chock.enforcement: advise
  chock.hooks: hooks.json
---

# Block Destructive Commands

Best-effort, on parsed commands (echo ignored); one verdict table shared with rtk-dangerous-actions-blocker. Blocks recursive deletes of root, home, abs paths or .git; mv/chmod/chown -R of system dirs; dd to devices, mkfs; lockouts; force, delete, mirror pushes; history rewrites; switch --discard; IaC destroy; cloud deletes; DB drops. Asks: rm -rf off a safe list, one named kubectl delete, bare lease, stash drop, -auto-approve, drain, prunes, API deletes. Dry runs pass. Pre-push: no non-ff.

```
block: rm|mv|chmod|chown_-R(root|home|sys),rm_-rf(abs|.|$PWD|.git),dd_of=/dev|mkfs|wipefs|shred,lockout(authkeys|usermod|passwd|chattr_+i|kill_-1|crontab),git(push_-f|+ref|-d|:ref|--mirror,reset_--hard,clean_-f,checkout|restore_.|switch_--discard,reflog|gc|filter),iac_destroy,cloud|k8s|helm_delete,sql_drop
ask: rm_-rf(off safe_list),kubectl_delete(1_named),lease_bare,stash_drop,branch_-D,-auto-approve,drain,prune,pkill_-f,systemctl_disable,api_DELETE;allow: tool_dry_run;push: refuse_non_ff
```

This policy ships a PreToolUse hook in this plugin's hooks.json, best-effort in Devin: fail-open by the vendor's own design, subject to the fail conditions stated in the plugin description. Repo-wide git-hook and CI coverage still needs `chock sync`. See https://github.com/open-coder-ai/chock
