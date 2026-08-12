# 📖 Anexo B — Portfólio Final: 10 Projetos Progressivos

`↩ Índice Geral: README.md` | `⬅ Anterior: Captulo 17 - cronogramas.md (Cronogramas de Estudo)`

---

## Introdução

Este anexo consolida o portfólio final que resulta da aplicação prática de todo o guia. São **10 projetos**, organizados em ordem crescente de complexidade, que juntos demonstram: fundamentos sólidos, raciocínio algorítmico, domínio de backend moderno (Node/TypeScript), modelagem de dados, arquitetura, testes, segurança, cloud/DevOps e pensamento de produto/negócio.

Você não precisa necessariamente construir os 10 — **3 a 4 projetos muito bem executados (especialmente os projetos 5 em diante) valem mais que 10 rasos.** Use os primeiros como degraus de aprendizado; use os últimos como as peças centrais do seu portfólio público.

---

## Projeto 1 — CLI de Gestão de Tarefas com Persistência em Arquivo

**Fase(s) de origem:** 2, 6

**Objetivo de negócio:** Simula uma ferramenta interna de produtividade — o tipo de utilitário que times de engenharia frequentemente constroem para uso próprio.

**Requisitos funcionais:** adicionar, listar, concluir, remover e priorizar tarefas via linha de comando; persistir dados em arquivo JSON entre execuções.
**Requisitos não funcionais:** operações devem ser O(log n) ou melhor para busca por ID (uso de estrutura de dados adequada, não busca linear ingênua em listas grandes); dados não podem ser perdidos em caso de encerramento abrupto do processo.

**Arquitetura sugerida:** camada de comando (parsing de argumentos) → camada de lógica (regras de negócio) → camada de persistência (leitura/escrita de arquivo), claramente separadas.

**Tecnologias:** Node.js + TypeScript, sem frameworks — para consolidar fundamentos antes de abstrações.

**Organização do repositório:** `src/commands`, `src/core`, `src/storage`, `tests/`.

**Critérios de qualidade:** cobertura de testes unitários na camada `core`; tratamento de erro para arquivo corrompido/inexistente.

**Funcionalidades obrigatórias:** CRUD completo de tarefas, priorização, persistência.
**Diferenciais:** undo da última ação (pilha de comandos — aplica estrutura de dados da Fase 2), busca por texto.

**Testes:** unitários para toda a lógica de negócio (camada `core`), isolando a camada de arquivo com um fake/mock.
**Deploy:** não aplicável (ferramenta CLI); publicar como pacote `npm` (opcional, diferencial).
**README:** deve incluir GIF do terminal em uso.

**Perguntas de entrevista possíveis:** "Por que você escolheu essa estrutura de dados para buscar tarefas por ID?"; "Como você garante que os dados não sejam corrompidos se o processo for interrompido no meio de uma escrita?".

---

## Projeto 2 — API de Controle Financeiro Pessoal

**Fase(s) de origem:** 7, 8, 12

**Objetivo de negócio:** Simula um módulo real de fintech — controle de receitas, despesas e categorização, com regras de consistência financeira rígidas.

**Requisitos funcionais:** cadastro/login de usuário, CRUD de transações (receita/despesa), categorização, saldo calculado (nunca armazenado diretamente — sempre derivado, para evitar inconsistência), relatório mensal.
**Requisitos não funcionais:** toda operação que afeta saldo deve ser transacional (ACID); senha nunca em texto puro; nenhuma rota sensível sem autenticação.

**Arquitetura sugerida:** Controller → Service → Repository, com validação de entrada (Zod) em toda rota de escrita.

**Tecnologias:** Node.js, Express ou NestJS, TypeScript, PostgreSQL, JWT em cookie HttpOnly.

**Organização do repositório:** estrutura em camadas por módulo de domínio (`modules/usuarios`, `modules/transacoes`), não por tipo técnico genérico.

**Critérios de qualidade:** testes unitários para cálculo de saldo/relatório; testes de integração para os endpoints principais.

**Funcionalidades obrigatórias:** autenticação, CRUD de transações, cálculo de saldo, relatório por período.
**Diferenciais:** metas de economia com progresso calculado; exportação de relatório em CSV/PDF.

**Testes:** unitários (cálculo de saldo/relatório) + integração (fluxo completo de criação de transação e verificação de saldo).
**Deploy:** Render/Railway com PostgreSQL gerenciado.
**README:** seção de decisões técnicas explicando por que saldo é calculado, não armazenado.

