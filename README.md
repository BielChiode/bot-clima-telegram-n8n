# 🤖 Chatbot de Clima no Telegram com N8N

Este projeto implementa um **chatbot no Telegram** usando **n8n** que informa a **temperatura atual de cidades do Brasil** a partir da API do **OpenWeather**.  
O bot recebe uma mensagem no formato `Cidade,UF`, valida a entrada, consulta a API de clima e responde com uma **mensagem curta, clara e amigável**.

Opcionalmente, o workflow utiliza o **Google Gemini** para melhorar a redação da resposta. Caso o Gemini não esteja configurado ou falhe, o fluxo possui um **fallback determinístico** para garantir que o bot continue funcionando.

---

## ✨ Funcionalidades

- ✅ Recebe mensagens via Telegram
- ✅ Valida formato de entrada: `Cidade,UF` (ex: `São Paulo,SP`)
- ✅ Valida UF contra a lista oficial do Brasil
- ✅ Consulta a API do OpenWeather
- ✅ Formata a resposta com temperatura atual
- ✅ (Opcional) Usa Google Gemini para gerar uma resposta mais natural
- ✅ Possui fallback caso o Gemini não esteja disponível
- ✅ Retorna mensagens de erro amigáveis quando a cidade é inválida

---

## 🧩 Estrutura do Workflow (Visão Geral)

1. **Telegram Trigger** – Recebe a mensagem do usuário
2. **Code (JavaScript)** – Valida e normaliza a entrada (`Cidade,UF`)
3. **IF** – Se inválido, retorna erro imediatamente
4. **Edit Fields / Geocoding / Get Weather** – Prepara dados e consulta OpenWeather
5. **IF** – Verifica se a API retornou dados válidos
6. **Gemini (opcional)** – Gera uma mensagem mais amigável
7. **IF (fallback)** – Se Gemini falhar, usa mensagem padrão
8. **Telegram Send Message** – Envia a resposta final ao usuário

---

## 📦 Arquivos do Repositório

- `workflow-chatbot-telegram.json` → Workflow exportado do n8n
- `README.md` → Este arquivo com a documentação

---

## 🚀 Como importar o workflow no n8n

1. Abra o **n8n** no seu ambiente (local, Docker ou cloud).
2. Vá em **Workflows** → **Import**.
3. Selecione o arquivo: workflow-chatbot-telegram.json
4. Após importar, abra o workflow e verifique se todos os nodes aparecem corretamente.
5. Configure as **credenciais** conforme descrito abaixo.
6. Salve o workflow e ative-o.

---

## 🔐 Configuração de Credenciais e Variáveis de Ambiente

⚠️ **Nunca coloque suas chaves diretamente no workflow ou no repositório.**  
Use **variáveis de ambiente** ou o sistema de **Credentials** do n8n.

Este projeto espera as seguintes variáveis:

### 1️⃣ OpenWeather

- Variável de ambiente: OPENWEATHER_API_KEY
- Use sua API Key do OpenWeather.
- No node **HTTP Request (Get Weather)**, o campo `appid` deve referenciar essa variável de ambiente.

---

### 2️⃣ Telegram Bot

- Variável de ambiente: TELEGRAM_BOT_TOKEN
- Crie seu bot via **@BotFather** no Telegram e obtenha o token.
- No n8n, crie uma credencial de **Telegram** usando esse token.
- Associe essa credencial aos nodes:
- **Telegram Trigger**
- **Telegram Send Message**

---

### 3️⃣ Google Gemini (Opcional)

- Variável de ambiente: GEMINI_API_KEY
- Use sua API Key do Google Gemini / PaLM.
- No n8n, crie a credencial de **Google Gemini / PaLM API** usando essa chave.
- Associe essa credencial ao node **Message a model**.

> ℹ️ Se o Gemini não estiver configurado, o workflow continuará funcionando usando o **fallback** (mensagem padrão determinística).

---

## 🧪 Como testar o bot

No Telegram, envie mensagens para o seu bot no formato:

- ✅ Exemplo válido:
  São Paulo,SP
  ou
  Rio de Janeiro,RJ

- ❌ Exemplos inválidos:
  São Paulo
  Cidade,XX

### Respostas esperadas:

- Para cidades válidas:
  🌤️ A temperatura em São Paulo é de 25°C.
  (ou uma versão mais natural gerada pelo Gemini, se configurado)

- Para entradas inválidas:
  ❌ Cidade não encontrada. Use o formato: Cidade,UF (ex: São Paulo,SP)

---

## 🛡️ Sobre o uso do Gemini e Fallback

- O node **Gemini** é usado para melhorar a qualidade da mensagem final.
- O workflow possui um **IF de verificação** após o Gemini:
- Se o Gemini retornar uma mensagem válida → usa a resposta do Gemini
- Se o Gemini falhar ou não estiver configurado → usa a **mensagem padrão (fallback)**
- Isso garante que o bot **nunca pare de funcionar** por causa de IA.

---

## ⚠️ Checklist antes de publicar

- [ ] Workflow importado e funcionando no n8n
- [ ] Testado com pelo menos 3 cidades válidas
- [ ] Testado com entradas inválidas
- [ ] Nenhuma chave ou token dentro do JSON do workflow
- [ ] Variáveis de ambiente configuradas:
- `OPENWEATHER_API_KEY`
- `TELEGRAM_BOT_TOKEN`
- `GEMINI_API_KEY` (opcional)

---

## 📌 Observação Final

Este projeto foi desenvolvido como parte de um desafio prático para consolidar conhecimentos em **integrações, automações e orquestração de fluxos com n8n**, utilizando APIs externas e, opcionalmente, **IA generativa** para melhorar a experiência do usuário.

---

Feito com 💜 e automação ⚙️
