# FASE 0: Mentalidade de Engenharia

## 🎯 Objetivo

Um erro comum ao iniciar um projeto de estudos é pensar somente no conteúdo que será consumido, mas não no processo de consumo do mesmo. Previamente à primeira linha de código ou ao primeiro projeto no portfólio, é preciso ter um **sistema operacional** mental que alinhe suas expectativas em relação ao que será aprendido: dentro do contexto deste guia, estamos adentrando a **mentalidade da pessoa engenheira**.

Sem essa estrutura como alicerce, é comum que o estudo autodidata fique confuso ou se perca no meio do caminho, seja por falta de organização ou de retenção. Organizar previamente um sistema de estudos é o que permite um preparo adequado para entrevistas técnicas ou para o processo de identificação e correção de bugs no dia a dia de um deploy em produção, sem criar grandes dificuldades por não saber nem por onde começar.

> Esta fase existe para resolver um problema específico: **transformar consumo de conteúdo em capacidade real**, mensurável e demonstrável.

---

## 📝 Conceitos

- Active Recall (recuperação ativa)
- Spaced Repetition (repetição espaçada)
- Leitura de documentação técnica (documentation-first learning)
- Leitura técnica em inglês
- Método de resolução de problemas (Polya / Feynman Technique)
- GitHub como diário de evolução (learning in public)
- Inglês técnico (vocabulário mínimo viável)
- Como pesquisar como engenheira (Google-fu, Stack Overflow, RFCs, changelogs)
- Uso responsável de IA no aprendizado (IA como par, não como substituto)
- Deliberate Practice (prática deliberada) vs. prática ingênua
    
---

## 📋 Ordem de estudo

1. Entenda **por que** memorização passiva não funciona (a Curva do Esquecimento de Ebbinghaus).
2. Aprenda **Active Recall** e aplique imediatamente em qualquer conteúdo que consumir daqui para frente.
3. Configure um sistema de **Spaced Repetition** (Anki).
4. Adote o hábito de **ler documentação oficial primeiro**, vídeo depois (nunca o contrário).
5. Configure seu **GitHub** como diário de evolução desde o dia 1 (antes mesmo de saber programar bem).
6. Construa seu **vocabulário técnico em inglês** em paralelo — não depois.
7. Aprenda a **técnica de Feynman** para validar se você realmente entendeu algo.
8. Defina suas **regras pessoais de uso de IA** (detalhado abaixo) antes de tocar em qualquer assistente de código.
    

---

## 🔍 Explicação

### 1. Por que "estudar" do jeito tradicional não funciona?

Desde o primeiro contato com o processo de aprendizagem nas escolas, aprendemos que os métodos de estudo funcionam da seguinte forma: você abre o caderno, assiste ao professor lecionar o conteúdo e copia tanto o conteúdo passado na lousa quanto a explicação verbal feita pelo professor. Mas, de fato, isso se converte em aprendizado?

O fato é que, até mesmo no processo de leitura de livros conceituais, há uma aquisição passiva — você reconhece a origem do conceito, mas não consegue colocá-lo em prática por ter apenas criado um reconhecimento. Mas, no dia a dia, trabalhando com código ou participando de uma entrevista técnica, seu **cérebro** precisa ativamente exercitar aquele conhecimento e aplicá-lo no processo de debug — e isso exige muito mais processo cognitivo do que apenas reconhecer.

Isso é comprovado por décadas de pesquisa em ciência cognitiva (Roediger & Karpicke, Cepeda et al.). A curva de esquecimento mostra que, sem reforço, esquecemos a maior parte de um novo conteúdo em poucos dias. A solução não é "estudar mais", é **estudar de forma diferente**.

### 2. Active Recall (Recuperação Ativa)

Ao invés de copiar conteúdos ou marcar textos que nunca mais serão relidos, podemos utilizar a recuperação ativa, exercitando atividades que ajudam efetivamente a combater o processo da curva do esquecimento.

Esse processo faz com que o cérebro precise reforçar as conexões neurais estabelecidas no processo de aquisição daquele conteúdo, porém de forma **ativa**. Na prática:

