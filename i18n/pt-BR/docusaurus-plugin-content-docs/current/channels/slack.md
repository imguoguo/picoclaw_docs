---
id: slack
title: Slack
---

# Slack

A integração com o Slack usa o Socket Mode, então não é necessário um IP público.

## Configuração

### 1. Criar um Slack App

- Acesse [api.slack.com/apps](https://api.slack.com/apps) → Criar Novo App → Do zero
- Escolha seu workspace

### 2. Ativar o Socket Mode

- App Home → Socket Mode → Ativar Socket Mode
- Crie um **App-Level Token** com o escopo `connections:write`
- Copie o token do app (começa com `xapp-`)

### 3. Adicionar Escopos do Bot Token

- OAuth & Permissions → Bot Token Scopes → Adicione:
  - `chat:write`
  - `im:history`
  - `im:read`
  - `app_mentions:read`

### 4. Instalar no Workspace

- OAuth & Permissions → Instalar no Workspace
- Copie o **Bot User OAuth Token** (começa com `xoxb-`)

### 5. Ativar Event Subscriptions

- Event Subscriptions → Ativar Events
- Inscreva-se nos eventos do bot:
  - `message.im`
  - `app_mention`

### 6. Configurar

```json
{
  "channels": {
    "slack": {
      "enabled": true,
      "bot_token": "xoxb-YOUR-BOT-TOKEN",
      "app_token": "xapp-YOUR-APP-TOKEN",
      "allow_from": []
    }
  }
}
```

### 7. Executar

```bash
picoclaw gateway
```
