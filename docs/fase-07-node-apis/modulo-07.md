# 📖 Capítulo 7 — Fase 7: Node.js, APIs REST e Autenticação

`↩ Índice Geral: README.md` | `⬅ Anterior: Capítulo 6 - modulo-06.md (Git, GitHub e Fluxo Profissional)` | `➡ Próximo:  Capítulo 8 - modulo-08.md (Banco de Dados)`

---

## 🎯 Objetivo

Esta é a fase central da sua trilha de Backend. Aqui você aprende a construir sistemas que realmente servem dados para o mundo — APIs que outras aplicações (frontend, mobile, outros serviços) consomem. O objetivo não é "saber usar Express", é entender profundamente como um servidor HTTP funciona, como estruturar uma API de forma profissional, e como lidar com os problemas reais que toda API enfrenta: autenticação, validação, erros, arquivos, logs.

> **Como isso aparece no mercado:** esta é, literalmente, a fase que mais aparece em desafios técnicos de vagas Backend Junior JS/TS — "construa uma API REST com autenticação" é um dos testes técnicos mais comuns do mercado brasileiro (Nubank, iFood, Stone, PicPay e centenas de outras empresas usam variações disso).

---

## 📝 Conceitos

- Node.js: runtime, módulos, `npm`/`pnpm`
- HTTP a fundo: métodos, status codes, headers, body
- Express.js: rotas, middlewares, request/response
- NestJS: arquitetura modular, decorators, injeção de dependência
- Design de API REST (recursos, verbos, convenções de URL)
- Validação de dados de entrada (Zod, class-validator)
- Autenticação: sessão vs. token
- JWT (JSON Web Tokens) — estrutura e uso correto
- Cookies (HttpOnly, Secure, SameSite)
- Upload e manipulação de arquivos
- Logs estruturados
- Middlewares (autenticação, tratamento de erro, rate limiting)
- Arquitetura em camadas (controller → service → repository)

---

## 📋 Ordem de estudo

1. Node.js e o ecossistema `npm`
2. HTTP a fundo (revisão aprofundada da Fase 1)
3. Express.js — servidor mínimo, rotas, middlewares
4. Design de API REST (convenções profissionais)
5. Validação de entrada
6. Autenticação (sessão, JWT, cookies)
7. Upload de arquivos e logs
8. NestJS — evolução arquitetural do Express

---

## 🔍 Explicação

### 1. Node.js — o runtime

Node.js permite executar JavaScript **fora do navegador**, usando o motor V8 (mesmo motor do Chrome — lembra da Fase 1?) mais um conjunto de APIs para sistema de arquivos, rede, etc. Seu modelo é **single-threaded com I/O não-bloqueante**, usando o Event Loop que você estudou na Fase 3 — só que agora aplicado a requisições HTTP reais, leitura de banco de dados, chamadas a outras APIs.

> **Por que isso importa:** entender que o Event Loop é o que permite Node.js lidar com milhares de conexões simultâneas sem criar uma thread por conexão (diferente de modelos tradicionais) é uma vantagem de performance real — e é uma pergunta clássica de entrevista ("por que Node.js é bom para I/O intensivo mas ruim para CPU intensivo?").

### 2. Design de API REST

REST (Representational State Transfer) é um estilo arquitetural, não um protocolo — mas na prática, "API REST" hoje significa um conjunto de convenções amplamente aceitas:

| Verbo HTTP | Uso | Exemplo |
|---|---|---|
| `GET` | Buscar recurso(s) | `GET /produtos` |
| `POST` | Criar recurso | `POST /produtos` |
| `PUT` | Substituir recurso inteiro | `PUT /produtos/123` |
| `PATCH` | Atualizar parcialmente | `PATCH /produtos/123` |
| `DELETE` | Remover recurso | `DELETE /produtos/123` |

**Convenções profissionais de URL:**
- Recursos são substantivos no plural: `/produtos`, não `/getProdutos` ou `/produto`.
- Hierarquia representa relação: `/usuarios/123/pedidos`.
- Filtros e paginação via query string: `/produtos?categoria=eletronicos&pagina=2`.

**Status codes corretos** (um dos pontos mais checados em code review e entrevistas):

| Código | Significado | Quando usar |
|---|---|---|
| 200 | OK | Sucesso em GET/PUT/PATCH |
| 201 | Created | Sucesso ao criar (POST) |
| 204 | No Content | Sucesso sem corpo de resposta (ex: DELETE) |
| 400 | Bad Request | Dados inválidos enviados pelo cliente |
| 401 | Unauthorized | Não autenticado |
| 403 | Forbidden | Autenticado, mas sem permissão |
| 404 | Not Found | Recurso não existe |
| 409 | Conflict | Conflito de estado (ex: e-mail já cadastrado) |
| 422 | Unprocessable Entity | Validação semântica falhou |
| 500 | Internal Server Error | Erro não tratado no servidor |