**Perguntas de entrevista possíveis:** "Por que o saldo não é um campo armazenado no usuário?"; "O que aconteceria se duas transações fossem criadas simultaneamente — como você garante consistência?".

---

## Projeto 3 — API de E-commerce: Gestão de Pedidos

**Fase(s) de origem:** 7, 8, 9, 10 (Projeto central da Fase 7)

**Objetivo de negócio:** Núcleo de qualquer plataforma de e-commerce — gestão de catálogo, carrinho, pedidos e estoque.

**Requisitos funcionais:** CRUD de produtos (admin), carrinho por usuário, criação de pedido com múltiplos itens, baixa de estoque atômica, papéis diferenciados (cliente/admin).
**Requisitos não funcionais:** baixa de estoque não pode gerar valores negativos mesmo sob concorrência (lock otimista ou transação com verificação); todas as regras de preço/desconto centralizadas (sem duplicação entre carrinho e pedido).

**Arquitetura sugerida:** Clean Architecture simplificada (entidades de domínio isoladas de Express/Prisma), Repository Pattern para acesso a dados.

**Tecnologias:** NestJS, TypeScript, PostgreSQL, Prisma, Zod/class-validator.

**Organização do repositório:** `src/domain` (entidades e regras puras), `src/application` (casos de uso), `src/infra` (banco, HTTP).

**Critérios de qualidade:** zero lógica de negócio em controllers; SOLID aplicado e justificado no README.

**Funcionalidades obrigatórias:** catálogo, carrinho, pedido, controle de estoque, papéis de acesso.
**Diferenciais:** cupons de desconto (Strategy Pattern), histórico de status do pedido.

**Testes:** unitários para regras de estoque/preço; integração para o fluxo completo de checkout.
**Deploy:** containerizado (Docker), deploy real com CI/CD (Fase 11).
**README:** diagrama Mermaid de arquitetura em camadas.

**Perguntas de entrevista possíveis:** "Como você evita que dois pedidos simultâneos vendam o último item em estoque duas vezes?"; "Por que separar entidades de domínio de Prisma?".

---

## Projeto 4 — Sistema de Reservas com Concorrência

**Fase(s) de origem:** 8, 9, 12, 13 (Projeto A)

**Objetivo de negócio:** Sistema de agendamento (salas, mesas, consultas) — problema clássico de concorrência de recursos físicos/temporais.

**Requisitos funcionais:** criação de reserva com verificação de conflito de horário, cancelamento com política de reembolso por tempo, fila de espera quando não há vaga.
**Requisitos não funcionais:** garantia absoluta (via constraint de banco + transação, não apenas validação em aplicação) de que dois horários conflitantes nunca coexistem para o mesmo recurso.

**Arquitetura sugerida:** camada de domínio com regra de conflito isolada e testável; constraint de exclusão no PostgreSQL (`EXCLUDE` com `tsrange`, avançado) como segunda linha de defesa.

**Tecnologias:** Node.js/NestJS, TypeScript, PostgreSQL, Redis (fila de espera).

**Critérios de qualidade:** teste de concorrência real (duas requisições simultâneas tentando reservar o mesmo horário) comprovando que apenas uma sucede.

**Funcionalidades obrigatórias:** reserva, cancelamento com política de reembolso, verificação de conflito.
**Diferenciais:** fila de espera automática, notificação (simulada) quando uma vaga abre.

**Testes:** teste de integração específico simulando concorrência (duas Promises disparadas em paralelo).
**Deploy:** Docker + deploy real.
**README:** ADR explicando a decisão entre lock otimista, pessimista, ou constraint de banco.

**Perguntas de entrevista possíveis:** "Como você testou que o sistema realmente previne dupla reserva sob concorrência?"; "Qual a diferença entre resolver isso na aplicação vs. no banco de dados, e por que você escolheu essa abordagem?".

---

## Projeto 5 — Motor de Precificação Dinâmica

**Fase(s) de origem:** 3, 10, 13 (Projeto B)

**Objetivo de negócio:** Núcleo de precificação usado por e-commerces e marketplaces — combinação de múltiplas regras de desconto/taxa sem gerar código inextensível.

