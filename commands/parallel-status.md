---
name: parallel-status
description: "Check research task status only. Usage: /parallel-status <run_id>"
---

# Check Research Status

## Run ID: $ARGUMENTS

Use only a saved research `run_id`. If the ID's origin is unknown, establish which operation returned it before calling the CLI. Enrichment task groups use `enrich status`, FindAll runs use `findall status`, and monitors use `monitor get`. Entity Search IDs and Search/Extract session IDs cannot be checked as research tasks.

```bash
parallel-cli research status "$ARGUMENTS" --json
```

Inspect the exit status and JSON. Report pending/running, completed, failed/cancelled or `action_required` accurately. On failure, preserve the run ID and report the returned error; do not create a new research task. An `action_required` state needs the indicated action, not indefinite polling. Use `/parallel-result` to retrieve completed output.

If the binary is missing, use `/parallel-setup`. For a missing command or option, use its installation-specific upgrade guidance. For authentication errors, inspect `parallel-cli auth --json` and `authenticated`; a `403` alone does not prove insufficient balance.
