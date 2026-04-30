---
id: whatsapp_native
title: WhatsApp Native
---

# WhatsApp Native

`whatsapp_native` is the native WhatsApp channel type. It shares the same settings shape as `whatsapp` but is used when you want the native implementation as an explicit channel entry.

## Setup

- Set the channel type to `whatsapp_native`.
- Choose a persistent `session_store_path`.
- Start the gateway and complete any required WhatsApp pairing flow.

## Configuration Example

```json
{
  "channel_list": {
    "whatsapp_native": {
      "enabled": true,
      "type": "whatsapp_native",
      "allow_from": [],
      "settings": {
        "bridge_url": "",
        "use_native": true,
        "session_store_path": "~/.picoclaw/whatsapp-native.db"
      }
    }
  }
}
```

## Field Reference

| Field | Type | Description |
| --- | --- | --- |
| `use_native` | bool | Should be true for native mode. |
| `session_store_path` | string | Persistent session database path. |
| `bridge_url` | string | Unused in native-only deployments. |

## Common Channel Fields

PicoClaw now stores channel definitions under `channel_list`. Each entry has common fields at the channel level and channel-specific fields in `settings`.

| Field | Type | Description |
| --- | --- | --- |
| `enabled` | bool | Enable this channel entry. |
| `type` | string | Channel implementation type. Use this when the map key is not the same as the channel type. |
| `allow_from` | string[] | Sender allowlist. Empty means allow all senders. |
| `reasoning_channel_id` | string | Optional chat/channel ID for reasoning or thinking output. |
| `group_trigger.mention_only` | bool | In group chats, respond only when mentioned. |
| `group_trigger.prefixes` | string[] | In group chats, respond only to configured prefixes. |
| `typing.enabled` | bool | Send typing indicators when the channel supports them. |
| `placeholder.enabled` | bool | Send a placeholder message while the agent is working. |
| `placeholder.text` | string or string[] | Placeholder text. Multiple entries are chosen randomly. |

Legacy flat `channels` entries are still migrated on load, but new docs and generated config should use `channel_list`.

## Run

```bash
picoclaw gateway
```
