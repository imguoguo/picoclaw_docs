---
id: index
title: Canais de Chat
sidebar_label: Visão Geral
---

# Canais de Chat

Conecte o PicoClaw a plataformas de mensagens através do modo **gateway**.

```bash
picoclaw gateway
```

## Canais Suportados

| Canal | Dificuldade | Observações |
| --- | --- | --- |
| **Telegram** | Fácil | Recomendado. Suporta transcrição de voz com Groq. |
| **Discord** | Fácil | Token de Bot + intents. Suporta acionamento em grupo. |
| **Slack** | Fácil | Modo Socket, não requer IP público. |
| **QQ** | Fácil | API oficial do QQ bot (AppID + AppSecret). |
| **DingTalk** | Médio | Modo Stream, não requer IP público. |
| **WeCom Bot** | Médio | Chat em grupo via webhook. |
| **WeCom App** | Difícil | Chat privado, envio proativo de mensagens. |
| **WeCom AI Bot** | Médio | AI Bot oficial com protocolo de streaming pull. |
| **Feishu** | Difícil | Plataforma de colaboração empresarial. |
| **LINE** | Difícil | Webhook via porta compartilhada do gateway. |
| **OneBot** | Médio | Compatível com NapCat/Go-CQHTTP. |
| **WhatsApp** | Médio | Modo bridge ou nativo (whatsmeow). |
| **MaixCam** | Fácil | Câmera de IA integrada ao hardware. |
| **Pico** | Fácil | Canal WebSocket nativo para clientes personalizados. |

## Como Funciona

1. Configure um ou mais canais em `~/.picoclaw/config.json` na chave `channels`
2. Defina `"enabled": true` para cada canal que deseja usar
3. Execute `picoclaw gateway` para começar a escutar
4. O gateway gerencia todos os canais simultaneamente

## Controle de Acesso

Todos os canais suportam o campo `allow_from` para restringir o acesso a usuários específicos:

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

Defina `allow_from` como um array vazio `[]` para permitir todos os usuários.

## Campos Comuns dos Canais

Todos os canais suportam estes campos opcionais:

| Campo | Descrição |
| --- | --- |
| `reasoning_channel_id` | Direcionar a saída de raciocínio/pensamento para um canal separado |
| `group_trigger` | Controlar o comportamento do bot em chats de grupo (somente menção, prefixos) |

## Gateway Compartilhado

Todos os canais baseados em webhook (LINE, WeCom, WeCom App, WeCom AI Bot) compartilham o único servidor HTTP do gateway na porta `18790`. Os campos `webhook_host`/`webhook_port` por canal não são mais necessários — basta configurar `webhook_path` para diferenciar os endpoints.