**Requisitos funcionais:** aplicar desconto por volume, cupom (com regras de expiração/uso único), taxa por forma de pagamento, cálculo de frete simplificado — todos combináveis.
**Requisitos não funcionais:** adicionar uma nova regra de precificação não deve exigir alterar código existente (Open/Closed Principle, validado na prática).

**Arquitetura sugerida:** Strategy Pattern para cada tipo de regra, com um "motor" que as compõe (padrão Chain of Responsibility ou pipeline funcional).

**Tecnologias:** TypeScript puro (a lógica central não deveria depender de framework HTTP).

**Critérios de qualidade:** cobertura de testes altíssima nesta lógica específica (é lógica de negócio crítica — erro aqui tem impacto financeiro direto).

**Funcionalidades obrigatórias:** as 4 regras citadas, compostas corretamente, com ordem de aplicação bem definida e documentada.
**Diferenciais:** simulação de "o que aconteceria se essa regra não existisse" (comparação de cenários), interface simples via API.

**Testes:** unitários exaustivos, incluindo combinações de múltiplas regras e casos de borda (desconto que zeraria o preço, cupom expirado, etc.).
**Deploy:** exposto via API simples com Express.
**README:** explicação de como adicionar uma nova regra sem tocar em código existente (prova prática de OCP).

**Perguntas de entrevista possíveis:** "Como você garante que a ordem de aplicação das regras está correta e é previsível?"; "Se amanhã surgir uma nova regra de imposto regional, o que muda no seu código?".

---

## Projeto 6 — Sistema de Processamento Assíncrono com Filas

**Fase(s) de origem:** 7, 8, 11, 13 (Projeto C)

**Objetivo de negócio:** Simula geração de relatórios/processamento de imagem/envio de e-mail em massa — qualquer operação demorada demais para bloquear a requisição HTTP.

**Requisitos funcionais:** endpoint que enfileira um job e retorna imediatamente um ID; worker separado que processa a fila; endpoint de consulta de status (pendente/processando/concluído/falho).
**Requisitos não funcionais:** falha em um job não pode travar o worker (deve seguir processando os próximos); jobs falhos devem ter retry com backoff.

**Arquitetura sugerida:** API (produtor) e Worker (consumidor) como processos separados, comunicando-se via fila Redis (Bull/BullMQ).

**Tecnologias:** Node.js/TypeScript, Redis, BullMQ, Docker Compose (orquestrando API + Worker + Redis).

**Critérios de qualidade:** logs estruturados por job; idempotência (reprocessar o mesmo job não deve gerar efeito duplicado).

**Funcionalidades obrigatórias:** enfileiramento, processamento assíncrono, consulta de status, retry com backoff.
**Diferenciais:** dashboard simples de monitoramento de fila (quantos pendentes, processando, falhos).

**Testes:** testes de integração do fluxo completo (enfileira → aguarda processamento → verifica status final); teste específico de retry após falha simulada.
**Deploy:** Docker Compose documentado, deploy real (API + Worker rodando separadamente).
**README:** diagrama de sequência (Mermaid) do fluxo produtor-consumidor.

**Perguntas de entrevista possíveis:** "Por que separar API e Worker em processos diferentes em vez de processar tudo na própria requisição?"; "O que acontece se o Worker cair no meio do processamento de um job?".

---

## Projeto 7 — API Multi-tenant SaaS com Controle de Acesso Granular

**Fase(s) de origem:** 8, 10, 12, 13 (Projeto D)

**Objetivo de negócio:** Modelo usado por praticamente todo SaaS B2B — múltiplas organizações isoladas na mesma aplicação, com papéis de usuário dentro de cada uma.

**Requisitos funcionais:** cadastro de organização, convite de membros com papéis (admin/editor/visualizador), qualquer recurso (ex: projetos, documentos) pertence a uma organização e é inacessível por membros de outra.
**Requisitos não funcionais:** isolamento de dados deve ser garantido mesmo sob erro de programação (defesa em profundidade — filtro de tenant tanto na aplicação quanto, idealmente, via Row-Level Security no PostgreSQL).

**Arquitetura sugerida:** middleware de resolução de tenant (a partir do token/subdomínio), aplicado a toda query; RBAC (Role-Based Access Control) como camada de autorização separada da autenticação.

**Tecnologias:** NestJS, TypeScript, PostgreSQL com Row-Level Security, JWT com claim de organização.

