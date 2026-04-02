---
id: qq
title: QQ
---

# QQ

## Configuração

### 1. Criar um Bot

- Acesse a [Plataforma Aberta QQ](https://q.qq.com/#)
- Crie um aplicativo → Obtenha o **AppID** e o **AppSecret**

### 2. Configurar

```json
{
  "channels": {
    "qq": {
      "enabled": true,
      "app_id": "YOUR_APP_ID",
      "app_secret": "YOUR_APP_SECRET",
      "allow_from": []
    }
  }
}
```

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `app_id` | string | AppID do bot QQ |
| `app_secret` | string | AppSecret do bot QQ |
| `allow_from` | array | Lista de IDs de usuários QQ permitidos (vazio = permitir todos) |

### 3. Executar

```bash
picoclaw gateway
```
