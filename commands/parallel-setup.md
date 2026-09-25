---
name: parallel-setup
description: Set up the Parallel plugin (install CLI and authenticate)
---

# Parallel Plugin Setup

## Check the CLI in Cursor's terminal

```bash
parallel-cli --version
```

If the binary exists, check the help for the feature the user needs before skipping installation. This package is checked against CLI 0.9.3. Monitor's GA commands require ≥ 0.4.0, Entity Search ≥ 0.6.0, research text/context and enrichment suggestions ≥ 0.3.0, and optional native Search `fast` ≥ 0.9.2. Default Search remains `basic`.

```bash
parallel-cli monitor --help
parallel-cli findall entity-search --help
parallel-cli research run --help
parallel-cli search --help
```

`No such command`, `No such option` or `unrecognized arguments` indicates a stale or mismatched CLI interface. API, authentication and invalid-input errors do not indicate a version problem. Identify the install method before upgrading:

| Install method | Upgrade |
| --- | --- |
| pipx | `pipx upgrade parallel-web-tools` |
| uv tool | `uv tool upgrade parallel-web-tools` |
| Homebrew | `brew upgrade parallel-web/tap/parallel-cli` |
| npm global | `npm update -g parallel-web-cli` |
| Standalone install script | `parallel-cli update` |

Recheck version and feature help in the same Cursor terminal after upgrading. Do not use the standalone updater for a package-manager install.

## Install when the binary is missing

Prefer pipx for an isolated Python CLI installation:

```bash
pipx install "parallel-web-tools[cli]"
pipx ensurepath
```

If pipx is unavailable, use the documented standalone installer in a terminal with the required network and filesystem access:

```bash
curl -fsSL https://parallel.ai/install.sh | bash
```

If an agent sandbox blocks installation, explain the specific error and give the user the appropriate terminal command. Do not tell them to disable sandboxing. Verify `parallel-cli --version` in Cursor's terminal; a successful install in another shell does not establish this terminal's PATH. If needed, add the actual installation bin directory (commonly `~/.local/bin`) to the shell PATH and open a new terminal.

## Check authentication and active credential source

```bash
parallel-cli auth --json
```

Inspect `authenticated`, `method`, `env_var_set` and `has_stored_credentials`. Exit zero alone is not success: this command also exits zero with `authenticated: false`. Authentication status reports available credentials; it does not validate API access, credit or account policy.

If `authenticated` is false, tell the user to run `parallel-cli login` in their terminal or set `PARALLEL_API_KEY` in the environment inherited by Cursor. Never request or print credentials in chat.

If `method` is `environment`, `PARALLEL_API_KEY` overrides stored login. Any `selected_org_id` or `selected_org_name` describes the inactive stored login, not the environment key's organization. Report that distinction. The environment key's billing organization must be verified independently before an account-specific or paid test; do not claim that it belongs to the stored organization. Do not switch accounts or remove overrides automatically.

If `method` is `oauth`, selected organization metadata describes the active stored login. Report only nonsecret account metadata needed for the request.

## Verify readiness

Repeat `parallel-cli auth --json` in the agent's terminal and confirm the binary, required feature help and `authenticated` boolean. State the active credential source and any unverified organization or API-access limitation. Do not start a paid job or add funds merely to prove setup. For a `403`, inspect the actual permissions, policy or billing error before suggesting a remedy.
