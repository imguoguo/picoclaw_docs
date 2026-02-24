---
id: discord
title: Discord
---

# Discord

## Setup

### 1. Create a Bot

- Go to [discord.com/developers/applications](https://discord.com/developers/applications)
- Create an application → Bot → Add Bot
- Copy the bot token

### 2. Enable Intents

- In Bot settings, enable **MESSAGE CONTENT INTENT**
- (Optional) Enable **SERVER MEMBERS INTENT** for member-based allow lists

### 3. Get Your User ID

- Discord Settings → Advanced → enable **Developer Mode**
- Right-click your avatar → **Copy User ID**

### 4. Configure

```json
{
  "channels": {
    "discord": {
      "enabled": true,
      "token": "YOUR_BOT_TOKEN",
      "allow_from": ["YOUR_USER_ID"],
      "mention_only": false
    }
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| `enabled` | bool | Enable/disable the channel |
| `token` | string | Bot token from Discord Developer Portal |
| `allow_from` | array | List of allowed user IDs (empty = allow all) |
| `mention_only` | bool | Only respond when @-mentioned |

### 5. Invite the Bot

- OAuth2 → URL Generator
- Scopes: `bot`
- Bot Permissions: `Send Messages`, `Read Message History`
- Open the generated invite URL and add the bot to your server

### 6. Run

```bash
picoclaw gateway
```

## Mention-Only Mode

Set `"mention_only": true` to make the bot respond only when @-mentioned. Useful for shared servers where you want the bot to be less intrusive.
