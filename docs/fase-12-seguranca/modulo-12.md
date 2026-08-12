# 📖 Capítulo 12 — Fase 12: Segurança de Aplicações

`↩ Índice Geral: 00-INDICE-GERAL.md` | `⬅ Anterior: modulo-11.md` | `➡ Próximo: modulo-13.md`

---

## 🎯 Objetivo

Segurança não é responsabilidade só de um "time de segurança" — é parte do trabalho de qualquer engenheira, desde o primeiro dia. Um sistema vulnerável não é apenas um problema técnico, é um risco real para pessoas (vazamento de dados) e para o negócio. Esta fase te dá a base para construir sistemas que não caem nas armadilhas mais comuns e mais exploradas do mundo real.

> **Como isso aparece no mercado:** perguntas sobre OWASP Top 10, hashing de senha e prevenção de SQL Injection/XSS são comuns até em entrevistas Junior, especialmente em empresas do setor financeiro (fintechs, bancos digitais) como Nubank, PicPay, Stone.

---

## 📝 Conceitos

- OWASP Top 10 (visão geral)
- Hashing de senha (bcrypt/argon2) vs. criptografia
- HTTPS e por que importa
- SQL Injection
- XSS (Cross-Site Scripting)
- CSRF (Cross-Site Request Forgery)
- Rate Limiting
- Boas práticas gerais (princípio do menor privilégio, defesa em profundidade)

---

## 📋 Ordem de estudo

1. OWASP Top 10 — visão panorâmica
2. Hashing de senha
3. SQL Injection
4. XSS
5. CSRF
6. HTTPS
7. Rate Limiting e boas práticas gerais

---

## 🔍 Explicação

### 1. OWASP Top 10

A OWASP (Open Web Application Security Project) mantém uma lista das 10 vulnerabilidades mais críticas e mais comuns em aplicações web, atualizada periodicamente. Não é necessário decorar a lista, mas é essencial entender as categorias que mais afetam aplicações backend comuns: **Injection** (SQL Injection e afins), **Broken Access Control**, **Cryptographic Failures**, **Security Misconfiguration**, entre outras.

> **Como isso aparece no mercado:** "você conhece o OWASP Top 10?" é uma pergunta comum em entrevistas — mesmo que a resposta completa não seja esperada de uma Junior, reconhecer o conceito e citar 2-3 vulnerabilidades com exemplo já demonstra consciência de segurança.

### 2. Hashing de senha — nunca armazene senha em texto puro

**Hashing** é uma função unidirecional: transforma uma senha em uma sequência que não pode ser revertida de volta à senha original. **Criptografia**, ao contrário, é reversível (com a chave certa) — por isso senha usa hash, não criptografia (você nunca precisa "descriptografar" uma senha, apenas comparar hashes).

```typescript
import bcrypt from 'bcrypt';

// Ao cadastrar:
const hashSenha = await bcrypt.hash(senhaDigitada, 10); // 10 = salt rounds
await salvarUsuario({ ...dados, senha: hashSenha });

// Ao fazer login:
const usuario = await buscarUsuarioPorEmail(email);
const senhaValida = await bcrypt.compare(senhaDigitada, usuario.senha);
```

**Salt** é um valor aleatório adicionado à senha antes do hash, garantindo que duas senhas iguais gerem hashes diferentes — prevenindo ataques com "rainbow tables" (tabelas pré-computadas de hashes conhecidos).

> ⚠️ **Armadilha comum, extremamente grave:** usar `MD5` ou `SHA-256` "puro" para senha. Essas funções são rápidas demais (feitas para outros propósitos, como checksums) e vulneráveis a ataques de força bruta em GPU. **Use sempre bcrypt, argon2 ou scrypt**, que são deliberadamente lentas e configuráveis para isso.

### 3. SQL Injection

Ocorre quando entrada do usuário é concatenada diretamente em uma query SQL, permitindo que um atacante injete comandos SQL maliciosos.

```typescript
// VULNERÁVEL — nunca faça isso
const query = `SELECT * FROM usuarios WHERE email = '${emailDigitado}'`;
// Se emailDigitado = "' OR '1'='1", a query retorna TODOS os usuários

// SEGURO — sempre use queries parametrizadas / prepared statements
const usuario = await db.query('SELECT * FROM usuarios WHERE email = $1', [emailDigitado]);
```

ORMs como Prisma (Fase 8) usam queries parametrizadas por padrão, o que já protege contra essa classe de ataque — mais um motivo (além de produtividade) para usar ORM ou, ao usar SQL puro, sempre parametrizar.

### 4. XSS (Cross-Site Scripting)

Ocorre quando entrada não sanitizada do usuário é renderizada como HTML/JavaScript executável no navegador de outra pessoa.

```javascript
// VULNERÁVEL: se um usuário cadastra o "nome" <script>roubarCookie()</script>,
// e esse nome é renderizado sem escapar, o script executa no navegador de quem visualiza
elemento.innerHTML = usuario.nome;

// SEGURO: usar textContent (não interpreta HTML) ou sanitizar/escapar entrada
elemento.textContent = usuario.nome;
```

No backend, sua responsabilidade é **nunca confiar** que dados armazenados são seguros para renderizar como HTML sem tratamento — e, como visto na Fase 7, armazenar JWT em cookie `HttpOnly` (em vez de `localStorage`) é justamente uma defesa contra XSS roubar tokens de sessão.

### 5. CSRF (Cross-Site Request Forgery)

Ocorre quando um site malicioso induz o navegador de um usuário autenticado a fazer uma requisição indesejada para outro site onde ele está logado (aproveitando cookies enviados automaticamente pelo navegador).

