---
id: whatsapp_native
title: WhatsApp Native
---

# WhatsApp Native

`whatsapp_native` 是原生 WhatsApp 通道类型。它与 `whatsapp` 使用相同的 settings 结构，但用于显式声明原生实现通道。

## 设置

- 将通道类型设置为 `whatsapp_native`。
- 选择持久化的 `session_store_path`。
- 启动 gateway，并完成所需的 WhatsApp 配对流程。

## 配置示例

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

## 字段参考

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `use_native` | bool | 原生模式下应为 true。 |
| `session_store_path` | string | 持久化会话数据库路径。 |
| `bridge_url` | string | 仅使用原生模式部署时不使用。 |

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
