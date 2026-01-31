---
title: "Non-Developer Guide: Complete OpenClaw Setup on Mac"
description: "Complete guide for Mac users - from OpenClaw installation to Discord bot integration"
pubDate: 2026-02-01T10:00:00+09:00
category: "AI"
---

## 🎯 Goal of This Guide

Follow this single guide to **completely** set up OpenClaw on Mac, from installation to Discord bot integration. Written for non-developers, so don't worry and follow along.

Estimated time: **30 minutes to 1 hour**

---

## 📋 Requirements

- Mac computer (macOS 12 Monterey or later recommended)
- Internet connection
- AI API key (Claude or OpenAI) - instructions below if you don't have one

---

## 🔧 Step 1: Open Terminal

Terminal is Mac's "command input window". It looks scary but it's just a text input window.

**How to open:**
1. Press `Cmd + Space` to open Spotlight search
2. Type "Terminal"
3. Press Enter to launch

If you see a window with a blinking text cursor on a black (or white) background, you're good.

**💡 Tip**: You'll use Terminal often, so pin it to your Dock. Right-click Terminal icon → "Options" → "Keep in Dock".

---

## 🍺 Step 2: Install Homebrew

Homebrew is a package manager for Mac. Like an App Store but for developer tools. You need this before installing OpenClaw.

### Check if Already Installed

Type in Terminal and press Enter:

```bash
brew --version
```

If you see version info like `Homebrew 4.x.x`, **it's already installed** - skip to Step 3.

If you see `command not found: brew`, you need to install it.

### Installing Homebrew

Copy and paste this **entire** command into Terminal:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**What happens:**
1. Press Enter to start installation
2. It will ask for your Mac login password (it won't show as you type - that's normal)
3. When "Press RETURN to continue" appears, press Enter
4. Installation takes about 5-10 minutes

### ⚠️ Required for Apple Silicon Mac (M1/M2/M3) Users

After installation, you might see this message:

```
==> Next steps:
- Run these commands in your terminal to add Homebrew to your PATH
```

Run these commands **in order**:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

If you skip this, the `brew` command won't work. This is important.

### Verify Installation

```bash
brew --version
```

If you see version info, success!

---

## 🦞 Step 3: Install OpenClaw

Finally, the main event. Type in Terminal:

```bash
brew install openclaw
```

Installation takes about 2-3 minutes. Lots of text scrolling by is normal.

### Verify Installation

```bash
openclaw --version
```

If you see version info, installation complete!

---

## 🔑 Step 4: Set Up API Key

OpenClaw needs an AI service API key to use AI features. Choose either Claude (Anthropic) or OpenAI.

### Get Claude API Key (Recommended)

1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Sign up or log in
3. Click "API Keys" in the left menu
4. Click "Create Key" button
5. Enter a name and create
6. Copy the generated key (it's only shown once - save it!)

### Get OpenAI API Key

1. Go to [platform.openai.com](https://platform.openai.com)
2. Sign up or log in
3. Click profile (top right) → "View API keys"
4. Click "Create new secret key"
5. Copy the generated key

### Register API Key

Create OpenClaw config file in Terminal:

```bash
mkdir -p ~/.openclaw
nano ~/.openclaw/config.yaml
```

When nano editor opens, enter (for Claude):

```yaml
ai:
  provider: anthropic
  api_key: sk-ant-xxxxx  # Enter your API key here
```

For OpenAI:

```yaml
ai:
  provider: openai
  api_key: sk-xxxxx  # Enter your API key here
```

Save and exit: `Ctrl + X` → `Y` → `Enter`

**💡 Safer Method**: Use environment variables

```bash
echo 'export ANTHROPIC_API_KEY="sk-ant-xxxxx"' >> ~/.zshrc
source ~/.zshrc
```

This way you don't need to write the key directly in the config file.

---

## 🚀 Step 5: Run OpenClaw

In Terminal:

```bash
openclaw
```

If you see a welcome message, success! Now you can ask the AI to do anything.

### First Test

In the OpenClaw prompt, try:

```
Show me the files in the current folder
```

If you see a file list, it's working properly.

---

## 🤖 Step 6: Discord Bot Integration (Optional)

Connecting OpenClaw as a Discord bot lets you command the AI from Discord. This enables automation like receiving daily news summaries.

### Create Discord Bot

1. Go to [Discord Developer Portal](https://discord.com/developers/applications)
2. Click "New Application"
3. Enter a name (e.g., "MyOpenClaw") and create
4. Click "Bot" in the left menu
5. Click "Add Bot" → "Yes, do it!"
6. Click "Reset Token" to copy the bot token (only shown once!)

### Set Bot Permissions

1. On the same page, find "Privileged Gateway Intents" section
2. Enable "Message Content Intent" (important!)
3. Save

### Invite Bot to Server

1. Click "OAuth2" → "URL Generator" in the left menu
2. Check "bot" in SCOPES
3. Check required permissions in BOT PERMISSIONS:
   - Send Messages
   - Read Message History
   - Use Slash Commands
4. Copy the generated URL and paste in browser
5. Select server to add bot and authorize

### Connect Discord to OpenClaw

Add to config.yaml:

```yaml
integrations:
  discord:
    enabled: true
    bot_token: "your_bot_token_here"
    prefix: "!"  # Symbol before commands
```

### Run Bot

```bash
openclaw --discord
```

If the bot responds to `!help` in your Discord server, success!

---

## 🔥 Troubleshooting: Common Issues

### ❌ "command not found: brew"

**Cause**: Homebrew PATH not set (especially on M1/M2/M3 Macs)

**Solution**:
```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
source ~/.zprofile
```

### ❌ "command not found: openclaw"

**Cause**: Installation failed or Terminal needs restart

**Solution**:
```bash
# Close Terminal completely and reopen
# If still not working, reinstall
brew reinstall openclaw
```

### ❌ "API key invalid" or Authentication Error

**Cause**: API key is wrong or expired

**Solution**:
1. Double-check API key (no extra spaces when copying)
2. Verify key is active in API service dashboard
3. Check if payment info is registered (free credits might be exhausted)

### ❌ "Permission denied"

**Cause**: Permission issue

**Solution**:
```bash
sudo chown -R $(whoami) ~/.openclaw
```

### ❌ Discord Bot Not Responding

**Cause**: Multiple possibilities

**Checklist**:
1. Verify bot is properly invited to server
2. Check "Message Content Intent" is enabled
3. Verify bot token is correct
4. Confirm OpenClaw is running with `--discord` option

### ❌ Error During Homebrew Installation

**Cause**: Xcode Command Line Tools not installed

**Solution**:
```bash
xcode-select --install
```

Click "Install" when popup appears, then reinstall Homebrew after completion.

---

## 🎉 Installation Complete! What's Next?

1. **Learn Basic Usage**: Start with simple requests ("What's today's date?", "Create a test.txt file in this folder")
2. **Apply to Projects**: Run OpenClaw in your actual work folders
3. **Set Up Automation**: Find ways to automate your frequent tasks

---

## 💡 Security Pro Tip

> "When running Terminal commands on Mac, use `sudo` carefully. `sudo` runs commands with admin privileges - wrong commands with sudo can destroy your system. Especially dangerous to blindly add sudo to commands copied from the internet. Understand what a command does before running it. And never share your API keys with anyone!"
