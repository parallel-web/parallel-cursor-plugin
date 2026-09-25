---
name: parallel-web-search
description: "CLI web search, the default for lookups, current information and research queries. Save retrieved sources as JSON and cite them. Only use parallel-deep-research when the user explicitly requests deep or exhaustive research."
compatibility: Requires parallel-cli and internet access.
allowed-tools: Bash(parallel-cli:*)
metadata:
  author: parallel
---

# Web Search

Search the web for: $ARGUMENTS

## Command

Choose a short, descriptive filename based on the query (e.g., `ai-chip-news`, `react-vs-vue`). Use lowercase with hyphens, no spaces. Substitute it into the command **inline** — `$FILENAME` and `<keyword>` below are placeholders, not shell variables; do not copy them verbatim.

```bash
parallel-cli search "$ARGUMENTS" -q "<keyword1>" -q "<keyword2>" --json --max-results 10 --excerpt-max-chars-total 27000 -o "/tmp/$FILENAME.json"
```

Concrete example for a query about React 19:

```bash
parallel-cli search "latest React 19 features and adoption" -q "React 19" -q "concurrent rendering" --json --max-results 10 --excerpt-max-chars-total 27000 -o "/tmp/react-19-features.json"
```

The first argument is the **objective** — a natural language description of what you're looking for. It replaces multiple keyword searches with a single call for broad or complex queries. Add `-q` flags for specific keyword queries to supplement the objective. The `-o` flag saves the full results to a JSON file for follow-up questions.

Options if needed:

- `--after-date YYYY-MM-DD` for time-sensitive queries
- `--include-domains domain1.com,domain2.com` to limit to specific sources
- `--exclude-domains domain.com` to filter out noisy sources
- `--mode turbo` for simple fact lookups where speed matters most; supports English and Japanese queries
- `--mode fast` for high-quality search within an approximately one-second latency budget; requires CLI ≥ 0.9.2, and latency is not guaranteed
- `--mode advanced` for harder questions (multi-step, agentic search). Keep the default `basic` unless the request needs another mode
- `--location us` (ISO 3166-1 alpha-2) for geo-targeted results
- `--session-id "<returned-session-id>"` to group related Search/Extract calls when a prior response returned one. A `session_id` or `search_id` is not a Task interaction ID or research run ID; never send it to research status/poll or `--previous-interaction-id`

## Parsing results

**Read the saved `-o` JSON file as the authoritative payload.** Result and excerpt limits bound requested content, but stdout can still exceed the tool's output limit. Truncated stdout is not parseable JSON and is not proof of incomplete saved results. Inspect an existing output path before using it because Search overwrites that file. For each result, extract:

- title, url, and publish_date if provided; omit unknown dates
- Useful content from excerpts (skip navigation noise like menus, footers, "Skip to content")

Check the exit status, returned API error and `warnings` before presenting results. On an error or empty `results`, report what happened and do not fabricate an answer. An old output file is not evidence that a failed request succeeded. For sparse results, state the coverage limits; refine the objective or queries only when useful for the user's request.

## Response format

**CRITICAL: Every claim must have an inline citation.** Use markdown links like [Title](URL) pulling only from the JSON output. Never invent or guess URLs.

Synthesize a response that:

- Leads with the key answer/finding
- Includes specific facts, names, numbers, dates
- Cites every fact inline as [Source Title](url) — do not leave any claim uncited
- Organizes by theme if multiple topics

**End with a Sources section** listing every URL referenced:

```text
Sources:
- [Source Title](https://example.com/article) (Feb 2026)
- [Another Source](https://example.com/other) (Jan 2026)
```

This Sources section is mandatory. Do not omit it.

Only include source dates that were returned or verified in the retrieved content. Leave the date out when unknown.

After the Sources section, mention the output file path (`/tmp/$FILENAME.json`) so the user knows it's available for follow-up questions.

## If the `parallel-cli` binary is not installed

If the shell reports `command not found: parallel-cli`, stop and tell the user to run `/parallel-setup`, then retry their request. Do not substitute built-in search, another provider or an answer from memory.

### Command and authentication failures

`No such command`, `No such option` or `unrecognized arguments` from an installed CLI indicate a stale or mismatched interface. Check its version and upgrade through its installation method using `/parallel-setup`; `parallel-cli update` is for standalone installs only. Verify the required command in the same Cursor terminal before retrying.

For authentication errors, run `parallel-cli auth --json` and inspect `authenticated`; exit zero alone does not prove authentication. Use `/parallel-setup` for terminal login or environment-key guidance, without requesting credentials in chat. A `403` can be an authorization or billing error: report the actual error and do not assume insufficient balance or add funds automatically.

For other API/input errors, report the error without calling it a version problem. Reuse saved run IDs to resume asynchronous work. After an ambiguous creation failure, resolve whether a job exists before retrying creation.
