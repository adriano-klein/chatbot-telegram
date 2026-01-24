# **🌦️ Agente de clima inteligente no Telegram com n8n**

Este repositório contém um workflow de automação criado no **n8n** que atua como um assistente meteorológico no Telegram.

O projeto implementa **Inteligência Artificial (Google Gemini/OpenRouter)** para gerar respostas humanizadas e um **Sistema de Fallback** para garantir disponibilidade mesmo sem IA.

## **📋 Funcionalidades**

* **Processamento de Linguagem Natural (NLP):** Utiliza Google Gemini para interpretar dados brutos do clima e gerar mensagens empáticas, criativas e com dicas úteis.  
* **Validação de Entrada:** Verifica se o usuário enviou o formato correto (Cidade, UF) antes de consumir APIs.  
* **Integração OpenWeather:** Consulta dados precisos de temperatura e condições climáticas em tempo real.  
* **Sistema de Fallback Determinístico:** Se a IA falhar (falta de créditos, erro de API ou timeout), um nó de código assume e gera uma resposta padrão formatada, garantindo que o usuário nunca fique sem a informação.  
* **Tratamento de Erros:** Mensagens amigáveis caso a cidade não seja encontrada ou o formato esteja incorreto.

## **🚀 Como importar o workflow**

1. **Baixe o workflow:**  
   * Faça o download do arquivo JSON deste repositório ou copie seu conteúdo bruto.  
2. **Importe no n8n:**  
   * Abra seu editor do n8n.  
   * Clique no menu no canto superior direito (três pontos) ou clique na tela vazia.  
   * Selecione **"Import from File"** (se baixou) ou **"Import from Clipboard"** (se copiou o JSON).  
   * O fluxo completo com todos os nós aparecerá na sua tela.

## **🔑 Configuração das credenciais**

Para que o bot funcione, você precisa configurar as credenciais no n8n. O workflow foi configurado para reconhecer os seguintes nomes de credenciais:

### **1\. Telegram (Bot)**

1. Fale com o **@BotFather** no Telegram para criar um novo bot (/newbot) e obter seu **Token**.  
2. No n8n, vá em **Credentials** \> **Add Credential**.  
3. Procure por **Telegram API**.  
4. Insira o Token fornecido pelo BotFather.  
5. **Importante:** Nomeie esta credencial exatamente como abaixo (ou selecione a credencial que você acabou de criar nos nós de Telegram do fluxo):  
   TELEGRAM\_BOT\_TOKEN

### **2\. OpenWeatherMap**

1. Crie uma conta em [OpenWeatherMap](https://openweathermap.org/) e gere uma API Key gratuita.  
2. No n8n, vá em **Credentials** \> **Add Credential**.  
3. Procure por **Header Auth** ou **Query Auth** (conforme configurado no nó "Consulta OpenWeather"). *No fluxo atual, está configurado como httpQueryAuth*.  
4. Preencha os campos:  
   * **Name:** appid  
   * **Value:** \<SUA\_CHAVE\_API\_AQUI\>  
5. **Importante:** Nomeie a credencial no n8n como:  
   OPENWEATHER\_API\_KEY

### **3\. Google Gemini**

1. Obtenha sua chave no [Google AI Studio](https://aistudio.google.com/).  
2. Crie uma credencial **Google Palm API** no n8n.

## **🤖 Como executar e testar**

Após importar o fluxo e configurar as credenciais, ative o workflow (chave **Active** no topo direito).

### **Teste de sucesso (Com IA):**

1. Abra seu bot no Telegram.  
2. Envie uma mensagem no formato: Cidade, UF.  
   * Exemplo: Curitiba, PR ou São Paulo, SP

**Retorno esperado:**

📍 Clima em Curitiba, PR. Atualmente faz 18ºC, mas a sensação é de 16ºC. O céu está nublado. ☁️ Não esqueça o casaco\! O povo curitibano adora esse friozinho, né?

### **Teste de erro (Validação):**

1. Envie apenas: Curitiba

**Retorno Esperado:**

❌ Formato inválido. Use o formato Cidade,UF (ex.: São Paulo,SP).

### **Teste de Fallback (Simulação de Falha na IA):**

Se você desativar a credencial do Gemini ou se a API falhar:

1. Envie: Rio de Janeiro, RJ  
2. O fluxo detectará a falha no Agente e acionará o nó de Código.

**Retorno Esperado (Padrão):**

🌤️ A temperatura em Rio de Janeiro é de 32.5ºC

## **🛠️ Estrutura do Projeto**

O fluxo segue a seguinte lógica de execução:

1. **Telegram Trigger:** Recebe a mensagem.  
2. **Code & If:** Normaliza o texto e valida se é Cidade, UF.  
3. **HTTP Request:** Busca dados na OpenWeather.  
4. **AI Agent:** Tenta gerar uma resposta criativa.  
5. **If (Validação de Resposta):** Verifica se o Agente gerou conteúdo.  
   * **Sim:** Envia a resposta da IA.  
   * **Não (Erro):** Aciona o nó "Ajusta resposta" e envia a previsão padrão (Fallback).

### **👨‍💻 Autor**

Projeto desenvolvido por Adriano Klein como parte do Desafio Fase 2 de Automação com n8n da Rocketseat.
