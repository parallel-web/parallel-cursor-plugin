---
name: parallel-result
description: "Get completed research task result. Usage: /parallel-result <run_id>"
---

# Get Research Result

## Run ID: $ARGUMENTS

```bash
parallel-cli research poll "$ARGUMENTS" --json
```

Present results in a clear, organized format.

If CLI not found, tell user to run `/parallel-setup`.
