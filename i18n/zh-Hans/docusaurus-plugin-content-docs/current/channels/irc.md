---
id: irc
title: IRC
---

# IRC

IRC 将 PicoClaw 连接到一个或多个 IRC 频道。它支持 TLS、NickServ/SASL 认证、群组触发，并可在服务器支持 message tags 时发送输入状态提示。

## 设置

- 选择 IRC 服务器和机器人昵称。
- 添加一个或多个频道名称。
- 仅在网络需要时配置认证字段。

## 配置示例

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

## 字段参考

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `server` | string | IRC 服务器 host:port。 |
| `tls` | bool | 使用 TLS。 |
| `nick` | string | 机器人昵称。 |
| `user` | string | 可选用户名。 |
| `real_name` | string | 可选真实名称。 |
| `password` | string | 可选服务器密码。 |
| `nickserv_password` | string | 可选 NickServ 密码。 |
| `sasl_user` / `sasl_password` | string | 可选 SASL 凭据。 |
| `channels` | string[] | 要加入的 IRC 频道。 |
| `request_caps` | string[] | 要请求的 IRC capability。 |

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
