---
name: parallel-web-search
description: "DEFAULT for all research and web queries. Use for any lookup, research, investigation, or question needing current info. Fast and cost-effective. Only use parallel-deep-research if user explicitly requests 'deep' or 'exhaustive' research."
compatibility: Requires parallel-cli and internet access.
allowed-tools: Bash(parallel-cli:*)
metadata:
  author: parallel
---

# Web Search

Search the web for: $ARGUMENTS

## Command

Choose a short, descriptive filename based on the query (e.g., `ai-chip-news`, `react-vs-vue`). Use lowercase with hyphens, no spaces.

```bash
mkdir -p /tmp/parallel && parallel-cli search "$ARGUMENTS" -q "<keyword1>" -q "<keyword2>" --json --max-results 10 --excerpt-max-chars-total 27000 -o "/tmp/parallel/$FILENAME.json"
```

The first argument is the **objective** — a natural language description of what you're looking for. It replaces multiple keyword searches with a single call for broad or complex queries. Add `-q` flags for specific keyword queries to supplement the objective. The `-o` flag saves the full results to `/tmp/parallel/` to avoid cluttering the workspace.

Options if needed:
- `--after-date YYYY-MM-DD` for time-sensitive queries
- `--include-domains domain1.com,domain2.com` to limit to specific sources

## Parsing results

The command outputs JSON to stdout. For each result, extract:
- title, url, publish_date
- Useful content from excerpts (skip navigation noise like menus, footers, "Skip to content")

## Response format

**CRITICAL: Every claim must have an inline citation.** Use markdown links like [Title](URL) pulling only from the JSON output. Never invent or guess URLs.

Synthesize a response that:
- Leads with the key answer/finding
- Includes specific facts, names, numbers, dates
- Cites every fact inline as [Source Title](url) — do not leave any claim uncited
- Organizes by theme if multiple topics

**End with a Sources section** listing every URL referenced:

```
Sources:
- [Source Title](https://example.com/article) (Feb 2026)
- [Another Source](https://example.com/other) (Jan 2026)
```

This Sources section is mandatory. Do not omit it.

After the Sources section, mention the output file path (`/tmp/parallel/$FILENAME.json`) so the user knows it's available for follow-up questions.

## If `parallel-cli` is not found

If the command fails with "command not found", **stop immediately**. Do NOT search the web yourself, do NOT use any built-in search tools, and do NOT try to answer the query from your own knowledge. Instead, tell the user:

1. `parallel-cli` is not installed
2. Run `/parallel-setup` to install it
3. Then retry their search
