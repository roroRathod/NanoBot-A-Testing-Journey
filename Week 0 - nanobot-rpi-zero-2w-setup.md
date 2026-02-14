# NanoBot on Raspberry Pi Zero 2W

### Setup & Installation Guide (My Exact Environment)

This document outlines the exact steps I followed to install and run **NanoBot** on a Raspberry Pi Zero 2W (512MB RAM).

Official repository:
👉 [https://github.com/HKUDS/nanobot](https://github.com/HKUDS/nanobot)

I followed the official documentation and adapted it for a low-memory edge environment.

---

## Hardware Used

* Raspberry Pi Zero 2W (512MB RAM)
* 32GB microSD card
* Raspberry Pi OS (Lite recommended)
* Internet connection
* OpenRouter API key (for LLM access)

---

## Step 1: System Preparation

Update your system first:

```bash
sudo apt update && sudo apt upgrade -y
```

---

## Step 2: Install Git

```bash
sudo apt install git -y
```

Verify:

```bash
git --version
```

---

## Step 3: Ensure Python Is Installed

NanoBot requires Python 3.10+ (recommended 3.11 if available).

Check version:

```bash
python3 --version
```

If not installed:

```bash
sudo apt install python3 python3-pip -y
```

---

## Step 4: Install `uv` (Python Package Manager)

NanoBot recommends using **uv** for tool installation.

Install uv:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Restart your shell or run:

```bash
source $HOME/.cargo/env
```

Verify installation:

```bash
uv --version
```

---

## Step 5: Install NanoBot

Install NanoBot globally using uv:

```bash
uv tool install nanobot-ai
```

Verify:

```bash
nanobot --help
```

If you see CLI commands listed, installation succeeded.

---

## Step 6: Initialize NanoBot (First Run)

Run:

```bash
nanobot onboard
```

This creates:

```
~/.nanobot/
  ├── config.json
  ├── workspace/
  ├── AGENTS.md
  ├── SOUL.md
  ├── USER.md
  └── memory/
```

You should see confirmation that configuration and workspace were created.

---

## Step 7: Configure LLM Provider

Edit:

```bash
nano ~/.nanobot/config.json
```

Replace contents with:

```json
{
  "providers": {
    "openrouter": {
      "apiKey": "sk-or-v1-xxx"
    }
  },
  "agents": {
    "defaults": {
      "model": "free model"
    }
  }
}
```

### Notes:

* Replace `"sk-or-v1-xxx"` with your OpenRouter API key.
* Use a free-tier model available via OpenRouter to minimize cost during experimentation.
* This keeps resource usage light for Raspberry Pi Zero 2W.

Save and exit.

---

## Step 8: Test NanoBot via CLI

Run:

```bash
nanobot agent -m "Hello"
```

If configured correctly, you should receive a response from the model.

On Raspberry Pi Zero 2W, I observed:

* ~0.5 second response times (network dependent)
* Stable memory usage
* No crashes under basic CLI usage

---

## Optional: Check Status

```bash
nanobot status
```

This confirms:

* Config path
* Workspace path
* Active model
* API key status

---

## Architecture Reference

NanoBot supports:

* Tool execution (shell, file system, web search)
* Subagents
* Cron scheduling
* Multi-provider LLM routing
* Telegram / WhatsApp integration

Official Documentation:
👉 [https://github.com/HKUDS/nanobot](https://github.com/HKUDS/nanobot)

I followed the official repo instructions and validated them under constrained hardware conditions.

---

## Resource Considerations (Pi Zero 2W)

* 512MB RAM is sufficient for CLI usage.
* Avoid running heavy background services.
* Prefer lightweight Raspberry Pi OS Lite.
* Use free or small models during testing.
* Monitor memory using:

```bash
htop
```

---

## Current Testing Scope

So far tested:

* CLI chat interaction
* Configuration introspection
* Workspace access
* Basic tool execution

Next phases will include:

* Programming assistance
* Stress testing under load
* Voice integration
* Long-running task execution

---

## Summary

NanoBot can run reliably on Raspberry Pi Zero 2W with:

* Minimal system footprint (~100MB)
* Fast response times (~0.5s CLI)
* Stable execution under light workloads

This confirms that agentic AI frameworks do not necessarily require high-end hardware for experimentation.
