---
id: dingtalk
title: DingTalk (钉钉)
---

# DingTalk

DingTalk integration uses Stream Mode, so no public IP is needed.

## Setup

### 1. Create a Bot

- Go to [Open Platform](https://open.dingtalk.com/)
- Create an internal app
- Copy **Client ID** and **Client Secret**

### 2. Configure

```json
{
  "channels": {
    "dingtalk": {
      "enabled": true,
      "client_id": "YOUR_CLIENT_ID",
      "client_secret": "YOUR_CLIENT_SECRET",
      "allow_from": []
    }
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| `client_id` | string | DingTalk app Client ID |
| `client_secret` | string | DingTalk app Client Secret |
| `allow_from` | array | List of allowed DingTalk user IDs (empty = allow all) |

### 3. Run

```bash
picoclaw gateway
```