- Depois de estudar um conceito, feche o material e escreva (ou fale em voz alta) tudo o que lembra, sem consultar nada.
- Crie perguntas sobre o conteúdo e tente respondê-las depois de um tempo, sem olhar a resposta.
- Ao terminar um exercício de código, **não copie e cole a solução** — reescreva do zero depois de entender a lógica.

Isso é fisicamente mais difícil e desconfortável do que reler — e é exatamente esse desconforto que fortalece a memória de longo prazo.

### 3. Spaced Repetition (Repetição Espaçada)

Mesmo exercitando as conexões neurais após a aquisição de conteúdo, precisamos encontrar meios de manter aquele conhecimento exercitado e constante, sedimentando-o na memória de longo prazo — para isso, podemos combinar a recordação ativa com a **repetição espaçada**. Os intervalos crescentes reforçam as bases e abrem caminho para outros conceitos mais complexos, sem a sensação de ter esquecido algo importante no meio do caminho.

Em vez de revisar um conceito todos os dias (ineficiente) ou nunca mais (esquecimento total), você revisa em intervalos que aumentam conforme você demonstra domínio: 1 dia, 3 dias, 7 dias, 15 dias, 30 dias...

**Ferramenta padrão da indústria:** Anki (gratuito, open-source). Crie flashcards para:

- Definições de conceitos (o que é Big O, o que é uma thread, o que é normalização)  
- Sintaxe que você erra com frequência
- Perguntas de entrevista técnica

> ⚠️ **Armadilha comum:** usar Anki para decorar sintaxe de código de memória, palavra por palavra. Isso é ineficaz. Use Anki para **conceitos, definições e "por quês"** — a sintaxe você fixa escrevendo código de verdade, repetidamente.

### 4. Documentação primeiro, vídeo depois

No processo de aprendizagem autodidata, é comum que o conhecimento fique direcionado para vídeos do YouTube. Vídeos são ótimos para uma primeira exposição visual ao conceito, ainda mais se tratando de um contexto atrelado à tecnologia, mas **documentação oficial é a fonte da verdade** — é o que profissionais consultam no dia a dia, é sempre atualizada e é o que você vai precisar saber ler fluentemente no trabalho.

A partir de agora, o fluxo padrão para aprender qualquer tecnologia nova é:

1. Ler a seção "Getting Started" / "Introduction" da documentação oficial.
2. Reproduzir os exemplos da própria documentação, sem vídeo.
3. Só recorrer a vídeo ou curso quando travar em algo específico.
4. Voltar à documentação para aprofundar (nunca considerar um vídeo como fonte final).
    
> **Como isso aparece no mercado:** Engenheiros seniores raramente "fazem um curso" para aprender uma lib nova — eles leem a documentação e o código-fonte. Treinar esse músculo cedo é uma vantagem competitiva enorme para uma Junior.

### 5. GitHub como diário de evolução

Desde o primeiro dia, é importante que todo código escrito (mesmo os exercícios mais simples) seja documentado diretamente no GitHub, com commits regulares. Isso cumpre três funções:

- **Prova de trabalho:** recrutadores e tech leads olham o histórico de commits, não apenas o "produto final".
- **Autoavaliação:** ao olhar seu código de 3 meses atrás, você literalmente vê sua evolução — e isso é um dos maiores motivadores de continuidade que existe.
- **Hábito profissional:** commitar com frequência, escrever mensagens de commit descritivas e manter repositórios organizados é uma habilidade que será cobrada no seu primeiro emprego, desde o primeiro dia.

> 💡 **Boas práticas:** crie um repositório chamado algo como `software-engineer` ou similar, com uma pasta por fase deste guia. Cada exercício, mesmo os pequenos, vira um commit com mensagem clara (ex.: `feat: implementa busca binária recursiva`).

### 6. Inglês técnico

O mercado de tecnologia é dominado pelo inglês, logo, não é preciso ser fluente em inglês para começar, mas é preciso desenvolver **inglês técnico de leitura** com urgência, porque:

