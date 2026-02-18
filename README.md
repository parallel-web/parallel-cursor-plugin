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

1. Install the plugin in Cursor from the marketplace (or see [Local Development](#local-development) to test from source).
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

## Local Development

To test the plugin locally without installing from the marketplace:

1. Clone this repo:
   ```bash
   git clone https://github.com/parallel-web/parallel-cursor-plugin.git
   ```

2. Open the repo in Cursor:
   ```bash
   cursor parallel-cursor-plugin
   ```

3. Skills and rules are auto-discovered from the standard directories. Type `/` in the chat to verify the `parallel-*` skills are listed.

4. Commands are **not** auto-discovered when testing locally. Symlink them into Cursor's project commands directory:
   ```bash
   ln -s ../commands .cursor/commands
   ```

5. Type `/` again — the `parallel-*` commands should now appear alongside the skills.

6. Run `/parallel-setup` to confirm the CLI is installed and authenticated.

## Plugin Structure

```
.cursor-plugin/plugin.json   Plugin manifest
skills/                       4 skills (auto-discovered)
commands/                     7 slash commands
rules/                        Citation standards rule
assets/                       Logo
```

## License

MIT — see [LICENSE](LICENSE).