**Defesas:**
- Cookies com `SameSite=Strict` ou `SameSite=Lax` (visto na Fase 7).
- Tokens CSRF (um valor único por sessão/formulário, verificado no servidor).
- Verificar o header `Origin`/`Referer` em operações sensíveis.

### 6. HTTPS

HTTPS criptografa a comunicação entre cliente e servidor (usando TLS), prevenindo que dados (incluindo senhas, tokens) sejam interceptados em trânsito por um atacante na mesma rede (ataque *man-in-the-middle*). Em 2026, **HTTPS não é opcional** — é o padrão mínimo absoluto para qualquer aplicação em produção, e a maioria das plataformas de deploy (Fase 11) já oferece isso automaticamente.

### 7. Rate Limiting

Limita quantas requisições um cliente (por IP, por usuário, por token) pode fazer em um intervalo de tempo, prevenindo abuso (força bruta em login, sobrecarga de API, scraping agressivo).

```typescript
import rateLimit from 'express-rate-limit';

const limiteLogin = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 5, // no máximo 5 tentativas
  message: 'Muitas tentativas de login. Tente novamente mais tarde.',
});

app.post('/login', limiteLogin, loginHandler);
```

> **Como isso aparece no mercado:** rotas de login/cadastro sem rate limiting são um alvo clássico de ataques de força bruta — implementar isso é considerado básico em qualquer API de produção séria.

### 8. Boas práticas gerais

- **Princípio do menor privilégio:** cada componente/usuário deve ter apenas as permissões estritamente necessárias.
- **Defesa em profundidade:** múltiplas camadas de segurança (validação no frontend E no backend, autenticação E autorização, rate limiting E monitoramento) — nunca confie em uma única barreira.
- **Nunca exponha detalhes internos em mensagens de erro** (ex: stack trace completo, versão do banco de dados) para o cliente final.
- **Mantenha dependências atualizadas** — vulnerabilidades conhecidas em bibliotecas desatualizadas são uma das causas mais comuns de incidentes reais.

---

## 💻 O que dominar

- [ ] Explicar por que senha usa hash (bcrypt/argon2) e nunca criptografia reversível
- [ ] Prevenir SQL Injection usando queries parametrizadas/ORM
- [ ] Explicar XSS e como preveni-lo (sanitização, cookies HttpOnly)
- [ ] Explicar CSRF e suas defesas (SameSite, tokens CSRF)
- [ ] Implementar rate limiting em rotas sensíveis
- [ ] Explicar por que HTTPS é obrigatório em produção
- [ ] Aplicar o princípio de defesa em profundidade em um sistema real

---

## ⚠️ Erros comuns

1. Armazenar senha em texto puro ou com hash fraco (MD5/SHA sem salt adequado).
2. Concatenar entrada do usuário diretamente em queries SQL.
3. Renderizar entrada do usuário como HTML sem sanitização.
4. Armazenar tokens sensíveis em `localStorage`, expostos a XSS.
5. Não implementar rate limiting em rotas de autenticação, permitindo força bruta.
6. Expor mensagens de erro detalhadas (stack traces) diretamente ao cliente em produção.
7. Validar dados apenas no frontend, confiando que o backend não precisa revalidar.

---

## 🧠 Exercícios

**Iniciante**
1. Implemente cadastro de usuário com hash de senha usando bcrypt, e login com comparação segura.
2. Identifique e corrija (propositalmente) uma vulnerabilidade de SQL Injection em uma query concatenada manualmente.

**Intermediário**
3. Implemente rate limiting na rota de login da API construída na Fase 7.
4. Configure cookies com `HttpOnly`, `Secure` e `SameSite` corretamente para o token JWT.

**Avançado**
5. Escreva um middleware de tratamento de erro que nunca vaza stack traces ou detalhes internos em produção, mas loga tudo detalhadamente no servidor (introdução a observabilidade segura).

**Desafio final**
6. Faça uma "auditoria de segurança" completa em um dos seus projetos anteriores (Fase 7 ou 8), documentando em um arquivo `SECURITY.md` quais dos itens desta fase já estão cobertos e quais precisam de correção — depois, corrija-os.

---

## 🌱 Projetos

**Projeto 1 — Hardening de segurança do "Sistema de gestão de pedidos"**
Pegue o projeto da Fase 7/11 e aplique todas as práticas desta fase: hash de senha correto, prevenção de SQL Injection (se usando SQL puro em algum ponto), cookies seguros, rate limiting em rotas críticas, e tratamento de erro que não vaza informação sensível. Documente cada mudança no `SECURITY.md` — isso demonstra, de forma concreta e auditável, maturidade de segurança para recrutadores técnicos.

---

## ✔️ Critério de conclusão

Você conclui a Fase 12 quando aplica hash de senha corretamente, previne SQL Injection e XSS nos seus projetos, implementa rate limiting em rotas sensíveis, e consegue explicar cada uma dessas defesas com um exemplo concreto de ataque que ela previne.

> **É isso que empresas realmente esperam de uma Junior?** Sim, no nível de consciência e aplicação prática básica — não se espera que uma Junior seja especialista em segurança ofensiva, mas se espera que ela **não introduza** vulnerabilidades básicas conhecidas no código que escreve.

---

## 📄 Documentações

- **owasp.org/www-project-top-ten** — a fonte oficial do OWASP Top 10.
- **cheatsheetseries.owasp.org** — cheat sheets práticos por tópico (autenticação, senha, etc.) — extremamente úteis como referência de implementação correta.

---

`↩ Índice Geral: 00-INDICE-GERAL.md` | `➡ Próximo: modulo-13.md`
