# 📖 Capítulo 14 — Fase 14: Portfólio

`↩ Índice Geral: README.md` | `⬅ Anterior: Capítulo 13 - modulo-13.md (Projetos Profissionais (Roadmap de Projetos))` | `➡ Próximo: Capítulo 15 - modulo-15.md (Currículo, Networking e Primeira Vaga)`

---

## 🎯 Objetivo

Um projeto excelente, mal apresentado, é invisível para um recrutador que passa, em média, menos de 2 minutos olhando um repositório. Esta fase ensina a transformar seu trabalho técnico em algo que comunica sua competência rapidamente e com credibilidade.

> **Como isso aparece no mercado:** recrutadores técnicos e tech leads frequentemente decidem se avançam um candidato só de olhar o README e a organização do GitHub, antes mesmo de rodar o código.

---

## 📝 Conceitos

- Estrutura de um GitHub profissional
- Anatomia de um README eficaz
- Documentação de decisões técnicas (ADR — Architecture Decision Records, simplificado)
- GIFs e capturas de tela demonstrativas
- Identidade profissional consistente (bio, pinned repos, README de perfil)

---

## 🔍 Explicação

### 1. Estrutura do GitHub como vitrine

Seu perfil GitHub é, na prática, o currículo técnico que muitos recrutadores olham primeiro. Elementos essenciais:

- **Pinned repositories:** fixe os 4-6 melhores projetos (os da Fase 13), não todos os exercícios soltos.
- **README de perfil** (`seu-usuario/seu-usuario`): uma breve apresentação profissional, stack principal, e destaques.
- **Repositórios organizados:** nomes claros, descrição preenchida, tópicos/tags configurados.
- **Atividade consistente:** o gráfico de contribuições conta uma história — consistência importa mais que picos isolados.

### 2. Anatomia de um README eficaz

Um README profissional de projeto segue uma estrutura que responde, em ordem, às perguntas que um avaliador técnico realmente tem:

```markdown
# Nome do Projeto

> Uma frase clara: o que o sistema faz e para quem.

## 🔗 Demo
[Link para o projeto rodando] — sempre que possível.

## 🎯 Problema resolvido
Descreva o problema de negócio real que motivou o projeto (não "projeto para praticar X").

## 🏗️ Arquitetura
Diagrama (Mermaid) mostrando a estrutura em alto nível.

## 🛠️ Stack e por quê
Liste tecnologias com uma linha justificando cada escolha — não apenas o nome.

## ✨ Funcionalidades
Lista objetiva do que o sistema faz.

## 🧪 Testes
Como rodar, e qual a cobertura/estratégia.

## 🚀 Como rodar localmente
Passo a passo real e testado (alguém deveria conseguir rodar seguindo isso, sem perguntar nada).

## 🤔 Decisões técnicas
As 2-3 decisões mais importantes e por que foram tomadas (ex: "por que Postgres em vez de Mongo aqui").

## 📈 Possíveis melhorias
Demonstra visão crítica sobre o próprio trabalho — um sinal de maturidade, não de fraqueza.
```

> ⚠️ **Armadilha comum:** README gerado só com "Clone o repo, npm install, npm start" e nada mais. Isso comunica "terminei um tutorial", não "resolvi um problema". O que diferencia é a seção de **decisões técnicas** e **problema resolvido**.

### 3. GIFs e demonstrações visuais

Um GIF curto (5-15 segundos) mostrando o sistema funcionando, no topo do README, aumenta drasticamente o engajamento de quem visita o repositório — muitos avaliadores nunca chegam a rodar o projeto localmente, então essa é sua única chance de "mostrar" o resultado.

**Ferramentas comuns:** ScreenToGif (Windows, gratuito), Kap (Mac, gratuito), ou gravação de tela + conversão para GIF/vídeo curto.

### 4. Documentando decisões técnicas (ADR simplificado)

Um Architecture Decision Record documenta uma decisão importante, o contexto, as alternativas consideradas e a justificativa — uma prática real usada em times de engenharia maduros para preservar "por que fizemos assim" ao longo do tempo.

```markdown
## ADR-001: Uso de fila assíncrona para processamento de relatórios

**Contexto:** Geração de relatórios grandes bloqueava a requisição HTTP por até 30s.

**Decisão:** Mover o processamento para uma fila (Redis + Bull), retornando
imediatamente um ID de job, com endpoint separado para consultar status.

**Alternativas consideradas:** aumentar timeout do servidor (rejeitada:
não resolve a experiência do usuário nem escalabilidade).

**Consequências:** maior complexidade operacional (precisa de worker rodando),
mas resposta imediata ao usuário e melhor uso de recursos.
```

Incluir 2-3 ADRs nos seus projetos mais complexos (da Fase 13) é um diferencial forte e pouco comum entre Juniors — sinaliza pensamento arquitetural genuíno.

### 5. Identidade profissional consistente

Use a mesma foto, o mesmo nome, e uma bio consistente entre GitHub, LinkedIn e (se tiver) portfólio pessoal. Isso parece trivial, mas recrutadores frequentemente cruzam essas fontes, e inconsistência gera desconfiança sutil e desnecessária.

---

## 💻 O que dominar

- [ ] Escrever um README completo seguindo a estrutura acima, para qualquer projeto
- [ ] Gravar e incluir um GIF demonstrativo em um projeto
- [ ] Escrever pelo menos um ADR documentando uma decisão técnica real
- [ ] Organizar o perfil GitHub com pinned repos e README de perfil

---

## ⚠️ Erros comuns

1. README genérico, copiado do template padrão de um framework, sem personalização.
2. Repositório sem descrição, sem tópicos, com nome tipo "projeto-final" ou "teste123".
3. Não incluir nenhuma evidência visual (GIF/screenshot) do sistema funcionando.
4. Documentar apenas "como rodar", sem nunca explicar "por quê" das decisões tomadas.

---

## 🧠 Exercícios

1. Reescreva o README de pelo menos 2 projetos da Fase 13 seguindo a estrutura completa apresentada aqui.
2. Grave um GIF demonstrativo de um dos seus projetos e inclua no README.
3. Escreva 2 ADRs para decisões reais que você tomou nos projetos da Fase 13.
4. Crie/atualize seu README de perfil do GitHub.

---

## 🌱 Projetos

Esta fase não gera projeto novo — ela **eleva a qualidade de apresentação** dos projetos já construídos na Fase 13, que são os que efetivamente compõem seu Portfólio Final (Anexo B).

---

## ✔️ Critério de conclusão

Você conclui a Fase 14 quando todos os seus projetos principais têm README completo, GIF demonstrativo, e pelo menos uma decisão técnica documentada via ADR — e seu perfil GitHub está organizado como uma vitrine profissional.

> **É isso que empresas realmente esperam de uma Junior?** Sim — e é, honestamente, um dos pontos mais negligenciados por autodidatas, o que torna essa fase um diferencial competitivo desproporcionalmente alto em relação ao esforço necessário.

---

`↩ Índice Geral: README.md` | `➡ Próximo: Capítulo 15 - modulo-15.md (Currículo, Networking e Primeira Vaga)`
