---
id: antigravity-usage
title: Guia de Uso do Antigravity
---

# Usando o Provider Antigravity no PicoClaw

Este guia explica como configurar e usar o provider **Antigravity** (Google Cloud Code Assist) no PicoClaw.

## Pré-requisitos

1.  Uma conta Google.
2.  Google Cloud Code Assist habilitado (geralmente disponível pelo onboarding do "Gemini para Google Cloud").

## 1. Autenticação

Para autenticar com o Antigravity, execute o seguinte comando:

```bash
picoclaw auth login --provider antigravity
```

### Autenticação Manual (Headless/VPS)
Se você está rodando em um servidor (Coolify/Docker) e não consegue acessar `localhost`, siga estes passos:
1.  Execute o comando acima.
2.  Copie a URL fornecida e abra no seu navegador local.
3.  Complete o login.
4.  Seu navegador irá redirecionar para uma URL `localhost:51121` (que não irá carregar).
5.  **Copie essa URL final** da barra de endereços do seu navegador.
6.  **Cole de volta no terminal** onde o PicoClaw está aguardando.

O PicoClaw irá extrair o código de autorização e completar o processo automaticamente.

## 2. Gerenciando Modelos

### Listar Modelos Disponíveis
Para ver quais modelos seu projeto tem acesso e verificar suas cotas:

```bash
picoclaw auth models
```

### Trocar Modelos
Você pode alterar o modelo padrão em `~/.picoclaw/config.json` ou sobrescrever via CLI:

```bash
# Sobrescrever para um único comando
picoclaw agent -m "Hello" --model claude-opus-4-6-thinking
```

## 3. Uso em Produção (Coolify/Docker)

Se você está fazendo deploy via Coolify ou Docker, siga estes passos para testar:

1.  **Variáveis de Ambiente**:
    *   `PICOCLAW_AGENTS_DEFAULTS_MODEL=gemini-flash`
2.  **Persistência de autenticação**:
    Se você fez login localmente, pode copiar suas credenciais para o servidor:
    ```bash
    scp ~/.picoclaw/auth.json user@your-server:~/.picoclaw/
    ```
    *Alternativamente*, execute o comando `auth login` uma vez no servidor se você tiver acesso ao terminal.

## 4. Solução de Problemas

*   **Resposta Vazia**: Se um modelo retorna uma resposta vazia, ele pode estar restrito para o seu projeto. Tente `gemini-3-flash` ou `claude-opus-4-6-thinking`.
*   **429 Limite de Taxa**: O Antigravity tem cotas rigorosas. O PicoClaw exibirá o "tempo de reset" na mensagem de erro se você atingir um limite.
*   **404 Não Encontrado**: Certifique-se de que está usando um ID de modelo da lista `picoclaw auth models`. Use o ID curto (ex.: `gemini-3-flash`) e não o caminho completo.

## 5. Resumo dos Modelos Funcionais

Com base nos testes, os seguintes modelos são os mais confiáveis:
*   `gemini-3-flash` (Rápido, alta disponibilidade)
*   `gemini-2.5-flash-lite` (Leve)
*   `claude-opus-4-6-thinking` (Poderoso, inclui raciocínio)
