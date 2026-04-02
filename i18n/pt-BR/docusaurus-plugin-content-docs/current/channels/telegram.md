---
id: telegram
title: Telegram
---

# Telegram

O Telegram é o canal **recomendado**. É fácil de configurar e suporta transcrição de voz.

## Configuração

### 1. Criar um Bot

- Abra o Telegram e pesquise por `@BotFather`
- Envie `/newbot` e siga as instruções
- Copie o token do bot

### 2. Obter Seu ID de Usuário

- Envie uma mensagem para `@userinfobot` no Telegram
- Copie seu ID de Usuário

### 3. Configurar

```json
{
  "channels": {
    "telegram": {
      "enabled": true,
      "token": "YOUR_BOT_TOKEN",
      "allow_from": ["YOUR_USER_ID"]
    }
  }
}
```

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `enabled` | bool | Ativar/desativar o canal |
| `token` | string | Token do Bot obtido do @BotFather |
| `base_url` | string | URL personalizada do servidor Telegram Bot API (opcional) |
| `proxy` | string | URL de proxy HTTP/SOCKS (opcional, também lê a variável de ambiente `HTTP_PROXY`) |
| `allow_from` | array | Lista de IDs de usuários permitidos (vazio = permitir todos) |
| `reasoning_channel_id` | string | Direcionar a saída de raciocínio para um chat separado |
| `group_trigger` | object | Configurações de acionamento em grupo (`mention_only`, `prefixes`) |

### 4. Executar

```bash
picoclaw gateway
```

## Transcrição de Voz

Mensagens de voz do Telegram podem ser transcritas automaticamente usando o Whisper da Groq:

```json
{
  "model_list": [
    {
      "model_name": "whisper",
      "model": "groq/whisper-large-v3",
      "api_key": "gsk_..."
    }
  ]
}
```

Obtenha uma chave de API Groq gratuita em [console.groq.com](https://console.groq.com).

## Solução de Problemas

**"Conflict: terminated by other getUpdates"**: Apenas uma instância de `picoclaw gateway` pode ser executada por vez. Pare quaisquer outras instâncias.

**Proxy**: Se o Telegram estiver bloqueado na sua região, use o campo `proxy`:

```json
{
  "channels": {
    "telegram": {
      "proxy": "socks5://127.0.0.1:1080"
    }
  }
}
```

## Comandos do Bot

O canal Telegram registra estes comandos integrados do bot:

| Comando | Descrição |
| --- | --- |
| `/start` | Mensagem de saudação |
| `/help` | Exibir texto de ajuda |
| `/show [model\|channel]` | Mostrar configuração atual |
| `/list [models\|channels]` | Listar opções disponíveis |

## Suporte a Mídia

O bot lida com fotos, arquivos de áudio, documentos e mensagens de voz. Mensagens de voz são transcritas se um modelo Whisper estiver configurado (veja [Transcrição de Voz](#transcrição-de-voz)).