- A quase totalidade da documentação de qualidade é em inglês.
- Mensagens de erro, logs e stack traces são em inglês.
- Boa parte das vagas competitivas (remotas, multinacionais) exige inglês em algum nível.
- Entrevistas em empresas como Google, Microsoft e Amazon frequentemente são conduzidas (ou têm etapas) em inglês.

**Como treinar em paralelo:**

- Configure o idioma do seu sistema operacional, IDE e ferramentas em inglês.
- Leia toda a documentação em inglês (nunca em traduções, que costumam estar desatualizadas).
- Assista a talks técnicas legendadas em inglês (não dubladas).
- Escreva seus commits e comentários de código em inglês desde já — é o padrão da indústria global.
    

### 7. Técnica de Feynman

Aprender um conteúdo pode ser facilitado quando simplificamos a conceituação dele, visando explicá-lo para alguém que nunca teve contato com aquele conceito — isso é a técnica de Feynman na prática. O processo:

1. Escolha um conceito (ex.: "o que é uma API REST").
2. Escreva a explicação como se estivesse ensinando para alguém sem nenhum conhecimento técnico.
3. Identifique onde você travou, usou jargão sem explicar ou "enrolou".
4. Volte ao material, estude o ponto em que travou e repita.
    

> **Aplicação prática:** ao final de cada fase deste guia, escreva um post curto (pode ser privado, num README, num blog) explicando o que você aprendeu, como se fosse para outra pessoa autodidata começando do zero. Isso é parte do "O que dominar" de toda fase.

### 8. IA como ferramenta de aprendizagem — não como muleta

No mercado atual, é impossível falar de tecnologia sem abordar os impactos da Inteligência Artificial dentro do setor. Evitar o uso de IA é uma prática que retrocede o processo de desenvolvimento de carreira, porém sua adoção no dia a dia deve ser mensurada de acordo com o momento atual e os conhecimentos adquiridos durante o estudo.

Ferramentas de IA (como Claude, ChatGPT, GitHub Copilot, Cursor) são extraordinariamente poderosas para acelerar o aprendizado — **quando usadas corretamente**. Usadas incorretamente, elas produzem o efeito oposto: uma pessoa que "entrega projetos", mas não consegue passar em uma entrevista técnica, porque nunca de fato desenvolveu o raciocínio.

**Regras práticas que você deve adotar a partir de hoje:**

|Uso correto ✅|Uso incorreto ❌|
|---|---|
|Pedir para a IA explicar um conceito de formas diferentes até você entender|Pedir para a IA resolver o exercício e você só copiar|
|Pedir para a IA revisar seu código já escrito e apontar melhorias|Pedir para a IA escrever o projeto inteiro do zero|
|Usar IA para debugar depois de você tentar sozinha por um tempo limite|Colar o erro na IA no primeiro sinal de dificuldade|
|Pedir perguntas de entrevista simuladas para você responder|Pedir para a IA te dar "as respostas certas" para decorar|
|Usar IA para gerar casos de teste extras depois que você escreveu a lógica|Usar IA para gerar toda a lógica de negócio|

**Regra de ouro:** trate a IA como um par sênior disponível 24h — você ainda precisa pensar, tentar, errar e só então pedir ajuda direcionada. Se você consegue "terminar" um projeto usando IA, mas não consegue explicar linha por linha o que o código faz e por quê, você não aprendeu — você apenas produziu um artefato.

> ⚠️ **Armadilha comum em 2026:** montar um portfólio inteiro "vibe coded" (gerado quase inteiramente por IA, sem compreensão) e ser reprovada na primeira pergunta técnica sobre o próprio projeto em entrevista. Isso já é um padrão reconhecido por recrutadores — e é motivo automático de eliminação em processos sérios.

---

## 💻 O que dominar

Ao final da Fase 0, você deve ser capaz de, sem consultar nada:

