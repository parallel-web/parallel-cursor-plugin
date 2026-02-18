# Parallel — Cursor Plugin

Web search, content extraction, deep research, and data enrichment powered by [parallel-cli](https://docs.parallel.ai/home).

## Features

| Capability | Skill | Command |
|---|---|---|
| **Web Search** | `parallel-web-search` | `/parallel-search <query>` |
| **Content Extraction** | `parallel-web-extract` | `/parallel-extract <url>` |
| **Deep Research** | `parallel-deep-research` | `/parallel-research <topic>` |
| **Data Enrichment** | `parallel-data-enrichment` | `/parallel-enrich <data>` |

Additional commands: `/parallel-setup`, `/parallel-status <run_id>`, `/parallel-result <run_id>`

## Installation

1. Install the plugin in Cursor from the marketplace (or clone this repo into your plugins directory).
2. Run `/parallel-setup` to install `parallel-cli` and authenticate.

### Manual CLI Setup

```bash
curl -fsSL https://parallel.ai/install.sh | bash
parallel-cli login
```

Or via pipx:

```bash
pipx install "parallel-web-tools[cli]"
parallel-cli login
```

## Quick Start

**Search the web:**
```
/parallel-search latest developments in AI chip manufacturing
```

**Extract a webpage:**
```
/parallel-extract https://example.com/article
```

**Deep research (slower, more thorough):**
```
/parallel-research comprehensive analysis of React vs Vue in 2026
```

**Enrich data:**
```
/parallel-enrich companies.csv with CEO name, funding amount, and headquarters
```

## Plugin Structure

```
.cursor-plugin/plugin.json   Plugin manifest
skills/                       4 skills (auto-discovered)
commands/                     7 slash commands (auto-discovered)
rules/                        Citation standards rule
assets/                       Logo
```

## License

MIT — see [LICENSE](LICENSE).
