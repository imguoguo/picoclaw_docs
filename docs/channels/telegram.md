---
id: telegram
title: Telegram
---

# Telegram

Telegram is the **recommended** channel. It's easy to set up and supports voice transcription.

## Setup

### 1. Create a Bot

- Open Telegram and search for `@BotFather`
- Send `/newbot`, follow the prompts
- Copy the bot token

### 2. Get Your User ID

- Message `@userinfobot` on Telegram
- Copy your User ID

### 3. Configure

```json
{
  "channels": {
    "telegram": {
      "enabled": true,
      "token": "YOUR_BOT_TOKEN",
      "allow_from": ["YOUR_USER_ID"]
    }
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| `enabled` | bool | Enable/disable the channel |
| `token` | string | Bot token from @BotFather |
| `proxy` | string | HTTP proxy URL (optional) |
| `allow_from` | array | List of allowed user IDs (empty = allow all) |

### 4. Run

```bash
picoclaw gateway
```

## Voice Transcription

Telegram voice messages can be automatically transcribed using Groq's Whisper:

```json
{
  "model_list": [
    {
      "model_name": "whisper",
      "model": "groq/whisper-large-v3",
      "api_key": "gsk_..."
    }
  ]
}
```

Get a free Groq API key at [console.groq.com](https://console.groq.com).

## Troubleshooting

**"Conflict: terminated by other getUpdates"**: Only one `picoclaw gateway` can run at a time. Stop any other instances.

**Proxy**: If Telegram is blocked in your region, use the `proxy` field:

```json
{
  "channels": {
    "telegram": {
      "proxy": "http://127.0.0.1:7890"
    }
  }
}
```
