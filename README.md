# 🐾 PataVida — Assistente Virtual para Clínica Veterinária

Chatbot para clínica veterinária em HTML puro, sem frameworks, sem build step. Basta um arquivo `.html` para rodar — ideal para hospedar no **AWS Lambda** como função com resposta HTML, ou em qualquer servidor estático.

![HTML](https://img.shields.io/badge/HTML-puro-orange?style=flat-square&logo=html5)
![JS](https://img.shields.io/badge/JavaScript-ES5-yellow?style=flat-square&logo=javascript)
![Gemini](https://img.shields.io/badge/Gemini-1.5%20Flash-blue?style=flat-square&logo=google)
![License](https://img.shields.io/badge/licença-MIT-green?style=flat-square)

---

## ✨ Funcionalidades

- 💬 Chat em tempo real com IA (Google Gemini)
- 📋 10 perguntas frequentes como botões clicáveis
- ⌨️ Digitação animada enquanto aguarda resposta
- 📱 Layout responsivo (desktop e mobile)
- 🔑 Campo para inserir a API Key sem precisar editar o código
- ♻️ Histórico de conversa mantido na sessão (últimas 24 mensagens)

---

## 🚀 Como usar

### 1. Obter API Key gratuita

Acesse [aistudio.google.com/apikey](https://aistudio.google.com/apikey), faça login com sua conta Google e crie uma chave.

> O modelo `gemini-1.5-flash` tem **free tier generoso**: 15 requisições/min e 1 milhão de tokens/dia — sem precisar de cartão de crédito.

### 2. Opção A — Testar localmente

Abra o terminal na pasta do projeto e rode:

```bash
# Python (já vem instalado no Mac/Linux)
python3 -m http.server 8080
```

Acesse `http://localhost:8080/vet-chatbot.html` no browser, cole sua API Key no campo e comece a conversar.

> ⚠️ **Não abra o arquivo direto** com `file://` — o browser bloqueia requisições externas (CORS). Use sempre um servidor local ou hospede online.

### 3. Opção B — Deploy no AWS Lambda

Crie uma função Lambda com o handler abaixo. O HTML é retornado diretamente como resposta HTTP:

```python
def lambda_handler(event, context):
    with open('vet-chatbot.html', 'r', encoding='utf-8') as f:
        html = f.read()
    return {
        'statusCode': 200,
        'headers': {'Content-Type': 'text/html; charset=utf-8'},
        'body': html
    }
```

Faça o upload do `vet-chatbot.html` junto com o handler e configure um **Function URL** ou **API Gateway** para expor o endpoint publicamente.

---

## 🔌 APIs gratuitas compatíveis

Você pode trocar o modelo editando a variável `API_URL` no HTML. Opções sem custo:

| Provedor | Modelo | Free Tier | Cadastro |
|---|---|---|---|
| **Google Gemini** ✅ (padrão) | `gemini-1.5-flash` | 15 req/min, 1M tokens/dia | Só conta Google |
| **Groq** | `llama-3.1-8b-instant` | 14.400 req/dia | Sem cartão |
| **OpenRouter** | vários (Llama, Gemma…) | Créditos iniciais grátis | Sem cartão |
| **Cohere** | `command-r` | 1.000 req/mês | Sem cartão |

### Trocar para Groq (exemplo)

1. Crie sua key em [console.groq.com](https://console.groq.com)
2. No HTML, substitua `API_URL` e ajuste o `fetch`:

```js
var API_URL = 'https://api.groq.com/openai/v1/chat/completions';

// No corpo do fetch, mude o formato para OpenAI-compatible:
body: JSON.stringify({
  model: 'llama-3.1-8b-instant',
  messages: [
    { role: 'system', content: SYS },
    ...chatHistory.map(function(m) {
      return { role: m.role === 'model' ? 'assistant' : m.role, content: m.parts[0].text };
    })
  ],
  max_tokens: 600,
  temperature: 0.75
})
```

> Groq e OpenRouter usam o formato **OpenAI-compatible** — só muda a URL e a chave.

---

## 📁 Estrutura do projeto

```
patavida-chatbot/
└── vet-chatbot.html   # tudo em um único arquivo
```

Sem dependências, sem `package.json`, sem build. Zero configuração.

---

## ⚙️ Personalização

Todas as informações da clínica ficam no `SYS` (system prompt) dentro do `<script>`. Edite livremente:

```js
var SYS = 'Você é o assistente virtual da PataVida...\n\n'
  + 'ENDEREÇO: Rua das Flores, 452\n'
  + 'TELEFONE: (11) 4002-8922\n'
  // ... adicione seus dados aqui
```

Para alterar as perguntas rápidas, edite o array `FAQ`:

```js
var FAQ = [
  '📅 Quero agendar uma consulta',
  '🕐 Qual o horário de funcionamento?',
  // adicione ou remova perguntas aqui
];
```

---

## 🛡️ Segurança

> A API Key é inserida pelo usuário no campo da interface e **nunca é armazenada** — ela vive apenas na memória da sessão do browser.

Se quiser fixar a key no código para uso interno (sem expor o campo), substitua:

```js
var key = keyEl.value.trim();
```

por:

```js
var key = 'SUA_KEY_AQUI';
```

E remova ou oculte o bloco `.api-bar` no HTML.

> ⚠️ **Não faça commit de API Keys** em repositórios públicos.

---

## 📄 Licença

MIT — use, modifique e distribua livremente.
