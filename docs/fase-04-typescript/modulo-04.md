# 📖 Capítulo 4 — Fase 4: TypeScript

`↩ Índice Geral: README.md` | `⬅ Anterior: Capítulo 3 -  modulo-03.md (JavaScript Moderno)` | `➡ Próximo:  Capítulo 5 - modulo-05.md (HTML, CSS, Responsividade e Acessibilidade)`

---

## 🎯 Objetivo

TypeScript é, em 2026, o padrão de fato para backend profissional em JS — a maioria das vagas sérias de Node.js/NestJS pede TypeScript, não JavaScript puro. Esta fase existe para você entender **por que** tipagem estática importa em sistemas reais (não é "burocracia", é prevenção de uma classe inteira de bugs) e dominar o sistema de tipos o suficiente para trabalhar em qualquer codebase profissional.

> **Como isso aparece no mercado:** vagas de backend JS em empresas como Nubank, iFood, PicPay e a maioria das startups sérias hoje exigem TypeScript como requisito, não como diferencial.

---

## 📝 Conceitos

- Tipos básicos e inferência de tipos
- Interfaces vs. Type Aliases
- Union Types e Intersection Types
- Generics
- Tipos utilitários (`Partial`, `Pick`, `Omit`, `Record`, etc.)
- Enums
- `unknown` vs. `any` vs. `never`
- Type Narrowing (guards de tipo)
- Configuração do `tsconfig.json` (`strict` mode)
- Tipagem de funções (parâmetros opcionais, sobrecarga)
- Decorators (introdução — essencial para NestJS na Fase 7)

---

## 📋 Ordem de estudo

1. Tipos básicos, inferência e por que tipagem estática importa
2. Interfaces e Type Aliases
3. Union/Intersection Types e Type Narrowing
4. Generics
5. Tipos utilitários
6. `tsconfig.json` e modo `strict`
7. Introdução a Decorators

---

## 🔍 Explicação

### 1. Por que tipagem estática importa

Em JavaScript puro, um erro como chamar `.toUpperCase()` em um número só aparece **em tempo de execução** — possivelmente em produção, possivelmente depois que o cliente já viu o erro. Em TypeScript, o compilador pega esse erro **antes mesmo de rodar o código**, durante o desenvolvimento.

Isso não é sobre "escrever mais código" — é sobre mover a detecção de erros o mais cedo possível no ciclo de desenvolvimento (o princípio de "shift left" em qualidade de software), que é exatamente o que empresas sérias exigem em escala, quando dezenas de pessoas mexem no mesmo código.

> **Como isso aparece no mercado:** em code review, um Pull Request com tipos mal definidos (ou abuso de `any`) é motivo comum de solicitação de mudanças (change request) em times que levam qualidade a sério.

### 2. Inferência de tipos

TypeScript frequentemente **infere** o tipo automaticamente, sem que você precise anotar tudo manualmente:

```typescript
let nome = "Ana"; // inferido como string
let idade = 28;   // inferido como number

function somar(a: number, b: number) {
  return a + b; // retorno inferido como number
}
```

> 💡 **Boa prática:** deixe o TypeScript inferir quando possível (evita verbosidade desnecessária), mas **sempre** anote explicitamente parâmetros de função e retornos de funções públicas/exportadas — isso documenta a intenção e evita inferências erradas silenciosas.

### 3. Interface vs. Type Alias

```typescript
interface Usuario {
  id: string;
  nome: string;
  email?: string; // opcional
}

type Status = 'ativo' | 'inativo' | 'suspenso'; // union type
```

| | `interface` | `type` |
|---|---|---|
| Extensão | `extends` | `&` (intersection) |
| Pode ser reaberta (declaration merging) | Sim | Não |
| Union types | Não | Sim |
| Convenção de mercado | Objetos e contratos de classes | Unions, tipos primitivos compostos, funções |

> **Regra prática usada pela maioria dos times profissionais:** use `interface` para formatos de objeto (especialmente contratos de API, entidades), use `type` para unions, intersections e tipos mais complexos.

### 4. Type Narrowing (estreitamento de tipo)

Quando você tem um tipo mais amplo (união), o TypeScript permite "estreitar" para um tipo mais específico usando checagens em tempo de execução:

```typescript
function processar(valor: string | number) {
  if (typeof valor === 'string') {
    return valor.toUpperCase(); // TS sabe que aqui é string
  }
  return valor.toFixed(2); // TS sabe que aqui é number
}
```

### 5. Generics

Generics permitem escrever código reutilizável que funciona com múltiplos tipos, mantendo segurança de tipo:

```typescript
function primeiroItem<T>(lista: T[]): T | undefined {
  return lista[0];
}

primeiroItem<string>(['a', 'b']); // string | undefined
primeiroItem<number>([1, 2]);     // number | undefined
```

Generics são a base de praticamente todo código de biblioteca profissional (ex: `Array<T>`, `Promise<T>`, e mais tarde, os `Repository<T>` que você vai construir na Fase 10 de arquitetura).

### 6. `any` vs `unknown` vs `never`

- **`any`:** desliga completamente a checagem de tipos. **Evite** — é a "porta dos fundos" que anula todo o propósito do TypeScript.
- **`unknown`:** também aceita qualquer valor, mas **exige verificação** antes de usar — é o substituto seguro para `any`.
- **`never`:** representa um valor que nunca deveria ocorrer (ex: uma função que sempre lança exceção, ou um branch impossível em um `switch` exaustivo).

