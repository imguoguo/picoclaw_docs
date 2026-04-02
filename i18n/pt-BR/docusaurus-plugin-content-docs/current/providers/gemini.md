---
id: gemini-api
title: Gemini API (Google AI Studio)
---

# Guia de Configuração da API Gemini

## Visão Geral

A **API Gemini** é a interface de IA do Google fornecida pelo **Google AI Studio**. Comparado ao Vertex AI do Google Cloud, o AI Studio oferece um método de autenticação via **API Key** mais simples, ideal para desenvolvimento rápido e integrações pessoais.

O Gemini oferece múltiplas séries de modelos para diferentes cenários de desempenho e custo:

| Modelo | Características | Casos de Uso |
|--------|-----------------|--------------|
| gemini-2.0-flash | Rápido, baixo custo | Alta concorrência, conversas do dia a dia |
| gemini-1.5-pro | Multimodal de alta qualidade | Tarefas complexas, compreensão de contextos longos |
| gemini-1.5-flash | Equilíbrio entre desempenho e custo | Casos de uso gerais |

---

## Obtendo a API Key

### Passo 1: Acesse o Google AI Studio

Vá para o [Google AI Studio](https://aistudio.google.com/) e faça login com sua conta Google.

### Passo 2: Gere a API Key

1. Clique em **"Get API key"** na barra de navegação lateral esquerda
2. Clique em **"Create API key in new project"** (ou selecione um projeto Google Cloud existente)
3. **Copie e salve** sua API Key

> ⚠️ **Atenção**: Mantenha sua API Key em segurança e não a exponha em repositórios de código públicos.

![Gemini API Key](/img/providers/geminiapi.png)

![Gemini API Key](/img/providers/geminiapi1.png)

---

## Configurando o PicoClaw

### Opção 1: Usando a WebUI (Recomendado)

O PicoClaw oferece uma interface WebUI onde você pode configurar modelos facilmente sem precisar editar arquivos de configuração manualmente.

Edite as configurações predefinidas ou clique no botão **"Adicionar Modelo"** no canto superior direito:

![Adicionar Modelo](/img/providers/webuimodel.png)

| Campo | Valor |
|-------|-------|
| Apelido do Modelo | Nome personalizado, ex.: `gemini-flash` |
| Identificador do Modelo | `google/gemini-2.0-flash` (ou outros modelos suportados) |
| API Key | API Key do Google AI Studio |
| URL Base da API | Deixe vazio (usa o padrão) |

### Opção 2: Editar o Arquivo de Configuração

Adicione modelos Gemini no `config.json` e defina `auth_method` como `api_key`:

```json
{
  "model_list": [
    {
      "model_name": "gemini-flash",
      "model": "google/gemini-2.0-flash",
      "auth_method": "api_key",
      "api_key": "YOUR_GEMINI_API_KEY_HERE"
    },
    {
      "model_name": "gemini-pro",
      "model": "google/gemini-1.5-pro",
      "auth_method": "api_key",
      "api_key": "YOUR_GEMINI_API_KEY_HERE"
    }
  ],
  "agents": {
    "defaults": {
      "model": "gemini-flash"
    }
  }
}
```

---

## Limites e Cotas

### Nível Gratuito

O Google AI Studio oferece um nível gratuito para desenvolvedores:

- **Cota Gratuita**: Allowance diária de requisições gratuitas
- **Limites de Taxa**: Limites de requisições por minuto (RPM) no nível gratuito
- **Privacidade de Dados**: No nível gratuito, o Google pode usar dados de entrada/saída para melhorar os modelos

### Nível Pago

Para cotas maiores ou proteção de privacidade em nível empresarial, faça upgrade para o nível pago ou use o Google Cloud Vertex AI.

---

## Problemas Comuns

### API Key Inválida

**Causa**: API Key expirada ou revogada

**Solução**: Gere uma nova API Key no Google AI Studio

### Timeout de Requisição

**Causa**: Problemas de rede ou muitas requisições

**Soluções**:
- Verifique a conexão de rede
- Reduza a frequência de requisições
- Use um proxy se necessário

### Modelo Indisponível

**Causa**: Alguns modelos não estão disponíveis em certas regiões

**Soluções**:
- Verifique se o modelo é suportado na sua região
- Tente usar outros modelos Gemini
