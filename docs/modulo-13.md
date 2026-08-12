# 📖 Capítulo 13 — Fase 13: Projetos Profissionais (Roadmap de Projetos)

`↩ Índice Geral: README.md` | `⬅ Anterior: Capítulo 12 - modulo-12.md (Cloud, Docker e CI/CD)` | `➡ Próximo: Capítulo 14 - modulo-14.md (Portfólio)`

---

## 🎯 Objetivo

Até aqui, cada fase gerou projetos pontuais para consolidar um conceito específico. Esta fase existe para integrar tudo o que você aprendeu em **projetos completos, de complexidade crescente**, que simulam problemas reais de empresas — não tutoriais reproduzidos. O objetivo é sair desta fase com 3 a 4 sistemas robustos, que servirão de base direta para o seu Portfólio Final (Anexo B).

> **Como isso aparece no mercado:** recrutadores técnicos e tech leads avaliam portfólio pela **complexidade de decisão**, não pela quantidade de projetos. Um projeto que resolve um problema real de negócio, com arquitetura justificada, vale mais que dez CRUDs simples.

---

## 📝 Conceitos

- Como transformar um "problema de negócio" em requisitos técnicos
- Requisitos funcionais vs. não funcionais
- Como justificar decisões de stack e arquitetura
- Como simular complexidade real (regras de negócio, concorrência, escala) mesmo em projeto pessoal

---

## 📋 Ordem de estudo

Esta fase não é sequencial por conceito, mas por **complexidade crescente de projeto**. Recomenda-se construir os projetos abaixo nesta ordem, cada um levando de 2 a 4 semanas dependendo do seu ritmo (ver cronogramas no Anexo A).

---

## 🔍 Explicação

### O que separa um projeto de portfólio "júnior comum" de um "júnior competitivo"

A maioria dos autodidatas constrói: to-do list, blog pessoal, clone de rede social. Esses projetos têm um problema estrutural: **não têm regra de negócio real**. Eles testam CRUD, mas não testam decisão. Uma empresa não contrata alguém para fazer CRUD — contrata alguém para **resolver problemas com restrições reais** (dinheiro, tempo, concorrência, regras que mudam).

Os projetos desta fase são desenhados para forçar decisões reais:

- Regras de negócio que mudam de comportamento dependendo do contexto (não apenas "salvar no banco").
- Concorrência real (dois usuários competindo pelo mesmo recurso).
- Estados que evoluem (pedido pendente → pago → enviado → entregue, cada estado com regras diferentes).
- Integração entre múltiplas partes do sistema (autenticação + regras + notificação + persistência).

### Roteiro de projetos desta fase

**Projeto A — Sistema de Reservas com Concorrência (nível: consolidação)**
Já prototipado na Fase 8. Aqui, você o expande: adicione cancelamento com política de reembolso baseada em tempo (ex: cancelamento com mais de 24h = reembolso total; menos de 24h = parcial), fila de espera quando não há vaga, notificação por e-mail (simulada) na confirmação. Adicione testes completos (Fase 9), Docker e CI/CD (Fase 11).

**Projeto B — Motor de Precificação Dinâmica (nível: regras de negócio complexas)**
Construa uma API que calcula preço final de um produto/serviço com base em múltiplas regras combináveis: desconto por volume, cupom promocional (com regras de expiração e uso único), taxa por forma de pagamento, frete calculado por distância/peso. O desafio central é arquitetural: como estruturar regras que se combinam sem virar um emaranhado de `if/else` (aplique Strategy Pattern e Open/Closed Principle da Fase 10).

**Projeto C — Sistema de Filas e Processamento Assíncrono (nível: arquitetura distribuída introdutória)**
Construa um sistema onde uma ação do usuário (ex: "gerar relatório", "processar imagem") não é executada imediatamente na requisição, mas enfileirada (usando Redis/Bull ou similar) e processada em segundo plano, com um endpoint para consultar o status do processamento. Isso introduz, de forma prática, o conceito de processamento assíncrono desacoplado — um padrão extremamente comum em sistemas reais de escala (processamento de pagamento, geração de PDF, envio de e-mail em massa).

**Projeto D — API Multi-tenant com Controle de Acesso Granular (nível: mais avançado)**
Construa uma API onde múltiplas "organizações" (tenants) compartilham a mesma aplicação, mas com dados completamente isolados entre si, e papéis de usuário com permissões granulares (admin, editor, visualizador) dentro de cada organização. Este é o modelo usado por praticamente todo SaaS B2B (ex: Notion, Slack, sistemas de gestão empresarial) — entender isolamento de dados multi-tenant é um diferencial técnico real.

---

## 💻 O que dominar

- [ ] Transformar uma descrição de problema de negócio em requisitos funcionais e não funcionais claros
- [ ] Justificar por escrito cada decisão de arquitetura e stack tomada em um projeto
- [ ] Modelar regras de negócio combináveis sem acoplamento excessivo
- [ ] Implementar processamento assíncrono desacoplado da requisição HTTP
- [ ] Implementar isolamento de dados multi-tenant

---

## ⚠️ Erros comuns

1. Construir projetos sem regra de negócio real (apenas CRUD disfarçado).
2. Não documentar as decisões técnicas tomadas — um projeto sem explicação de "por quê" perde a maior parte do seu valor demonstrativo.
3. Superestimar escopo e nunca terminar (melhor um projeto completo e polido que três inacabados).
4. Ignorar testes e CI/CD nesses projetos "porque são só para portfólio" — na prática, é exatamente esses projetos que mais precisam demonstrar essas habilidades.

---

## 🧠 Exercícios

Esta fase é, por natureza, orientada a projeto — os "exercícios" são os próprios projetos A, B, C e D, construídos incrementalmente. Trate cada projeto com o mesmo rigor de commits pequenos (Fase 6), testes (Fase 9) e arquitetura em camadas (Fase 10) já praticados.

---

## 🌱 Projetos

Ver "Roteiro de projetos desta fase" acima — Projetos A, B, C e D são o núcleo desta fase e alimentarão diretamente o Portfólio Final (Anexo B).

---

## ✔️ Critério de conclusão

Você conclui a Fase 13 quando tem pelo menos 2 dos 4 projetos (A, B, C, D) completos, com testes, documentação de decisões técnicas, Docker e deploy real — prontos para serem apresentados como peças centrais do seu portfólio final.

> **É isso que empresas realmente esperam de uma Junior?** Sim — este é exatamente o nível de complexidade que diferencia um portfólio "genérico de curso" de um portfólio que gera convites para entrevista.

---

`↩ Índice Geral: README.md` | `➡ Próximo: Capítulo 14 - modulo-14.md (Portfólio)`
