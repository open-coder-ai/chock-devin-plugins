---
name: rtk-dangerous-actions-blocker
description: "Deprecated: use block-destructive-commands, which shares this policy's destructive-command table. rtk#1007. Same table as block-destructive-commands: blocks root/home/.git deletes, force/delete/mirror pushes, reset --hard, clean -f, switch --discard, IaC destroy, cloud deletes, DB drops, lockouts; asks rm -rf off rtk's safe list, one named kubectl delete, bare lease, stash drop, prunes; dry-run flags pass. Own rows block credential-file reads (.env, keys, ~/.ssh) and echo or inline *_API_KEY/_SECRET/_TOKEN. Skips file rows in container exec."
metadata:
  chock.artifact: rule
  chock.enforcement: advise
  chock.hooks: hooks.json
---

# Block Dangerous Commands (Deprecated)

Deprecated: use block-destructive-commands, which shares this policy's destructive-command table. rtk#1007. Same table as block-destructive-commands: blocks root/home/.git deletes, force/delete/mirror pushes, reset --hard, clean -f, switch --discard, IaC destroy, cloud deletes, DB drops, lockouts; asks rm -rf off rtk's safe list, one named kubectl delete, bare lease, stash drop, prunes; dry-run flags pass. Own rows block credential-file reads (.env, keys, ~/.ssh) and echo or inline *_API_KEY/_SECRET/_TOKEN. Skips file rows in container exec.

```
block: shared table (= block-destructive-commands: rm_-rf(/|~|.|abs|.git),push(-f|+ref|:ref|--mirror),reset_--hard|clean_-f|checkout_.|switch_--discard,destroy|cloud_delete,sql_drop|dropdb),read(.env|*.pem|*.key|id_rsa|~/.ssh|~/.aws),echo|inline($*_API_KEY|$*_SECRET|$*_TOKEN)
ask: rm_-rf(off safe_list),kubectl_delete(one_named),lease_bare,stash_drop,branch_-D,docker_prune,-auto-approve,pkill_-f,systemctl_disable;allow: dry_run(tool_flag);skip_inside: container_exec(file rows);prefix: rtk
```

This policy ships a PreToolUse hook in this plugin's hooks.json, best-effort in Devin: fail-open by the vendor's own design, subject to the fail conditions stated in the plugin description. Repo-wide git-hook and CI coverage still needs `chock sync`. See https://github.com/open-coder-ai/chock