**Critérios de qualidade:** teste específico de segurança comprovando que um usuário da Organização A nunca consegue acessar dados da Organização B, mesmo manipulando IDs diretamente na requisição.

**Funcionalidades obrigatórias:** multi-tenancy, RBAC, convite de membros.
**Diferenciais:** auditoria de ações (quem fez o quê, quando) por organização.

**Testes:** teste de segurança de isolamento (tentativa deliberada de acesso cross-tenant); testes de autorização por papel.
**Deploy:** Docker + deploy real com banco gerenciado.
**README:** ADR sobre a escolha de isolamento (schema por tenant vs. coluna `tenant_id` vs. RLS) e por quê.

**Perguntas de entrevista possíveis:** "Como você garante que um bug de programação não vaze dados entre organizações?"; "Compare as estratégias de isolamento multi-tenant que você conhece e por que escolheu essa."

---

## Projeto 8 — Plataforma de API Pública com Gestão de API Keys e Rate Limiting

**Fase(s) de origem:** 7, 11, 12

**Objetivo de negócio:** Simula um produto "API as a Service" (como muitas fintechs e SaaS oferecem hoje) — gestão de chaves de API, planos com limites diferentes, e billing simplificado por uso.

**Requisitos funcionais:** geração de API keys por usuário, autenticação via API key (não JWT de usuário), planos com limites de requisição diferentes (free/pro), contabilização de uso.
**Requisitos não funcionais:** rate limiting deve ser preciso mesmo com múltiplas instâncias do servidor rodando (rate limiting distribuído via Redis, não em memória local).

**Arquitetura sugerida:** middleware de autenticação por API key + middleware de rate limiting com Redis (sliding window ou token bucket).

**Tecnologias:** Node.js/TypeScript, Redis, PostgreSQL (registro de uso).

**Critérios de qualidade:** teste de carga simples comprovando que o rate limit é respeitado sob múltiplas requisições concorrentes.

**Funcionalidades obrigatórias:** geração/revogação de API key, rate limiting por plano, contabilização de uso.
**Diferenciais:** dashboard de uso para o desenvolvedor consumidor da API; webhook de notificação ao atingir 80% do limite.

**Testes:** testes de integração para rate limiting sob concorrência; testes unitários para lógica de plano/limite.
**Deploy:** Docker + deploy real, com Redis gerenciado.
**README:** explicação do algoritmo de rate limiting escolhido (token bucket vs. sliding window) e por quê.

**Perguntas de entrevista possíveis:** "Por que rate limiting em memória local não funciona com múltiplas instâncias do servidor?"; "Qual algoritmo de rate limiting você escolheu e quais são os trade-offs em relação aos outros?".

---

## Projeto 9 — Sistema de Notificações em Tempo Real

**Fase(s) de origem:** 7, 8, 11

**Objetivo de negócio:** Simula funcionalidade central de qualquer produto moderno (chat, notificações de pedido, atualizações ao vivo) — comunicação em tempo real entre servidor e cliente.

**Requisitos funcionais:** conexão via WebSocket, envio de notificação para usuário(s) específico(s), histórico de notificações persistido, marcação de lida/não lida.
**Requisitos não funcionais:** reconexão automática do cliente após queda de conexão sem perda de notificações (uso de fila/buffer no servidor para mensagens não entregues).

**Arquitetura sugerida:** gateway WebSocket (Socket.io ou WS nativo do NestJS) + serviço de persistência de notificações + Redis Pub/Sub caso haja múltiplas instâncias do servidor (para uma notificação chegar ao usuário independente de qual instância ele está conectado).

**Tecnologias:** NestJS com `@nestjs/websockets`, Redis Pub/Sub, PostgreSQL.

**Critérios de qualidade:** teste simulando desconexão e reconexão, validando que nenhuma notificação é perdida.

**Funcionalidades obrigatórias:** conexão em tempo real, envio direcionado, histórico, status de leitura.
**Diferenciais:** notificações em grupo/broadcast, preferências de notificação por usuário.

**Testes:** testes de integração com cliente WebSocket simulado.
**Deploy:** Docker, deploy em plataforma com suporte a WebSocket persistente (verificar compatibilidade — nem toda plataforma serverless suporta bem).
**README:** diagrama de arquitetura mostrando o papel do Redis Pub/Sub em múltiplas instâncias.