> ⚠️ **Armadilha comum:** retornar sempre `200` (mesmo em erros) com um campo `{ erro: true }` no corpo. Isso quebra convenções HTTP, dificulta debugging e é considerado falha de design em qualquer review sério.

### 3. Express.js — o essencial

```javascript
import express from 'express';
const app = express();
app.use(express.json()); // middleware para parsear JSON do body

app.get('/produtos/:id', (req, res) => {
  const { id } = req.params;
  res.status(200).json({ id, nome: 'Produto exemplo' });
});

app.listen(3000, () => console.log('Servidor rodando na porta 3000'));
```

**Middlewares** são o conceito central do Express: funções que interceptam a requisição antes (ou depois) do handler final, usadas para autenticação, logging, tratamento de erro, validação.

```mermaid
graph LR
    A[Requisição] --> B[Middleware: Logger]
    B --> C[Middleware: Autenticação]
    C --> D[Middleware: Validação]
    D --> E[Handler da rota]
    E --> F[Resposta]
```

### 4. Validação de entrada

**Nunca confie em dados vindos do cliente** — este é um princípio de segurança fundamental (aprofundado na Fase 12). Use bibliotecas de validação em vez de checagens manuais espalhadas:

```typescript
import { z } from 'zod';

const criarUsuarioSchema = z.object({
  nome: z.string().min(2),
  email: z.string().email(),
  senha: z.string().min(8),
});

// no handler:
const resultado = criarUsuarioSchema.safeParse(req.body);
if (!resultado.success) {
  return res.status(400).json({ erros: resultado.error.issues });
}
```

**Zod** é hoje um dos padrões dominantes do mercado JS/TS para validação (junto de `class-validator`, mais usado no ecossistema NestJS).

### 5. Autenticação: sessão vs. token

| | Sessão (cookie de sessão) | Token (JWT) |
|---|---|---|
| Onde o estado fica | No servidor (memória/Redis) | No próprio token, "stateless" |
| Escalabilidade horizontal | Precisa de sessão compartilhada (Redis) | Mais simples — qualquer servidor valida o token |
| Revogação imediata | Fácil (apaga a sessão) | Mais difícil (token válido até expirar, a menos que use blacklist) |
| Uso típico | Aplicações web tradicionais monolíticas | APIs consumidas por SPA, mobile, múltiplos serviços |

### 6. JWT — estrutura e uso correto

Um JWT tem três partes, separadas por ponto: `header.payload.assinatura`, cada uma codificada em Base64.

```
eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxMjMiLCJyb2xlIjoidXNlciJ9.assinatura...
```

- **Header:** algoritmo usado (ex: HS256).
- **Payload:** dados (claims) — ex: id do usuário, papel, data de expiração. **Nunca coloque dados sensíveis aqui** (senha, dados pessoais) — o payload é apenas codificado, não criptografado, qualquer um pode decodificar e ler.
- **Assinatura:** garante que o token não foi alterado (mas não esconde o conteúdo).

```typescript
import jwt from 'jsonwebtoken';

const token = jwt.sign({ sub: usuario.id, role: usuario.role }, process.env.JWT_SECRET, { expiresIn: '1h' });

// Middleware de verificação:
function autenticar(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.status(401).json({ erro: 'Token ausente' });
  try {
    req.usuario = jwt.verify(token, process.env.JWT_SECRET);
    next();
  } catch {
    return res.status(401).json({ erro: 'Token inválido' });
  }
}
```

> ⚠️ **Armadilha comum, extremamente frequente em portfólios de Junior:** armazenar JWT no `localStorage` do navegador. Isso expõe o token a ataques XSS (Fase 12). A prática recomendada profissionalmente é armazenar em **cookie HttpOnly**, inacessível via JavaScript no navegador.

### 7. Cookies seguros

```javascript
res.cookie('token', jwtToken, {
  httpOnly: true,  // inacessível via JS (previne XSS)
  secure: true,    // só enviado via HTTPS
  sameSite: 'strict', // previne CSRF
  maxAge: 3600000,
});
```

### 8. NestJS — evolução arquitetural

Depois de entender Express "por baixo do capô", NestJS se torna muito mais fácil de compreender de verdade (em vez de "decorar decorators"). NestJS impõe uma arquitetura opinativa, inspirada em Angular, baseada em:

- **Módulos** (`@Module`) — organizam a aplicação em blocos coesos.
- **Controllers** (`@Controller`) — recebem requisições HTTP.
- **Providers/Services** (`@Injectable`) — contêm a lógica de negócio.
- **Injeção de Dependência** — o NestJS gerencia automaticamente a criação e injeção de dependências entre classes, um padrão vindo diretamente de linguagens como Java/C# — e essencial de entender para a Fase 10 (Arquitetura).

