# IARA - Assistente Virtual do CPD (n8n Workflow)

**IARA** é uma automação desenvolvida no [n8n](https://n8n.io) para atuar como Assistente Virtual do Centro de Processamento de Dados (CPD) da **Escola Técnica Estadual Monteiro Lobato**.

O agente atende alunos, responsáveis e professores via integração de mensagens (Uazapi / WhatsApp), oferecendo respostas rápidas sobre horários de aulas, dúvidas frequentes (FAQ), suporte a mensagens de áudio com transcrição e muito mais.

---

## 🚀 Funcionalidades

- **Atendimento Automatizado com IA:** Utiliza o modelo `gpt-4o-mini` da OpenAI via LangChain Agent para processar intenções do usuário.
- **Processamento de Áudios:** Transcreve automaticamente mensagens de voz enviadas pelo usuário utilizando a API da OpenAI.
- **Consulta de Horários de Aula:** Mapeia cursos (Eletrônica, Eletrotécnica, Mecânica, Informática, Química, etc.) e busca o link do PDF de horários no banco de dados.
- **Base de Conhecimento (FAQ):** Responde dúvidas frequentes buscando diretamente na tabela de FAQ.
- **Memória de Conversa:** Mantém o contexto de até 15 mensagens utilizando armazenamento Postgres/Supabase.
- **Filtro de Grupos e Origem:** Processa apenas mensagens diretas (*1-on-1*) e mensagens recebidas (*incoming*).

---

## 🛠️ Arquitetura do Workflow

1. **Webhook / Normalização:** Recebe dados do Uazapi e padroniza as variáveis da mensagem e do usuário.
2. **Filtro & Roteamento:** Verifica se a mensagem veio de um grupo e identifica o tipo de mídia (Áudio, Imagem, Documento ou Texto).
3. **Transcrição de Áudio (URLaudio):** Caso a mensagem seja um áudio, efetua o download e transcrição via OpenAI.
4. **Agente de IA & Sub-Agentes:** - `consultar_horario`: Ferramenta para buscar horários por curso na tabela `Horarios` do Supabase.
   - `consultar_faq`: Ferramenta para buscar respostas na tabela `FAQ` do Supabase.
5. **Envio da Resposta:** Envia a resposta final formatada de volta ao usuário através do endpoint de texto do Uazapi.

---

## 📋 Pré-requisitos

- Instância do **n8n** (self-hosted ou cloud).
- Conta na **OpenAI** com chave de API ativa.
- Instância do **Supabase** (com PostgreSQL habilitado) para a base de conhecimento e memória do chat.
- Instância da API de mensagens **Uazapi**.

---

## ⚙️ Configuração e Instalação

### 1. Variáveis de Ambiente

Crie um arquivo `.env` na raiz do seu projeto (baseado no `.env.example` fornecido) com as seguintes variáveis:

```env
OPENAI_API_KEY=sua_chave_aqui
UAZAPI_BASE_URL=[https://sua-instancia.uazapi.com](https://sua-instancia.uazapi.com)
