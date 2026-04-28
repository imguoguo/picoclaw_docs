---
id: pico
title: Pico Native Channel
---

# Pico Native Channel

Pico 通道是 PicoClaw 面向自定义客户端的原生 WebSocket 协议。同一个包还提供 `pico_client` 模式，用于连接远程 Pico 服务器。

## 设置

- 当本 gateway 托管 WebSocket 端点时使用 `pico`。
- 当此实例连接远程端点时使用 `pico_client`。
- 仅与可信客户端共享配置的 token。

## 配置示例

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

## 字段参考

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `token` | string | 共享的 Pico 协议 token。 |
| `allow_token_query` | bool | 允许无法设置 header 的客户端通过 URL query 传递 token。 |
| `allow_origins` | string[] | 允许的 WebSocket origin。 |
| `ping_interval` | int | Ping 间隔，单位为秒。 |
| `read_timeout` / `write_timeout` | int | 连接超时设置。 |
| `max_connections` | int | 最大并发客户端连接数。 |
| `url` | string | `pico_client` 的远程服务器 URL。 |
| `session_id` | string | 可选客户端会话 ID。 |

## 通道通用字段

PicoClaw 现在把通道定义存放在 `channel_list` 下。每个条目在通道层包含通用字段，并在 `settings` 中包含通道专属字段。

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `enabled` | bool | 启用此通道条目。 |
| `type` | string | 通道实现类型。当 map key 与通道类型不同时使用。 |
| `allow_from` | string[] | 发送者白名单。为空表示允许所有发送者。 |
| `reasoning_channel_id` | string | 可选的推理/思考输出聊天或通道 ID。 |
| `group_trigger.mention_only` | bool | 在群聊中仅在被提及时回复。 |
| `group_trigger.prefixes` | string[] | 在群聊中仅响应配置的前缀。 |
| `typing.enabled` | bool | 通道支持时发送输入状态提示。 |
| `placeholder.enabled` | bool | Agent 工作时发送占位消息。 |
| `placeholder.text` | string 或 string[] | 占位文本。多个条目会随机选择。 |

旧版扁平 `channels` 条目仍会在加载时迁移，但新文档和生成的配置应使用 `channel_list`。

## 运行

```bash
picoclaw gateway
```
