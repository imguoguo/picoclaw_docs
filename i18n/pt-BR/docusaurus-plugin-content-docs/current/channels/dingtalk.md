---
id: dingtalk
title: DingTalk (钉钉)
---

# DingTalk

A integração com o DingTalk usa o Stream Mode, então não é necessário um IP público.

## Configuração

### 1. Criar um Bot

- Acesse a [Plataforma Aberta](https://open.dingtalk.com/)
- Crie um app interno
- Copie o **Client ID** e o **Client Secret**

### 2. Configurar

```json
{
  "channels": {
    "dingtalk": {
      "enabled": true,
      "client_id": "YOUR_CLIENT_ID",
      "client_secret": "YOUR_CLIENT_SECRET",
      "allow_from": []
    }
  }
}
```

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `client_id` | string | Client ID do app DingTalk |
| `client_secret` | string | Client Secret do app DingTalk |
| `allow_from` | array | Lista de IDs de usuários DingTalk permitidos (vazio = permitir todos) |

### 3. Executar

```bash
picoclaw gateway
```
