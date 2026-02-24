---
id: feishu
title: Feishu / Lark (飞书)
---

# Feishu / Lark

## Setup

### 1. Create a Feishu App

- Go to [Feishu Open Platform](https://open.feishu.cn/)
- Create a custom app
- Note the **App ID** and **App Secret**

### 2. Configure Permissions

In your app settings, add the following bot permissions:
- `im:message:receive_v1`
- `im:message`

### 3. Enable Message Events

- Add an **Encrypt Key** and **Verification Token** in Event Subscription settings
- Subscribe to the `im.message.receive_v1` event

### 4. Configure PicoClaw

```json
{
  "channels": {
    "feishu": {
      "enabled": true,
      "app_id": "cli_xxx",
      "app_secret": "YOUR_APP_SECRET",
      "encrypt_key": "YOUR_ENCRYPT_KEY",
      "verification_token": "YOUR_VERIFICATION_TOKEN",
      "allow_from": []
    }
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| `app_id` | string | Feishu app ID (starts with `cli_`) |
| `app_secret` | string | Feishu app secret |
| `encrypt_key` | string | Event encryption key |
| `verification_token` | string | Event verification token |
| `allow_from` | array | Allowed user open IDs |

### 5. Run

```bash
picoclaw gateway
```

:::note
Full Feishu documentation is available in Chinese in the repository at `docs/channels/feishu/README.zh.md`.
:::
