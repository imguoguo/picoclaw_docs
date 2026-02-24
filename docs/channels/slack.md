---
id: slack
title: Slack
---

# Slack

Slack integration uses Socket Mode, so no public IP is needed.

## Setup

### 1. Create a Slack App

- Go to [api.slack.com/apps](https://api.slack.com/apps) → Create New App → From scratch
- Choose your workspace

### 2. Enable Socket Mode

- App Home → Socket Mode → Enable Socket Mode
- Create an **App-Level Token** with `connections:write` scope
- Copy the app token (starts with `xapp-`)

### 3. Add Bot Token Scopes

- OAuth & Permissions → Bot Token Scopes → Add:
  - `chat:write`
  - `im:history`
  - `im:read`
  - `app_mentions:read`

### 4. Install to Workspace

- OAuth & Permissions → Install to Workspace
- Copy the **Bot User OAuth Token** (starts with `xoxb-`)

### 5. Enable Event Subscriptions

- Event Subscriptions → Enable Events
- Subscribe to bot events:
  - `message.im`
  - `app_mention`

### 6. Configure

```json
{
  "channels": {
    "slack": {
      "enabled": true,
      "bot_token": "xoxb-YOUR-BOT-TOKEN",
      "app_token": "xapp-YOUR-APP-TOKEN",
      "allow_from": []
    }
  }
}
```

### 7. Run

```bash
picoclaw gateway
```
