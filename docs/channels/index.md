---
id: index
title: Chat Channels
sidebar_label: Overview
---

# Chat Channels

Connect PicoClaw to messaging platforms through its **gateway** mode.

```bash
picoclaw gateway
```

## Supported Channels

| Channel | Difficulty | Notes |
| --- | --- | --- |
| **Telegram** | Easy | Recommended. Supports voice transcription with Groq. |
| **Discord** | Easy | Bot token + intents. Supports mention-only mode. |
| **Slack** | Easy | Socket mode, no public IP needed. |
| **QQ** | Easy | Official QQ bot API (AppID + AppSecret). |
| **DingTalk** | Medium | Stream mode, no public IP needed. |
| **WeCom Bot** | Medium | Group chat via webhook. |
| **WeCom App** | Hard | Private chat, more features, requires HTTPS. |
| **Feishu** | Hard | Enterprise collaboration platform. |
| **LINE** | Hard | Requires HTTPS webhook. |
| **OneBot** | Medium | Compatible with NapCat/Go-CQHTTP. |
| **MaixCam** | Easy | Hardware-integrated AI camera. |

## How It Works

1. Configure one or more channels in `~/.picoclaw/config.json` under the `channels` key
2. Set `"enabled": true` for each channel you want to use
3. Run `picoclaw gateway` to start listening
4. The gateway handles all channels concurrently

## Access Control

All channels support the `allow_from` field to restrict access to specific users:

```json
{
  "channels": {
    "telegram": {
      "enabled": true,
      "token": "YOUR_TOKEN",
      "allow_from": ["123456789"]
    }
  }
}
```

Set `allow_from` to an empty array `[]` to allow all users.
