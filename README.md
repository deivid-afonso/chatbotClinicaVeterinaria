# 🐾 PataVida — Assistente Virtual com IA (AWS + Groq)

Chatbot inteligente para clínica veterinária rodando com arquitetura serverless:

* 🌐 Frontend estático (S3)
* ⚙️ Backend serverless (AWS Lambda)
* 🔗 API Gateway
* 🤖 IA via Groq (Llama 3)

> Projeto leve, escalável e sem custo inicial — ideal para MVP ou produção.

---

## ✨ Funcionalidades

* 💬 Chat com IA em tempo real
* ⚡ Respostas rápidas com modelo `llama-3.1-8b-instant`
* 🧠 Contexto de conversa (histórico)
* 📱 Interface responsiva (desktop + mobile)
* ⏳ Indicador de “Digitando…”
* 🔒 API Key protegida no backend (não exposta no front)

---

## 🏗️ Arquitetura

```
[S3 - Frontend]
       ↓
[API Gateway]
       ↓
[AWS Lambda]
       ↓
[Groq API]
```

---

## 🚀 Deploy (AWS)

### 1. Frontend (S3)

* Criar bucket S3
* Ativar **Static Website Hosting**
* Subir o `index.html`
* Acessar via:

```
http://SEU-BUCKET.s3-website-us-east-1.amazonaws.com
```

---

### 2. Backend (Lambda)

* Criar função Node.js 18+ ou 20+
* Subir arquivo:

```
backend/index.mjs
```

---

### 3. Variável de ambiente

Na Lambda:

```
GROQ_API_KEY= SUA_CHAVE_AQUI
```

---

### 4. API Gateway

Criar:

```
POST /chat
```

E integrar com a Lambda

---

### 5. Habilitar CORS

No API Gateway:

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST,OPTIONS
Access-Control-Allow-Headers: *
```

Depois:

```
Deploy → prod
```

---

### 6. URL final da API

```
https://SEU-ID.execute-api.us-east-1.amazonaws.com/prod/chat
```

---

### 7. Ajustar frontend

No `index.html`:

```js
var API_URL = 'https://SEU-ID.execute-api.us-east-1.amazonaws.com/prod/chat';
```

---

## 🧪 Teste

### Backend (PowerShell)

```powershell
Invoke-RestMethod -Method POST `
-Uri "https://SEU-ID.execute-api.us-east-1.amazonaws.com/prod/chat" `
-Headers @{ "Content-Type" = "application/json" } `
-Body '{"messages":[{"role":"user","content":"oi"}]}'
```

---

### Frontend

Abra no navegador:

```
http://SEU-BUCKET.s3-website-us-east-1.amazonaws.com
```

Digite:

```
oi
```

---

## 🔌 IA utilizada

| Provedor | Modelo                 | Observação              |
| -------- | ---------------------- | ----------------------- |
| **Groq** | `llama-3.1-8b-instant` | Ultra rápido e gratuito |

Criar chave em:

👉 https://console.groq.com

---

## 📁 Estrutura do projeto

```
pata-vida-chat/
│
├── frontend/
│   └── index.html
│
├── backend/
│   └── index.mjs
│
└── README.md
```

---

## ⚙️ Personalização

### Prompt da IA

```js
var SYS = `Você é o assistente da clínica veterinária...`;
```

---

### Estilo

Edite diretamente no CSS do `index.html`

---

## 🛡️ Segurança

* API Key fica **somente na Lambda**
* Nunca exposta no frontend
* Comunicação via HTTPS

---

## 📄 Licença

MIT — uso livre para estudo e projetos comerciais.

---

## 🚀 Próximos upgrades

* ⚡ Streaming de resposta (tipo ChatGPT)
* 🧠 Memória persistente (DynamoDB)
* 🔐 Autenticação de usuários
* 🌐 CloudFront + HTTPS + domínio próprio
* 📱 Transformar em PWA (app)

---

## 👨‍💻 Autor

Desenvolvido por **Deivid da Silva Afonso**
🔗 https://www.linkedin.com/in/deivid-da-silva-afonso-dev/

---