**Perguntas de entrevista possíveis:** "Como você garante que uma notificação chega ao usuário certo se ele está conectado em uma instância diferente do servidor que originou o evento?"; "O que acontece se o cliente ficar offline por um tempo — como as notificações perdidas são recuperadas?".

---

## Projeto 10 — Sistema Completo com Observabilidade (Projeto-síntese)

**Fase(s) de origem:** todas as fases — projeto de consolidação final

**Objetivo de negócio:** Este projeto integra tudo: pegue o **Projeto 3 (E-commerce)** ou o **Projeto 7 (SaaS Multi-tenant)** — os mais robustos — e adicione uma camada completa de observabilidade e confiabilidade, simulando o padrão de maturidade esperado de um sistema real em produção em uma empresa séria.

**Requisitos funcionais adicionais:** logs estruturados e centralizados; métricas básicas (tempo de resposta por endpoint, taxa de erro); health check endpoint (`/health`) usado por orquestradores.
**Requisitos não funcionais:** o sistema deve continuar respondendo (com degradação graciosa) mesmo se uma dependência externa (ex: Redis) estiver indisponível — não deve derrubar a aplicação inteira.

**Arquitetura sugerida:** middleware de logging estruturado (ex: `pino`), endpoint `/health` verificando conectividade com banco/Redis, tratamento de erro que classifica falhas (erro do cliente vs. erro do servidor vs. indisponibilidade de dependência externa).

**Tecnologias:** todas as anteriores + `pino` (logs), possivelmente Prometheus/Grafana (introdução, se o tempo permitir) para métricas.

**Critérios de qualidade:** simular a queda de uma dependência (ex: derrubar o container do Redis) e comprovar que a aplicação principal continua respondendo requisições que não dependem dela.

**Funcionalidades obrigatórias:** logs estruturados, health check, tratamento gracioso de falha de dependência.
**Diferenciais:** dashboard de métricas básico, alertas simples (ex: notificação quando taxa de erro ultrapassa um limiar).

**Testes:** teste de resiliência (derrubar dependência e validar comportamento esperado, não apenas testes de "caminho feliz").
**Deploy:** pipeline de CI/CD completo (Fase 11), com o sistema efetivamente rodando publicamente.
**README:** este é o projeto "carro-chefe" do portfólio — deve ter a documentação mais completa de todas: arquitetura, decisões técnicas (múltiplos ADRs), demonstração em vídeo/GIF, e uma seção explícita conectando cada fase deste guia ao que foi aplicado no projeto.

**Perguntas de entrevista possíveis:** "O que você faria se esse sistema precisasse suportar 100x mais tráfego amanhã?"; "Como você saberia, em produção, que algo deu errado antes que um usuário reclamasse?"; "Descreva a decisão mais difícil que você tomou nesse projeto e por quê."

---

## Como apresentar este portfólio em entrevistas

1. **Não apresente os 10 projetos igualmente** — tenha 2-3 "projetos-carro-chefe" (recomendação: Projeto 3 ou 7, e o Projeto 10) que você conhece profundamente e consegue discutir por 20+ minutos sem travar.
2. **Prepare-se para ser questionada sobre qualquer decisão** — se você usou PostgreSQL, esteja pronta para justificar por que não MongoDB; se usou Strategy Pattern, esteja pronta para explicar a alternativa mais simples que você descartou.
3. **Tenha os links funcionando** antes de qualquer entrevista — nada comunica menos confiança do que um deploy fora do ar no momento em que um recrutador tenta acessar.
4. **Conecte cada projeto a um problema de negócio real**, não a uma tecnologia — recrutadores técnicos maduros se importam mais com "que problema isso resolve e por quê" do que com "quais tecnologias você usou".

---

## Fechamento do guia

Você chegou ao fim de um roadmap equivalente, em profundidade, ao esperado de uma excelente formação em Ciência da Computação aplicada — construído para autodidatas, alinhado ao mercado real de 2026/2027. O caminho não termina na primeira vaga: engenharia de software é uma disciplina de aprendizado contínuo. Os hábitos da Fase 0 (Active Recall, documentação como fonte primária, uso responsável de IA) são, propositalmente, os que vão te acompanhar por toda a carreira — muito depois de qualquer tecnologia específica deste guia ter ficado obsoleta.

---

`↩ Voltar ao Índice Geral: 00-INDICE-GERAL.md`
