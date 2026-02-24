---
id: qq
title: QQ
---

# QQ

## Setup

### 1. Create a Bot

- Go to [QQ Open Platform](https://q.qq.com/#)
- Create an application → Get **AppID** and **AppSecret**

### 2. Configure

```json
{
  "channels": {
    "qq": {
      "enabled": true,
      "app_id": "YOUR_APP_ID",
      "app_secret": "YOUR_APP_SECRET",
      "allow_from": []
    }
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| `app_id` | string | QQ bot AppID |
| `app_secret` | string | QQ bot AppSecret |
| `allow_from` | array | List of allowed QQ user IDs (empty = allow all) |

### 3. Run

```bash
picoclaw gateway
```
