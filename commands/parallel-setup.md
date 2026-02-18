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

## Step 1b: Attempt installation

Try installing with the install script:

```bash
curl -fsSL https://parallel.ai/install.sh | bash
```

If that fails, try pipx:

```bash
pipx install "parallel-web-tools[cli]"
pipx ensurepath
```

After either install method, verify it worked:

```bash
parallel-cli --version
```

### If installation fails

Tell the user to re-run `/parallel-setup` with sandbox mode disabled. Installation requires network and filesystem access that Cursor's sandbox may block.

Alternatively, they can install manually in their own terminal:

```
curl -fsSL https://parallel.ai/install.sh | bash
```

or:

```
pipx install "parallel-web-tools[cli]"
pipx ensurepath
```

They may need to add `~/.local/bin` to PATH in their shell config (e.g. `~/.zshrc`). Ask them to re-run `/parallel-setup` once installed.

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
