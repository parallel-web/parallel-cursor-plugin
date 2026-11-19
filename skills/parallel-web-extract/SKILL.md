---
name: parallel-web-extract
description: "Content extraction from one or more URLs, including webpages, articles and PDFs. Preserve successful content and report per-URL failures."
compatibility: Requires the Parallel MCP or parallel-cli, and internet access.
allowed-tools: Bash(parallel-cli:*)
metadata:
  author: parallel
---

# URL Extraction

Extract content from: $ARGUMENTS

Use `web_fetch` from the `parallel` MCP bundled with the Parallel plugin. Cursor may prefix its name. If the user configured a signed-in Parallel MCP as a replacement, use that server. Use the CLI steps below when the selected MCP tool is unavailable.

If anonymous MCP returns a rate limit, report it and offer to retry later or use a signed-in Parallel MCP. If the user requests the CLI fallback, use the steps below.

## Command

Choose a short, descriptive filename based on the URL or content (e.g., `vespa-docs`, `react-hooks-api`). Use lowercase with hyphens, no spaces. Substitute it into the command **inline** — `$FILENAME` is a placeholder, not a shell variable.

Pass each requested URL as a separate quoted positional argument, up to 20 per call. Do not collapse multiple URLs into one quoted `$ARGUMENTS` string or use `eval` to split them. Construct arguments directly from the requested URLs. For example:

```bash
parallel-cli extract "https://docs.parallel.ai/integrations/cli" "https://docs.parallel.ai/integrations/cursor-marketplace" --json -o "/tmp/parallel-docs.json"
```

`-o` saves JSON. Use a `.json` extension and inspect an existing path before use because Extract overwrites it. Read the saved file as authoritative; stdout may truncate and human-readable output previews only part of the content. Do not treat a stale file as a successful response after a failed call.

Options if needed:

- `--objective "focus area"` to focus extraction on a specific goal (also silences the "neither objective nor search_queries" warning that V1 emits when neither is set)
- `-q "keyword"` (repeatable) to prioritize keywords in excerpts
- `--full-content` to include the complete page body (for long articles, PDFs, or when excerpts may not capture what you need)
- `--full-content-max-chars N` to cap full-content size per result
- `--no-excerpts` to strip excerpts when you only want full content
- `--session-id "<returned-session-id>"` to group related Search/Extract calls. A session ID is not a Task interaction ID or run ID; never use it with research status/poll or `--previous-interaction-id`

## Handling failed extractions

Inspect the exit status, API error, `results`, per-URL `errors` and any warnings. `errors: []` is normal success. Nonempty errors can coexist with successful results: retain and present successful content, then name each failed URL and its returned reason. Empty results or missing content are not a successful extraction. Do not fabricate content. For affected URLs, suggest:

- Verifying the URL (the page may have moved)
- Requesting `--full-content` if excerpts are empty but the returned metadata supports that the page was fetched
- Using `parallel-cli search` to locate the current URL if the page was renamed

## Response format

Return content as:

**[Page Title](URL)**

Use returned `full_content` for full-page requests; excerpts alone are selected passages and must be labelled as such. Even full content may be capped by `--full-content-max-chars` or upstream limits; do not promise completeness when capped. Preserve retrieved content verbatim, with these rules:

- Keep content verbatim - do not paraphrase or summarize
- Preserve every numbered/bulleted item in the retrieved content; do not claim an excerpt contains the whole page
- Strip only obvious noise: nav menus, footers, ads
- Preserve all facts, names, numbers, dates, quotes

If you used the CLI, mention the output file path after the response (`/tmp/$FILENAME.json`) so the user knows it's available for follow-up questions.

For large content, keep the full verbatim text in the saved file and provide a brief labelled preview plus its path. Never silently truncate content while claiming it is the complete extraction.

## If the `parallel-cli` binary is not installed

If the shell reports `command not found: parallel-cli`, tell the user to enable the bundled `parallel` server under Customize → MCPs, or run `/parallel-setup` and ask for CLI fallback setup. Do not substitute built-in search, another provider or an answer from memory.

### Command and authentication failures

`No such command`, `No such option` or `unrecognized arguments` from an installed CLI indicate a stale or mismatched interface. Check its version and upgrade through its installation method using `/parallel-setup`; `parallel-cli update` is for standalone installs only. Verify the required command in the same Cursor terminal before retrying.

For authentication errors, run `parallel-cli auth --json` and inspect `authenticated`; exit zero alone does not prove authentication. Use `/parallel-setup` for terminal login or environment-key guidance, without requesting credentials in chat. A `403` can be an authorization or billing error: report the actual error and do not assume insufficient balance or add funds automatically.

For other API/input errors, report the error without calling it a version problem. Reuse saved run IDs to resume asynchronous work. After an ambiguous creation failure, resolve whether a job exists before retrying creation.