- Explicar o que é Active Recall e aplicá-lo em qualquer novo conteúdo de estudo
- Configurar e usar Anki (ou similar) para revisão espaçada
- Ler a documentação oficial de uma tecnologia nova como primeira fonte, antes de vídeo
- Ter um repositório GitHub configurado como diário de evolução, com commits organizados
- Escrever commits, comentários e código em inglês
- Aplicar a técnica de Feynman para validar seu próprio entendimento
- Ter regras pessoais claras e escritas de como (e quando) usar IA nos estudos
    
---

## ⚠️ Erros comuns

1. **"Tutorial hell"**: consumir curso atrás de curso sem nunca aplicar, porque parece produtivo, mas é fuga do desconforto de praticar.
2. **Ilusão de competência**: reler o mesmo material várias vezes e confundir familiaridade com domínio.
3. **Estudar sem medir**: não ter nenhum critério objetivo de "eu sei isso ou não sei", vivendo na zona cinzenta.
4. **Terceirizar o raciocínio para IA**: usar IA para gerar soluções completas e nunca desenvolver a capacidade de resolver problemas sozinha.
5. **Não documentar a jornada**: chegar em uma entrevista sem conseguir mostrar evolução real, porque nunca commitou nada, nunca escreveu sobre o que aprendeu.
6. **Ignorar inglês até "ser necessário"**: adiar o inglês técnico e travar meses depois ao tentar ler uma RFC ou documentação avançada.

---

## 🧠 Exercícios

- **Iniciante**
1. Crie sua conta no GitHub (se ainda não tiver) e crie o repositório `estudos-engenharia-de-software`.
2. Instale o Anki e crie seu primeiro deck: "Fundamentos de Engenharia de Software".
3. Escreva, em um arquivo `README.md`, sua própria definição (com suas palavras, sem copiar) do que é Active Recall e por que ele funciona.
    
- **Intermediário**
1. Escolha um conceito técnico qualquer que você já "conhece superficialmente" (ex.: API, banco de dados, variável) e aplique a Técnica de Feynman por escrito. Identifique pelo menos dois pontos em que você travou.
2. Configure seu ambiente (sistema, editor de código) em inglês.

- **Avançado**
1. Escreva, em texto, suas 5 regras pessoais de uso de IA nos estudos — o que você vai permitir e o que não vai permitir a si mesma fazer. Faça commit desse arquivo no seu repositório.
    
- **Desafio final**
1. Pelos próximos 7 dias, mantenha um "log de estudo" diário no seu GitHub (um commit por dia, mesmo que pequeno), descrevendo o que estudou e o que conseguiu recordar sem consultar material (Active Recall aplicado à própria rotina).
    
---

## 🌱 Projetos

Esta fase não tem projeto de código — o "projeto" é o **sistema de estudo** que você acabou de construir: repositório GitHub ativo, Anki configurado, rotina de leitura de documentação e regras de uso de IA por escrito. Isso será a espinha dorsal de todas as fases seguintes.

---

## ✔️ Critério de conclusão

Você conclui a Fase 0 quando:

- Tem um repositório GitHub ativo, com pelo menos 7 commits reais.
- Tem um sistema de Spaced Repetition funcionando (Anki configurado e com pelo menos um deck em uso).
- Consegue explicar por escrito, sem consultar, o que é Active Recall, Spaced Repetition e a Técnica de Feynman.
- Tem suas regras de uso de IA escritas e commitadas.
    
> **É isso que empresas realmente esperam de uma Junior?** Não diretamente — nenhuma empresa vai perguntar "você usa Anki?". Mas toda empresa vai notar a **consequência** disso: uma Junior que aprende rápido, retém conhecimento, documenta seu trabalho e não trava quando a IA não está disponível. Essa fase é o alicerce invisível que torna todas as próximas mais eficientes.

---

## Checklist consolidado — Fase 0


- Sei explicar Active Recall, Spaced Repetition e Técnica de Feynman
- Tenho GitHub configurado como diário de evolução, com commits reais
- Tenho Anki configurado e em uso
- Tenho minhas regras pessoais de uso de IA escritas

---

`↩ Voltar ao Índice Geral: README.md` `➡ Próximo: Capítulo 1 — modulo-01.md (Fundamentos da Computação)`
