---
name: parallel-setup
description: Set up the Parallel plugin (install CLI and authenticate)
---

# Parallel Plugin Setup

## Step 1: Check if CLI is already installed

```bash
parallel-cli --version
```

If this prints a version, skip to **Step 2: Authenticate**.

## Step 1b: Install CLI (requires user's terminal)

The CLI cannot be installed from within Cursor's sandbox. Tell the user to run one of these commands in their own terminal:

**Option A — install script:**
```
curl -fsSL https://parallel.ai/install.sh | bash
```

**Option B — pipx:**
```
pipx install "parallel-web-tools[cli]"
pipx ensurepath
```

After installing, they may need to add `~/.local/bin` to PATH in their shell config (e.g. `~/.zshrc`).

Do NOT attempt to run the install commands here. Wait for the user to confirm they've installed it, then re-run `/parallel-setup` to verify.

## Step 2: Authenticate

Check if already authenticated:

```bash
parallel-cli auth
```

If not authenticated, tell the user to run `parallel-cli login` in their terminal, or set `PARALLEL_API_KEY` in their environment.

## Step 3: Verify

```bash
parallel-cli auth
```

Confirm the CLI is installed, authenticated, and ready to use.
