---
id: pico
title: Pico Native Channel
---

# Pico Native Channel

The Pico channel is PicoClaw’s native WebSocket protocol for custom clients. The same package also provides a `pico_client` mode for connecting to a remote Pico server.

## Setup

- Use `pico` when this gateway hosts the WebSocket endpoint.
- Use `pico_client` when this instance connects to a remote endpoint.
- Share the configured token with trusted clients only.

## Configuration Example

```json
{
  "channel_list": {
    "pico": {
      "enabled": true,
      "type": "pico",
      "allow_from": [],
      "settings": {
        "token": "YOUR_PICO_TOKEN",
        "allow_token_query": false,
        "allow_origins": [],
        "ping_interval": 30,
        "read_timeout": 60,
        "write_timeout": 0,
        "max_connections": 100
      }
    },
    "pico_client": {
      "enabled": false,
      "type": "pico_client",
      "allow_from": [],
      "settings": {
        "url": "wss://remote-pico-server/pico/ws",
        "token": "YOUR_PICO_TOKEN",
        "session_id": "",
        "ping_interval": 30,
        "read_timeout": 60
      }
    }
  }
}
```

## Field Reference

| Field | Type | Description |
| --- | --- | --- |
| `token` | string | Shared Pico protocol token. |
| `allow_token_query` | bool | Allow token in URL query for clients that cannot set headers. |
| `allow_origins` | string[] | Allowed WebSocket origins. |
| `ping_interval` | int | Ping interval in seconds. |
| `read_timeout` / `write_timeout` | int | Connection timeout settings. |
| `max_connections` | int | Maximum concurrent client connections. |
| `url` | string | Remote server URL for `pico_client`. |
| `session_id` | string | Optional client session ID. |

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