> ⚠️ **Armadilha comum:** usar `any` "para o TypeScript parar de reclamar" em vez de resolver o tipo corretamente. Isso é considerado um anti-padrão sério em times profissionais e é frequentemente bloqueado por linters (`eslint` com regra `no-explicit-any`).

### 7. Tipos utilitários

TypeScript vem com utilitários prontos, extremamente usados no dia a dia profissional:

```typescript
interface Usuario { id: string; nome: string; email: string; senha: string; }

type UsuarioPublico = Omit<Usuario, 'senha'>;      // remove um campo
type UsuarioParcial = Partial<Usuario>;            // todos os campos opcionais
type ApenasNomeEmail = Pick<Usuario, 'nome' | 'email'>; // seleciona campos
type MapaDeUsuarios = Record<string, Usuario>;     // dicionário tipado
```

> **Como isso aparece no mercado:** ao criar um endpoint de atualização parcial (`PATCH`), é comum usar `Partial<Entidade>` para o corpo da requisição — isso é praticamente onipresente em APIs REST profissionais.

### 8. `tsconfig.json` e modo `strict`

O modo `strict: true` ativa um conjunto de verificações rigorosas (`strictNullChecks`, `noImplicitAny`, etc.) que é **o padrão esperado em qualquer projeto profissional sério**. Projetos sem modo estrito geralmente acumulam bugs de tipo silenciosos.

```json
{
  "compilerOptions": {
    "strict": true,
    "target": "ES2022",
    "module": "commonjs",
    "esModuleInterop": true
  }
}
```

---

## 💻 O que dominar

- [ ] Explicar por que tipagem estática reduz uma classe de bugs em produção
- [ ] Escrever interfaces e types corretamente, sabendo quando usar cada um
- [ ] Usar union types e Type Narrowing com `typeof`/`in`/type guards customizados
- [ ] Escrever funções e estruturas genéricas simples
- [ ] Usar `Partial`, `Pick`, `Omit`, `Record` fluentemente
- [ ] Explicar por que `any` deve ser evitado e quando `unknown` é a alternativa correta
- [ ] Configurar um `tsconfig.json` com modo `strict`

---

## ⚠️ Erros comuns

1. Abusar de `any` para "resolver" erros de tipo rapidamente.
2. Não configurar `strict: true`, perdendo boa parte do valor do TypeScript.
3. Confundir `interface` e `type` sem critério, gerando inconsistência no código do time.
4. Tipar demais coisas óbvias (verboso) e tipar de menos coisas críticas (parâmetros de função pública).
5. Não entender a diferença entre tipo em tempo de compilação e validação em tempo de execução — TypeScript **não** valida dados vindos de fora (API, formulário) automaticamente; isso é feito com bibliotecas de validação (Zod, class-validator — vistas na Fase 7).

---

## 🧠 Exercícios

**Iniciante**
1. Converta uma função JavaScript simples (ex: calculadora de desconto) para TypeScript, tipando parâmetros e retorno.
2. Crie uma `interface Produto` com id, nome, preço e categoria, e uma função que recebe um array de produtos e retorna o total.

**Intermediário**
3. Crie um union type `type Resultado<T> = { sucesso: true; dado: T } | { sucesso: false; erro: string }` e uma função que o utiliza com Type Narrowing.
4. Implemente uma função genérica `agruparPor<T, K extends keyof T>(lista: T[], chave: K)` que agrupa uma lista de objetos por uma propriedade.

**Avançado**
5. Modele, usando `interface` e union types, o estado de um pedido de e-commerce (`pendente`, `pago`, `enviado`, `cancelado`), com campos específicos de cada estado (ex: `enviado` tem `codigoRastreio`, `cancelado` tem `motivo`) — isso é o padrão profissional de "discriminated unions".
6. Refatore um objeto com `any` em múltiplos pontos (fornecido por você mesma, propositalmente mal tipado) para tipagem completa e correta.

**Desafio final**
7. Construa um pequeno sistema de tipos para uma API de tarefas com Generics, usando `Partial` para atualização parcial, `Omit` para criação (sem id) e `Pick` para uma versão resumida de listagem.

---

## 🌱 Projetos

**Projeto 1 — SDK tipado para uma API fictícia**
Construa um pequeno "cliente" (SDK) TypeScript para consumir uma API fictícia de pedidos, com tipos completos para requisição e resposta, tratamento de erro tipado (union type de sucesso/erro), e Generics para reutilizar a lógica de chamada HTTP entre diferentes endpoints. Isso simula exatamente o tipo de código que empresas escrevem para consumir APIs internas ou de terceiros.

---

## ✔️ Critério de conclusão

Você conclui a Fase 4 quando consegue tipar corretamente qualquer código JavaScript que você mesma escreveu nas fases anteriores, sem usar `any`, com modo `strict` ativado, e explicar as decisões de tipagem que tomou.

> **É isso que empresas realmente esperam de uma Junior?** Sim, integralmente, para vagas de backend/fullstack JS. Em 2026, "sei JavaScript mas não uso TypeScript" é considerado uma lacuna relevante na maioria dos processos seletivos sérios.

---

## 📄 Documentações

- **typescriptlang.org/docs** — a documentação oficial, extremamente bem escrita, com um handbook completo. É a referência primária desta fase.

---

`↩ Índice Geral: README.md` | `➡ Próximo: Capítulo 5 - modulo-05.md (HTML, CSS, Responsividade e Acessibilidade)`
