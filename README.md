# 🤖 Projeto de Automação de Mensagens — n8n + WAHA

Este repositório documenta o fluxo automatizado desenvolvido em **n8n**, integrado ao **WAHA (WhatsApp HTTP API)** e **hospedado na AWS** utilizando **Docker**.  
O sistema realiza o envio controlado de mensagens personalizadas com base em dados de planilhas, incluindo controle de horário, dia e registros de execução.

---

## ☁️ Arquitetura do Projeto

O projeto é totalmente **containerizado** e executado em uma instância **AWS (EC2)**.  
Os serviços principais são:

- **n8n** → responsável pela automação e orquestração dos fluxos.
- **WAHA (WhatsApp HTTP API)** → responsável por enviar mensagens via WhatsApp.
- **Docker Compose** → gerencia os containers e seus volumes persistentes.
- **AWS EC2** → infraestrutura de hospedagem do ambiente.

---

## 🧩 Fluxo de Automação

![Fluxo n8n](./fluxo-n8n.png)

### Etapas do Workflow:
1. **Trigger manual** – o fluxo inicia quando o usuário clica em **"Executar workflow"**.
2. **Leitura de planilha** – o sistema lê a planilha de contatos e mensagens.
3. **Extração e formatação** – os dados são tratados via código JavaScript.
4. **Loop** – percorre cada linha/contato.
5. **Envio de mensagem** – o n8n envia mensagens através do WAHA.
6. **Registro do envio** – cada resultado é registrado de volta na planilha.
7. **Controle de contadores** – contabiliza o progresso dos envios.
8. **Validação de horário/dia** – garante que mensagens só sejam enviadas em horários permitidos.
9. **Delays (Wait)** – adiciona pausas automáticas entre envios, evitando bloqueios do WhatsApp.

---

## 🧱 Estrutura de Arquivos


- ├── docker-compose.yaml 
- ├── media/ 
- ├── sessions/
- ├── fluxo-n8n.png 
- └── README.md 

---

## ⚙️ Ambiente de Execução

| Recurso | Descrição |
|----------|------------|
| **Plataforma** | AWS EC2 (Ubuntu) |
| **Orquestração** | Docker Compose |
| **Containers** | n8n + WAHA |
| **Porta WAHA** | 3000 (localhost) |
| **Rede Docker** | rede-projeto-gabi |

---

## 🧠 Objetivo

Automatizar o envio de mensagens de WhatsApp de forma escalável, rastreável e dentro de horários controlados.  
O fluxo garante:
- Redução de erros manuais no envio;
- Controle de horários e dias de operação;
- Logs e histórico de execução em planilha;
- Persistência de sessões e mídias via Docker.

---

## 🔒 Segurança e Manutenção

- O acesso à instância AWS é restrito via SSH.
- Credenciais sensíveis estão armazenadas no arquivo `.env` (não versionado).
- Atualizações da imagem WAHA podem ser aplicadas com:
  ```bash
  docker compose pull && docker compose up -d
