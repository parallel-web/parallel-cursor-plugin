# Parallel Cursor Plugin

Web search, content extraction, deep research, data enrichment, entity discovery and web monitoring powered by [parallel-cli](https://docs.parallel.ai/integrations/cli).

## Features

This source package contains six skills, nine commands and one citation rule. Cursor's official marketplace reviews updates separately from GitHub changes, so the installed listing may contain an earlier revision. Check the listing and installed version before assuming source features have shipped.

| Capability | Skill | Command |
| --- | --- | --- |
| Web Search | `parallel-web-search` | `/parallel-search <query>` |
| Content Extraction | `parallel-web-extract` | `/parallel-extract <url> [url2]` |
| Deep Research | `parallel-deep-research` | `/parallel-research <topic>` |
| Data Enrichment | `parallel-data-enrichment` | `/parallel-enrich <data>` |
| Entity Discovery | `parallel-findall` | `/parallel-findall <objective>` |
| Web Monitoring | `parallel-monitor` | `/parallel-monitor <action>` |

Additional commands: `/parallel-setup`, `/parallel-status <run_id>` and `/parallel-result <run_id>`. Status and result are for research tasks only.

The package uses the authenticated CLI and does not bundle an MCP server. Ordinary lookups use Search with its default `basic` mode. Deep research requires an explicit request. FindAll discovers entities; enrichment adds fields to an existing list. Monitors persist until cancelled and can incur charges on scheduled or triggered executions.

## Installation

1. Install **Parallel** from Cursor's marketplace.
2. Run `/parallel-setup` in Cursor to check CLI installation, feature support and authentication.

For manual setup, prefer pipx:

```bash
pipx install "parallel-web-tools[cli]"
pipx ensurepath
parallel-cli login
```

If pipx is unavailable, the standalone installer is another option:

```bash
curl -fsSL https://parallel.ai/install.sh | bash
parallel-cli login
```

This source package is checked against CLI 0.9.3. Monitor GA commands require ≥ 0.4.0, Entity Search ≥ 0.6.0, research text/context and enrichment suggestions ≥ 0.3.0, and optional Search `fast` ≥ 0.9.2. Upgrade through the original installation method using `/parallel-setup`, and verify the CLI in Cursor's terminal. Cursor and Parallel CLI versions are separate.

`PARALLEL_API_KEY` overrides stored login credentials. `/parallel-setup` checks the active credential source without exposing secrets; stored organization metadata does not identify an overriding environment key's organization.

## Quick Start

```text
/parallel-search latest developments in AI chip manufacturing
/parallel-extract https://docs.parallel.ai/integrations/cli https://docs.parallel.ai/integrations/cursor-marketplace
/parallel-research comprehensive analysis of React vs Vue in 2026
/parallel-enrich companies.csv with CEO name, funding amount, and headquarters
/parallel-findall Find European climate-tech companies with headquarters and official homepage
/parallel-monitor Watch for official Parallel API changelog announcements daily
```

Async research, enrichment and FindAll return IDs that should be saved before polling. Polling timeouts end the local wait; resume the saved job instead of submitting another. Research may produce JSON only or JSON plus Markdown. Async enrichment produces JSON; requested CSV is converted locally with failed rows retained. FindAll requested fields require verified enrichment output, and an Entity Search ID cannot be polled as a FindAll run.

Monitor setup must follow the user's requested schedule and notification destination. A CLI polling loop does not guarantee future notifications. Save its monitor ID, inspect events and completion history, and cancel it when monitoring is no longer wanted.

## Local Development

Follow [Cursor's plugin documentation](https://cursor.com/docs/plugins) and [manifest reference](https://cursor.com/docs/reference/plugins). Test in a disposable Cursor environment so existing installs and work are preserved.

1. Clone this repository.
2. Confirm `~/.cursor/plugins/local/parallel` is absent before copying. If it exists, inspect its ownership and contents rather than overwriting it.
3. Copy the real package directory under that path, including `.cursor-plugin/plugin.json`, `skills`, `commands` and `rules`. Exclude `.git` and planning artifacts. Symlinks outside the local plugin directory are skipped.
4. Reload the disposable Cursor window. Inspect Customize for package origin, all six skills, nine commands and citation rule at the intended user or project scope.
5. Check that slash commands invoke their corresponding skills. Then use `/parallel-setup` to verify the terminal's CLI and credential source before an authorized capability test.

A same-name marketplace plugin takes precedence over the local copy. Test with that conflict absent only in the disposable environment. Enterprise policy can block local imports; do not change policy to force a test. Opening this repository or adding project-command symlinks does not prove plugin loading.

Verify each intended consumer independently, including the IDE, Agents Window and any Cursor CLI workflow. This source update does not establish propagation to cloud agents, Grok or Slack. Local loading and an upgrade rehearsal do not prove an official marketplace update or an existing marketplace install's upgrade.

## Plugin Structure

```text
.cursor-plugin/plugin.json   Plugin manifest
skills/                      6 capability skills
commands/                    9 slash commands
rules/                       1 citation standards rule
```

## License

MIT. See [LICENSE](LICENSE).
