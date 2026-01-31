---
title: "Non-Developer Guide: Complete OpenClaw Setup on Windows"
description: "Complete guide for Windows users - from OpenClaw installation to Discord bot integration"
pubDate: 2026-02-01T11:00:00+09:00
category: "AI"
---

## 🎯 Goal of This Guide

Follow this single guide to **completely** set up OpenClaw on Windows, from installation to Discord bot integration. Written for non-developers, so don't worry and follow along.

Estimated time: **30 minutes to 1 hour**

---

## 📋 Requirements

- Windows 10 (version 1809 or later) or Windows 11
- Internet connection
- AI API key (Claude or OpenAI) - instructions below if you don't have one

---

## 🔧 Step 1: Open PowerShell

PowerShell is Windows' "advanced command input window". More powerful than Command Prompt (cmd).

**How to open:**
1. Press the `Windows key`
2. Type "PowerShell"
3. Right-click **"Windows PowerShell"** → Click **"Run as administrator"**
4. Click "Yes" when asked "Do you want to allow this app to make changes to your device?"

If you see a blue window with text like `PS C:\Users\YourName>`, you're good.

**⚠️ Important**: You MUST run as **administrator**. Running normally will cause permission errors during installation.

**💡 Tip**: You'll use PowerShell often, so pin it to your taskbar. Right-click icon → "Pin to taskbar".

---

## 📦 Step 2: Check and Install winget

winget is a package manager for Windows. It comes pre-installed on Windows 11, but Windows 10 needs verification.

### Check winget Installation

Type in PowerShell and press Enter:

```powershell
winget --version
```

If you see version info like `v1.x.xxxxx`, **it's already installed** - skip to Step 3.

If you see `'winget' is not recognized...` error, you need to install it.

### Installing winget (Windows 10)

1. Open Microsoft Store app
2. Search for "App Installer"
3. Click "App Installer" → Click "Update" or "Install" button
4. After installation, **close PowerShell completely and reopen** (as administrator)

### Verify Again

```powershell
winget --version
```

If you see version info, success!

---

## 🦞 Step 3: Install OpenClaw

Finally, the main event. Type in PowerShell:

```powershell
winget install openclaw
```

**What happens:**
1. Type `Y` and press Enter when asked "Do you agree?"
2. Installation proceeds (2-5 minutes)
3. Done when you see "Successfully installed" message

### Verify Installation

**Close PowerShell and reopen** (as administrator):

```powershell
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
6. Copy the generated key (only shown once - save it in Notepad!)

### Get OpenAI API Key

1. Go to [platform.openai.com](https://platform.openai.com)
2. Sign up or log in
3. Click profile (top right) → "View API keys"
4. Click "Create new secret key"
5. Copy the generated key

### Register API Key

Create config folder in PowerShell:

```powershell
mkdir $env:USERPROFILE\.openclaw -Force
notepad $env:USERPROFILE\.openclaw\config.yaml
```

When Notepad opens, enter (for Claude):

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

Save: `Ctrl + S` → Close Notepad

**💡 Safer Method**: Use environment variables

1. Search "environment variables" in Windows search
2. Click "Edit the system environment variables"
3. Click "Environment Variables" button
4. Under "User variables", click "New"
5. Variable name: `ANTHROPIC_API_KEY` (or `OPENAI_API_KEY`)
6. Variable value: Paste your API key
7. OK → OK → OK
8. **Close PowerShell completely and reopen**

This way you don't need to write the key directly in the config file.

---

## 🚀 Step 5: Run OpenClaw

In PowerShell:

```powershell
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
6. Click "Reset Token" to copy the bot token (only shown once - save in Notepad!)

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

Open config.yaml file:

```powershell
notepad $env:USERPROFILE\.openclaw\config.yaml
```

Add the following:

```yaml
integrations:
  discord:
    enabled: true
    bot_token: "your_bot_token_here"
    prefix: "!"  # Symbol before commands
```

Save and close.

### Run Bot

```powershell
openclaw --discord
```

If the bot responds to `!help` in your Discord server, success!

---

## 🔥 Troubleshooting: Common Issues

### ❌ "'winget' is not recognized..."

**Cause**: winget not installed

**Solution**:
1. Install/update "App Installer" from Microsoft Store
2. Close PowerShell completely and reopen
3. If still not working, restart PC

### ❌ "'openclaw' is not recognized..."

**Cause**: PATH not applied after installation

**Solution**:
1. Close PowerShell completely and reopen (as administrator)
2. If still not working, restart PC
3. If still not working after restart:
```powershell
winget uninstall openclaw
winget install openclaw
```

### ❌ "Access is denied" or Permission Error

**Cause**: Running without administrator privileges

**Solution**:
1. Reopen PowerShell **as administrator**
2. If still not working, check if Windows Defender or antivirus is blocking

### ❌ "API key invalid" or Authentication Error

**Cause**: API key is wrong or expired

**Solution**:
1. Double-check API key (no extra spaces when copying)
2. Verify key is active in API service dashboard
3. Check if payment info is registered (free credits might be exhausted)

### ❌ Can't Find config.yaml File

**Cause**: File path issue

**Solution**:
```powershell
# Check config folder
dir $env:USERPROFILE\.openclaw

# Create folder if it doesn't exist
mkdir $env:USERPROFILE\.openclaw -Force
```

### ❌ Discord Bot Not Responding

**Cause**: Multiple possibilities

**Checklist**:
1. Verify bot is properly invited to server
2. Check "Message Content Intent" is enabled
3. Verify bot token is correct
4. Confirm OpenClaw is running with `--discord` option
5. Check if firewall is blocking

### ❌ Windows Defender Blocking

**Cause**: New program looks suspicious

**Solution**:
1. Windows Security → Virus & threat protection
2. Find OpenClaw-related item in Protection history
3. Select "Allow"

---

## 🎉 Installation Complete! What's Next?

1. **Learn Basic Usage**: Start with simple requests ("What's today's date?", "Create a test.txt file in this folder")
2. **Apply to Projects**: Run OpenClaw in your actual work folders
3. **Set Up Automation**: Find ways to automate your frequent tasks

---

## 💡 Security Pro Tip

> "'Run as administrator' on Windows is a double-edged sword. You need it for installation, but for regular OpenClaw use, running with normal privileges is safer. With admin privileges, AI can touch system files. Also, never share API keys via chat or email. If leaked, you'll face a massive bill. Setting up environment variables is much safer than writing directly in config files!"
