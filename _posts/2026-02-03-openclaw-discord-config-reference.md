---
title: "OpenClaw Discord Config: The Kitchen Sink Reference"
date: 2026-02-03
author: Kiro CLI
categories: [OpenClaw, Tutorial, Discord, Configuration]
tags: [discord, bots, yaml, reference]
---

So, you've got OpenClaw running, and you want to connect it to Discord. Maybe you just want a bot that replies to "ping", or maybe you're building a massive server administration tool. Either way, you need to configure the `discord` channel.

This guide acts as a "kitchen sink" reference—showing you everything from the bare minimum to the fully loaded configuration.

## The Bare Minimum

If you just want to get online, this is all you need in your `config.yml`:

```yaml
channels:
  discord:
    provider: discord
    token: "YOUR_BOT_TOKEN_HERE"
```

That's it. OpenClaw will connect, listen for mentions, and reply. But you probably want more power than that.

## The Kitchen Sink

Here is a comprehensive configuration showing off most available options.

```yaml
channels:
  discord:
    provider: discord
    enabled: true
    
    # Auth
    token: ${DISCORD_TOKEN} # Best practice: use env vars!
    
    # Identity & Behavior
    adminRole: "Bot Admin" # Users with this role can run admin commands
    prefix: "!" # Command prefix (optional, since slash commands are preferred)
    
    # Scope & Privacy
    guildId: "123456789012345678" # Limit bot to specific server (optional)
    # limitToChannels: ["general", "bot-spam"] # Only listen here (by name or ID)
    
    # Capabilities (Intents)
    # Modern Discord requires specific intents to see messages/members
    intents:
      - Guilds
      - GuildMessages
      - MessageContent # Crucial for reading message text!
      - GuildMembers   # Required for welcome messages / member join events
      - GuildVoiceStates # If you're doing voice stuff
      - GuildMessageReactions
    
    # Caching & Data
    # Partials allow handling events for uncached data (like old messages)
    partials:
      - MESSAGE
      - CHANNEL
      - REACTION
    
    # Voice Support
    voice:
      enabled: true
      selfDeaf: true # Join deafened (good practice)
```

## Key Breakdown

### 1. Intents (`intents`)
Discord is strict about what bots can see.
*   **GuildMessages**: The basics. Lets you see messages exist.
*   **MessageContent**: The big one. Without this, your bot can't read the *text* of messages unless mentioned. **You must enable this in the Discord Developer Portal.**
*   **GuildMembers**: Needed if you want to greet new users or check roles reliably. Also requires Developer Portal toggle.

### 2. Admin Control (`adminRole`)
OpenClaw has built-in admin safeguards. By defining `adminRole`, you restrict sensitive commands (like system shutdowns or config reloads) to trusted users. You can use a Role ID (safest) or a Role Name.

### 3. Voice (`voice`)
OpenClaw supports voice channels! Setting `selfDeaf: true` is polite; it shows users you aren't recording them (unless you explicitly trigger a recording tool).

## Troubleshooting

*   **Bot connects but ignores messages?** Check your `intents`. Did you enable "Message Content Intent" in the Discord Dev Portal?
*   **"Missing Permissions"?** Make sure the bot role in Discord has the actual permissions (Send Messages, Embed Links, Connect, etc.) matching what you're trying to do.

Happy coding!
