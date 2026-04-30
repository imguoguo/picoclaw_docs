---
id: irc
title: IRC
---

# IRC

IRC connects PicoClaw to one or more IRC channels. It supports TLS, NickServ/SASL authentication, group triggers, and typing indicators when the server supports message tags.

## Setup

- Choose an IRC server and bot nickname.
- Add one or more channel names.
- Configure authentication fields only if your network requires them.

## Configuration Example

```json
{
  "channel_list": {
    "irc": {
      "enabled": true,
      "type": "irc",
      "allow_from": [],
      "group_trigger": { "mention_only": true },
      "typing": { "enabled": false },
      "settings": {
        "server": "irc.libera.chat:6697",
        "tls": true,
        "nick": "mybot",
        "user": "",
        "real_name": "",
        "password": "",
        "nickserv_password": "",
        "sasl_user": "",
        "sasl_password": "",
        "channels": ["#mychannel"],
        "request_caps": ["server-time", "message-tags"]
      }
    }
  }
}
```

## Field Reference

| Field | Type | Description |
| --- | --- | --- |
| `server` | string | IRC server host:port. |
| `tls` | bool | Use TLS. |
| `nick` | string | Bot nickname. |
| `user` | string | Optional username. |
| `real_name` | string | Optional real name. |
| `password` | string | Optional server password. |
| `nickserv_password` | string | Optional NickServ password. |
| `sasl_user` / `sasl_password` | string | Optional SASL credentials. |
| `channels` | string[] | IRC channels to join. |
| `request_caps` | string[] | IRC capabilities to request. |

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
