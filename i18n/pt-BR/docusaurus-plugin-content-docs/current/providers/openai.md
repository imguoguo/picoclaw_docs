---
id: openai-api
title: OpenAI API
---

# Guia de Configuração da API OpenAI

## Visão Geral

A **API OpenAI** é uma interface de IA de uso geral fornecida pela OpenAI, com suporte a geração de texto, conversas, geração de código e muito mais. Ela oferece uma especificação de interface altamente unificada e é amplamente suportada.

A OpenAI disponibiliza múltiplas séries de modelos para diferentes cenários de desempenho e custo:

| Modelo | Características | Casos de Uso |
|--------|-----------------|--------------|
| gpt-4o-mini | Rápido, baixo custo | Alta concorrência, conversas do dia a dia |
| gpt-4o | Multimodal de alta qualidade | Tarefas complexas, compreensão de imagens |
| gpt-4.1 | Maior capacidade de raciocínio e código | Geração de código, raciocínio lógico |

---

## Obtendo a API Key

### Passo 1: Acesse a Plataforma OpenAI

Vá para a [Plataforma OpenAI](https://platform.openai.com/) e faça login na sua conta.

### Passo 2: Gere a API Key

1. Navegue até **Dashboard → API Keys**
2. Clique em **"Create new secret key"**
3. **Copie e salve** sua API Key

> ⚠️ **Atenção**: A API Key é exibida apenas uma vez. Mantenha-a em segurança e não a compartilhe com outras pessoas.

![Página de API Keys](/img/providers/openaiapi.png)

![Criar Nova API Key](/img/providers/openaiapi1.png)

---

## Configurando o PicoClaw

### Opção 1: Usando a WebUI (Recomendado)

O PicoClaw oferece uma interface WebUI onde você pode configurar modelos facilmente sem precisar editar arquivos de configuração manualmente.

Edite as configurações predefinidas ou clique no botão **"Adicionar Modelo"** no canto superior direito:

![Adicionar Modelo](/img/providers/webuimodel.png)

| Campo | Valor |
|-------|-------|
| Apelido do Modelo | Nome personalizado, ex.: `gpt-4o` |
| Identificador do Modelo | `openai/gpt-4o-mini` (ou outros modelos suportados) |
| API Key | API Key da OpenAI (`sk-xxxxx`) |
| URL Base da API | Deixe vazio (padrão: `https://api.openai.com/v1`) |

### Opção 2: Editar o Arquivo de Configuração

Adicione modelos OpenAI no `config.json` e defina `auth_method` como `api_key`:

```json
{
  "model_list": [
    {
      "model_name": "gpt-4o-mini",
      "model": "openai/gpt-4o-mini",
      "auth_method": "api_key",
      "api_key": "YOUR_OPENAI_API_KEY_HERE"
    },
    {
      "model_name": "gpt-4o",
      "model": "openai/gpt-4o",
      "auth_method": "api_key",
      "api_key": "YOUR_OPENAI_API_KEY_HERE"
    }
  ],
  "agents": {
    "defaults": {
      "model": "gpt-4o-mini"
    }
  }
}
```

---

## Limites e Cotas

### Cobrança

A OpenAI utiliza um modelo de **pagamento por uso**, cobrando com base no consumo real de tokens.

### Limites de Taxa

Diferentes níveis de conta e modelos possuem limites de taxa distintos:

- **RPM** (Requisições Por Minuto): Número de requisições por minuto
- **TPM** (Tokens Por Minuto): Número de tokens por minuto

Ao exceder os limites, você receberá um erro `429 Too Many Requests`.

---

## Problemas Comuns

### Erro max_tokens

```
Invalid max_tokens value
```

**Causa**: Excede o limite do modelo

**Solução**: Reduza o valor do parâmetro `max_tokens` (ex.: 1024 ou 2048)

### Erro 429 de Limite de Taxa

**Soluções**:

- Reduza a frequência de requisições
- Faça upgrade do seu nível de conta na OpenAI
- Ative o limitador de taxa de requisições no PicoClaw

### Não Consegue Conectar à API

**Verifique os seguintes pontos**:

- O `base_url` está correto? (Padrão: `https://api.openai.com/v1`)
- Você precisa de um proxy? (Para usuários no Brasil ou regiões com restrições de acesso)
- A resolução DNS está funcionando corretamente?
- Conectividade de rede
