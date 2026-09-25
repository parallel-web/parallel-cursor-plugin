---
name: parallel-result
description: "Retrieve research task output only. Usage: /parallel-result <run_id>"
---

# Get Research Result

## Run ID: $ARGUMENTS

Use only a saved research `run_id`. Establish an unknown ID's origin first. For enrichment use `enrich poll`, for FindAll use `findall poll` or `findall result`, and for Monitor use `monitor events`. Never poll an Entity Search ID or Search/Extract session ID as research.

Choose a concrete, run-specific output base in a persistent directory, such as `reports/research-<run-id>`. Create the parent directory if needed and inspect existing `.json` and `.md` paths. Replace `$OUTPUT_BASE` below with that chosen base.

```bash
parallel-cli research poll "$ARGUMENTS" --timeout 60 -o "$OUTPUT_BASE"
```

Do not add `--json` or dump the full output into chat. On completion, JSON contains metadata and basis; Markdown exists only for text output. In that case, resolve `output.content_file` relative to the saved JSON. Read the actual saved paths and verify files exist before linking them. The CLI can fall back to temporary storage after a write error; inspect partial writes and copy the final files to the intended persistent location before claiming durable delivery.

Existing files are refused unless `--force` is explicit. Prefer a fresh base, and use `--force` only when replacing those files is intended. Share an executive summary if printed; otherwise summarize only inspected relevant output. Report the actual paths and retain the returned `interaction_id` for Task follow-ups.

Timeout exit 5 or interruption ends the local wait. Check the same saved task with `/parallel-status`, then resume this poll for a pending/running task. Retrieve completed output or report failed/cancelled/`action_required` states; never submit a replacement task because polling ended.

If the binary is missing, use `/parallel-setup`. For a missing command or option, use its installation-specific upgrade guidance. For authentication errors, inspect `parallel-cli auth --json` and `authenticated`; a `403` alone does not prove insufficient balance.
