---
id: index
title: 聊天通道
sidebar_label: 概览
---

# 聊天通道

通过 **网关模式** 将 PicoClaw 连接到各种即时通讯平台。

```bash
picoclaw gateway
```

## 支持的通道

| 通道 | 难度 | 说明 |
| --- | --- | --- |
| **Telegram** | 简单 | 推荐。支持 Groq 语音转文字。 |
| **Discord** | 简单 | 机器人 token + 意图配置。支持仅@模式。 |
| **Slack** | 简单 | Socket 模式，无需公网 IP。 |
| **QQ** | 简单 | 官方 QQ 机器人 API（AppID + AppSecret）。 |
| **钉钉** | 中等 | Stream 模式，无需公网 IP。 |
| **企业微信机器人** | 中等 | 群聊 Webhook。 |
| **企业微信自建应用** | 困难 | 私聊，功能更多，需要 HTTPS。 |
| **飞书** | 困难 | 企业协作平台。 |
| **LINE** | 困难 | 需要 HTTPS Webhook。 |
| **OneBot** | 中等 | 兼容 NapCat/Go-CQHTTP。 |
| **MaixCam** | 简单 | 硬件集成 AI 摄像头。 |

## 工作原理

1. 在 `~/.picoclaw/config.json` 的 `channels` 字段中配置一个或多个通道
2. 为要启用的每个通道设置 `"enabled": true`
3. 运行 `picoclaw gateway` 开始监听
4. 网关同时处理所有通道

## 访问控制

所有通道都支持 `allow_from` 字段，用于限制特定用户的访问：

```json
{
  "channels": {
    "telegram": {
      "enabled": true,
      "token": "YOUR_TOKEN",
      "allow_from": ["123456789"]
    }
  }
}
```

将 `allow_from` 设置为空数组 `[]` 可允许所有用户访问。
