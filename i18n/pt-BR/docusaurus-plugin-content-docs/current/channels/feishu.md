---
id: feishu
title: Feishu / Lark (飞书)
---

# Feishu / Lark

## Configuração

### 1. Criar um App Feishu

- Acesse a [Plataforma Aberta Feishu](https://open.feishu.cn/)
- Crie um app personalizado
- Anote o **App ID** e o **App Secret**

### 2. Configurar Permissões

Nas configurações do seu app, adicione as seguintes permissões do bot:
- `im:message:receive_v1`
- `im:message`

### 3. Ativar Eventos de Mensagem

- Adicione uma **Encrypt Key** e um **Verification Token** nas configurações de Event Subscription
- Inscreva-se no evento `im.message.receive_v1`

### 4. Configurar o PicoClaw

```json
{
  "channels": {
    "feishu": {
      "enabled": true,
      "app_id": "cli_xxx",
      "app_secret": "YOUR_APP_SECRET",
      "encrypt_key": "YOUR_ENCRYPT_KEY",
      "verification_token": "YOUR_VERIFICATION_TOKEN",
      "allow_from": []
    }
  }
}
```

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `app_id` | string | ID do app Feishu (começa com `cli_`) |
| `app_secret` | string | Secret do app Feishu |
| `encrypt_key` | string | Chave de criptografia de eventos |
| `verification_token` | string | Token de verificação de eventos |
| `allow_from` | array | IDs de usuários open permitidos |

### 5. Executar

```bash
picoclaw gateway
```

:::note
A documentação completa do Feishu está disponível em chinês no repositório em `docs/channels/feishu/README.zh.md`.
:::
