## **Data Metrics App** - Documento de Requisitos de Sistemas(DSR)

Teste

### Projeto:
Plataforma para Análise de Dados de Usuários em Tempo Real.

---

### Objetivo do Sistema:
Fornecer uma solução SaaS para empresas coletarem, processarem e visualizarem dados comportamentais de usuários (web, apps, IoT) em tempo real, com dashboards personalizáveis, alertas inteligentes e integrações com ferramentas de marketing.

---

### Escopo:
Funcionalidades Principais:
- Coleta de eventos via Script (JavaScript).
- Processamento em tempo real com agregação de métricas.
- Dashboard interativo com gráficos e filtros dinâmicos.
- Sistema de alertas personalizáveis (Slack, e-mail).
- Gestão de usuários e projetos (planos free/paid).
- API para integração com ferramentas externas (Zapier, Shopify).

---

### Requisitos Funcionais:
RF01 - Coleta de Dados:
- RF01.1: Script deve rastrear cliques e pageviews automaticamente.
- RF01.2: Script deve permitir eventos customizados.
- RF01.3: Script deve ter suporte para aplicações SPA's (mudança de rotas)

RF02 - Processamento:
- RF02.1: Agregação de métricas a cada 1 minuto (pageviews, usuários ativos).
- RF02.2: Enriquecimento de dados (Ex.: geolocalização via IP).
- RF02.3: Retenção de eventos brutos por 30 dias.

RF03 - Dashboard:
- RF03.1: Gráficos em tempo real (linhas, barras, mapas de calor).
- RF03.2: Filtros por período, dispositivo, localização.
- RF03.3: Exportação de relatórios em PDF/CSV.

RF04 – Alertas
- RF04.1: Configuração de regras baseadas em métricas (ex: "usuários ativos < 100 em 1h").
- RF04.2: Notificações via e-mail e Slack.
- RF04.3: Histórico de alertas disparados.

RF05 – Gestão de Usuários
- RF05.1: Cadastro com e-mail/senha ou SSO (Google/facebook).
- RF05.2: Planos (Free, Starter, Pro) com limites de eventos/dia.
- RF05.3: Geração de autorização da API por projeto.

RF06 – API
- RF06.1: Endpoints REST para consultar métricas e eventos.
- RF06.2: Sistema multi-tenant com migrations
- RF06.3: Autenticação via JWT.

---

### Requisitos não Funcionais:
RNF01 – Performance
- Processar grande carga de eventos/segundo por cliente.
- Baix latência entre coleta e exibição no dashboard.

RNF02 – Escalabilidade
- Kafka deve escalar horizontalmente para lidar com picos de tráfego.
- PostgreSQL com replicação de leitura para consultas em dashboards.

RNF03 – Segurança
- Criptografia de dados em trânsito (HTTPS) e em repouso (AES-256).
- Compliance com GDPR e LGPD (opção de exclusão de dados).

RNF04 – Disponibilidade
- Alta disponibilidade para clientes Pro.
- Media disponibilidade para demais clientes.

RNF05 – Usabilidade
- Dashboard responsivo (mobile/desktop).
- Documentação completa para desenvolvedores.

---

### Arquitetura:
Componentes Principais:
- Coleta de Dados: Script mais Bff (Javascript e node.js com TS).
- Injestão de Dados: Apache Kafka para Recebimento de Eventos.
- Processamaneto de Dados: Node.js + Kafka consumer
- Armazenamento: PostgresSQL + Redis + S3(backup de eventos brutos)
- API: REST node.js com TS
- Frontend: Next.js + D3.js para dashboards

---

### Diagrama de Fluxo:

Cliente -> (SDK+BFF) -> kafka -> node.js(consumer + Processamento) -> API -> PostgresSQL -> Dashboard next.js 

---

### User Stories:
US01 – Como Marketing Manager
- "Quero ver um gráfico de pageviews por hora para ajustar campanhas em tempo real."

US02 – Como Desenvolvedor
- "Preciso integrar o SDK em nosso app React Native e validar os dados no dashboard."

US03 – Como SysAdmin
- "Devo receber um alerta se o volume de eventos cair 50% em 10 minutos."

---

### Casos de Uso:
Caso de Uso 01 – Configurar Alerta
- Usuário acessa a seção "Alertas".
- Define métrica (ex: "erros"), condição ("> 5%") e canais de notificação.
- Sistema valida e ativa o alerta.

Caso de Uso 02 – Visualizar Dashboard
- Usuário seleciona projeto e intervalo de tempo.
- Sistema carrega métricas pré-agregadas do PostgreSQL.
- Gráficos são atualizados em tempo real via WebSocket.

---

### Regras de Negocio:
RN01: Clientes do plano Free têm acesso a apenas 3 dashboards.  
RN02: Alertas só podem ser configurados no plano Pro.  
RN03: Dados brutos são excluídos após 30 dias.  

---

### Riscos X Metigação
- Alta latência no processamento: Otimizar consultas com TimescaleDB e índices.
- Perda de dados: Backups diários no S3 e replicação do Kafka.
- Ataques DDoS:	Uso de Cloudflare/WAF e rate limiting na API.

---

### Roadmap
- MVP (3 meses): SDK básico + dashboard simples.
- Fase 2 (6 meses): Alertas e integrações.
- Fase 3 (12 meses): Análise preditiva e suporte a IoT.