```typescript
@Controller('produtos')
export class ProdutosController {
  constructor(private readonly produtosService: ProdutosService) {}

  @Get(':id')
  buscarPorId(@Param('id') id: string) {
    return this.produtosService.buscarPorId(id);
  }
}
```

> **Como isso aparece no mercado:** NestJS é hoje o framework Node dominante em empresas de médio/grande porte no Brasil e no mundo para backend TypeScript, justamente por impor estrutura e escalar bem em times grandes — Express puro (ou Fastify) ainda é comum em microsserviços menores e projetos que precisam de controle total.

---

## 💻 O que dominar

- [ ] Explicar o ciclo completo de uma requisição HTTP (rota → middleware → handler → resposta)
- [ ] Construir uma API REST seguindo convenções corretas de verbo, URL e status code
- [ ] Validar dados de entrada com uma biblioteca (Zod ou class-validator)
- [ ] Implementar autenticação via JWT corretamente, com armazenamento seguro (cookie HttpOnly)
- [ ] Escrever middlewares customizados (autenticação, tratamento de erro, logging)
- [ ] Explicar a diferença entre sessão e token, e quando usar cada um
- [ ] Estruturar uma API em camadas (controller → service → repository)
- [ ] Explicar a arquitetura modular do NestJS e injeção de dependência

---

## ⚠️ Erros comuns

1. Colocar toda a lógica de negócio dentro do handler da rota (sem separar em camadas).
2. Não validar dados de entrada, confiando no que o cliente envia.
3. Armazenar JWT em `localStorage` em vez de cookie HttpOnly.
4. Colocar dados sensíveis no payload do JWT.
5. Retornar status codes incorretos (sempre 200, ou 500 para erros de validação do usuário).
6. Não tratar erros assíncronos em middlewares/handlers, derrubando o servidor.
7. Misturar responsabilidades: um service que também formata resposta HTTP, um controller que acessa banco diretamente.

---

## 🧠 Exercícios

**Iniciante**
1. Construa um servidor Express com 3 rotas simples (`GET`, `POST`, `DELETE`) para uma lista de tarefas em memória (sem banco ainda).
2. Adicione um middleware de logging que imprime método, rota e tempo de resposta de cada requisição.

**Intermediário**
3. Adicione validação com Zod nas rotas de criação, retornando 400 com mensagens claras em caso de erro.
4. Implemente autenticação completa: cadastro (com hash de senha — antecipa Fase 12), login (retornando JWT em cookie HttpOnly), e uma rota protegida que só funciona autenticado.

**Avançado**
5. Refatore o projeto anterior para arquitetura em camadas (controller/service/repository em memória), separando claramente responsabilidades.
6. Implemente um middleware de tratamento de erro centralizado (`error handler`), eliminando `try/catch` repetido em cada rota.

**Desafio final**
7. Recrie a mesma API usando NestJS, mantendo as mesmas funcionalidades, e compare a experiência com Express puro — escreva, no README, quando você usaria cada um profissionalmente.

---

## 🌱 Projetos

**Projeto 1 — API de gestão de pedidos (e-commerce simplificado)**
Construa uma API REST completa com: autenticação JWT, CRUD de produtos, criação de pedidos com múltiplos itens, cálculo de total, validação de estoque, e diferentes papéis (cliente vs. admin, com rotas protegidas por papel). Este é o tipo de sistema que reflete diretamente problemas reais de e-commerce (Mercado Livre, Amazon), com regras de negócio genuínas, não um CRUD trivial.

**Projeto 2 — API de agendamento com regras de conflito (estilo clínica/salão)**
Construa uma API para agendamento de horários que impede conflitos (dois agendamentos não podem sobrepor o mesmo profissional/horário), com autenticação, validação de regras de negócio complexas (horário de funcionamento, duração de serviço), e logs de auditoria (quem criou/alterou cada agendamento). Este tipo de problema (concorrência de recursos, regras de negócio não triviais) aparece constantemente em sistemas reais de empresas de serviços.

---

## ✔️ Critério de conclusão

Você conclui a Fase 7 quando constrói uma API REST completa, com autenticação segura, validação, arquitetura em camadas, e consegue explicar cada decisão técnica (por que JWT em cookie HttpOnly, por que separar em camadas, por que cada status code) sem consultar.

> **É isso que empresas realmente esperam de uma Junior?** Sim — esta é provavelmente a fase mais diretamente testada em processos seletivos reais de Backend Junior JS/TS no mercado brasileiro e internacional em 2026.

---

## 📄 Documentações

- **nodejs.org/docs** — documentação oficial do Node.js.
- **expressjs.com** — documentação oficial do Express.
- **docs.nestjs.com** — documentação oficial do NestJS, muito bem estruturada.
- **zod.dev** — documentação da biblioteca Zod.

---

`↩ Índice Geral: README.md` | `➡ Próximo: Capítulo 8 - modulo-08.md (Banco de Dados)`
