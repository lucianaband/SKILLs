---
name: phocus-app-guide
description: >
  Guia completo e obrigatório para construir qualquer app da Phocus usando vibe coding. Contém o fluxo de 7 passos (/spec → /break → /plan → /architecture → brand-phocus → /docs → /execute), as regras que nunca quebram, a estrutura de pastas padrão e os erros mais comuns. Use este skill sempre que o usuário quiser iniciar um novo app, quiser entender a metodologia Phocus, perguntar "por onde começo", "qual é o fluxo", "como construir um app Phocus", "quero criar um app", "metodologia Phocus", "vibe coding Phocus", ou antes de rodar qualquer um dos passos individuais (/spec, /break, /plan, /architecture, /docs, /execute). Este skill é o ponto de entrada de todo projeto.
---

# Phocus App Guide

## O que é vibe coding (e por que a Phocus usa)

Vibe coding é construir software usando IA como desenvolvedor principal. Você dá as instruções, a IA escreve o código. Mas — e esse é o ponto crítico — **IA sem estrutura produz caos**. Começa bem, perde o fio depois de 30% do trabalho, e você termina com um app quebrado que ninguém consegue manter.

A metodologia Phocus resolve isso. Cada passo tem um propósito. Cada arquivo gerado serve um papel claro. A IA trabalha dentro de uma estrutura que você controla.

---

## A lógica geral: dois momentos distintos

O trabalho se divide em dois momentos que não devem se misturar:

**Momento 1 — Planejar** (antes de escrever uma linha de código)
Você transforma o PRD em documentos que a IA vai usar como referência durante todo o desenvolvimento. Se esse momento for mal feito, o código vai mal.

**Momento 2 — Executar** (com a fundação pronta)
A IA implementa uma task de cada vez, seguindo os documentos que você criou. Sem improvisar. Sem pular etapas.

---

## O fluxo completo

```
PRD
 │
 ▼
/spec ──────────── Transforma o PRD em especificação técnica
 │
 ▼
/break ─────────── Divide a spec em módulos de desenvolvimento
 │
 ▼
/plan ──────────── Detalha as tasks de cada módulo + cria issues
 │
 ▼
/architecture ──── Documenta a arquitetura e os padrões do projeto
 │
 ▼
brand-phocus ───── Aplica a identidade visual (gera docs/DESIGN.md)
 │
 ▼
/docs ──────────── Gera CLAUDE.md + WORKFLOW.md (instruções para a IA)
 │
 ▼
/execute ────────── Implementa uma issue por vez
```

---

## Passo a passo

### /spec — Especificação técnica

**O que faz:** Lê o PRD e transforma em uma spec técnica precisa — o documento que define o que construir, em que ordem, com qual stack, e quais regras não podem quebrar.

**Por que importa:** O Claude Code trabalha mal com ambiguidade. Uma spec vaga gera código errado. Uma spec precisa gera código correto.

**O que você precisa ter:** O PRD do app (pode ser um .md, .pdf ou texto colado).

**O que é gerado:**
- `planning/spec-[app].md` — arquivo de trabalho
- `docs/SPEC.md` — referência permanente do projeto

**Stack já definida (não precisa decidir):**
- Frontend: Next.js com App Router
- Backend: Next.js API Routes
- Banco de dados: Prisma ORM + SQLite
- Autenticação: Clerk
- Linguagem: TypeScript

**Como usar:** No Claude Code, rode `/spec` e forneça o PRD. O skill vai fazer perguntas rápidas para preencher lacunas, depois gera a spec completa.

---

### /break — Quebra em módulos

**O que faz:** Divide a spec em módulos de desenvolvimento — unidades de trabalho que o Claude Code consegue executar em uma sessão sem se perder.

**Por que importa:** Jogar a spec inteira de uma vez não funciona. A IA começa bem e perde o fio. Módulos pequenos com escopo claro produzem resultado consistente.

**Regra de ouro:** Módulos sempre seguem esta ordem de camadas — nunca pule:

1. Schema do banco e migrations (fundação)
2. Lógica de negócio (cálculos, regras, validações)
3. Acesso a dados — Repository
4. APIs — rotas e autenticação
5. Interface — componentes e páginas
6. Integrações externas (email, PDF, etc.)

**O que é gerado:**
- `planning/break-[app].md` — lista de módulos com escopo, dependências e prompt de entrada para cada um

**Como usar:** Rode `/break` e forneça a spec. Você recebe os módulos em ordem com o prompt pronto para cada sessão.

---

### /plan — Plano de tasks + issues

**O que faz:** Pega um módulo específico do /break e detalha as tasks técnicas — na ordem certa, com critério de aceite claro e alertas de risco.

**Por que importa:** Módulos complexos precisam de granularidade maior. Uma task mal descrita é uma task mal executada.

**É obrigatório?** Não para módulos simples. Use o /plan quando o módulo tiver mais de 3 componentes interdependentes ou envolver lógica crítica de negócio.

**O que é gerado:**
- `planning/plan-[app]-moduloN.md` — plano detalhado do módulo
- `issues/01-nome-da-task.md`, `issues/02-...` — um arquivo por task, prontos para o /execute

**Como usar:** Rode `/plan` informando qual módulo quer detalhar. O skill cria o plano e os arquivos de issue automaticamente.

---

### /architecture — Arquitetura do projeto

**O que faz:** Documenta como o projeto é organizado em camadas, quais são as regras de importação entre elas, como tratar erros, como proteger secrets, e os padrões de nomenclatura.

**Por que importa:** Sem arquitetura documentada, a IA improvisa — e improvisos em estrutura de código geram dívida técnica que trava o projeto meses depois.

**O que é gerado:**
- `docs/ARCHITECTURE.md`

**A lógica das camadas (Clean Architecture):**

```
Domain         → Regras de negócio puras. Nunca importa Prisma, HTTP ou Clerk.
Application    → Casos de uso. Orquestra Domain + Repository.
Adapters       → Controllers (HTTP) e Repositories (banco). Traduzem entre camadas.
Infrastructure → Configurações, conexões, frameworks.
```

**Regra mais importante:** O fluxo de uma requisição é sempre:

```
Requisição HTTP → Middleware Clerk → Controller → UseCase → Repository → Banco
```

Nunca pule uma camada. Nunca inverta a ordem.

**Como usar:** Rode `/architecture`. O skill faz perguntas simples (nome do projeto, domínios principais) e gera o documento completo.

---

### brand-phocus — Identidade visual

**O que faz:** Gera o `docs/DESIGN.md` com todas as especificações visuais da Phocus Propaganda — cores, tipografia, componentes, espaçamento, tom de voz da interface.

**Por que importa:** O Claude Code precisa saber como a interface deve parecer antes de construí-la. O DESIGN.md é a referência que garante consistência visual em todo o projeto.

**O que é gerado:**
- `docs/DESIGN.md`

**Identidade visual da Phocus:**

| Elemento | Valor |
|---|---|
| Cor primária | `#B0A2F9` (roxo) |
| Cor de sucesso | `#45B577` (verde) |
| Cor de alerta | `#EE7D00` (laranja) |
| Fundo geral | `#F9F9F9` |
| Texto principal | `#191818` |
| Fonte | Montserrat ou Nunito (900 para títulos, 400 para corpo) |

**Como usar:** No Cowork, acione o skill `brand-phocus` e peça para gerar o `docs/DESIGN.md`. Ele cria o arquivo com toda a especificação.

---

### /docs — Instruções para a IA

**O que faz:** Gera dois arquivos que o Claude Code lê automaticamente a cada sessão de desenvolvimento — o `CLAUDE.md` e o `docs/WORKFLOW.md`.

**Por que importa:** O Claude Code não tem memória entre sessões. O `CLAUDE.md` é o briefing que garante que a IA sempre sabe o que é o projeto, quais são os padrões, o que não pode fazer e onde estão os documentos de referência.

**O que é gerado:**
- `CLAUDE.md` (na raiz do projeto) — instruções completas para a IA
- `docs/WORKFLOW.md` — documentação do fluxo de trabalho da equipe

**Pré-requisito:** Rode `/architecture` e `brand-phocus` antes do `/docs` — o `CLAUDE.md` referencia os arquivos que eles geram.

**Como usar:** Rode `/docs`. O skill lê os documentos que já existem no projeto e gera os dois arquivos automaticamente.

---

### /execute — Implementação

**O que faz:** Lê um arquivo de issue (`issues/XX-nome.md`) e implementa o que está descrito, usando o subagente especializado correto.

**Por que importa:** É aqui que o código é escrito de verdade. A estrutura toda que você construiu nos passos anteriores serve para que este passo seja preciso e sem surpresas.

**Subagentes disponíveis** (o /execute escolhe automaticamente):

| Tipo de task | Subagente |
|---|---|
| Componente React, página, layout | `component-writer` |
| API Route, Controller, middleware | `route-writer` |
| Schema Prisma, migration, seed | `model-writer` |
| Integração externa (email, PDF, API) | `integration-writer` |
| Testes | `test-writer` |

**Ritual obrigatório antes de cada /execute:**

```bash
git add . && git commit -m "checkpoint antes da issue XX"
```

Sem isso, um erro de migration ou de lógica pode exigir recriar o banco do zero.

**Modos de execução:**
- **Automático (/play):** A IA executa sem parar. Use para tasks simples e bem definidas.
- **Controlado (Shift+Tab → Edit):** Você revisa cada passo antes de aprovar. Use para banco, autenticação e integrações externas.

**Como usar:** No Claude Code, rode `/execute issues/01-nome-da-task.md`. Valide o checklist de aceite antes de avançar para a próxima issue.

---

## A estrutura de pastas de todo projeto Phocus

```
nome-do-projeto/
├── CLAUDE.md                    → Instruções para o Claude Code (gerado por /docs)
├── .env.local                   → Secrets — NUNCA commitar
├── .env.example                 → Template de variáveis — SEMPRE commitar
│
├── docs/
│   ├── SPEC.md                  → Especificação do produto (gerada por /spec)
│   ├── ARCHITECTURE.md          → Arquitetura e padrões (gerada por /architecture)
│   ├── DESIGN.md                → Identidade visual (gerada por brand-phocus)
│   └── WORKFLOW.md              → Fluxo de trabalho (gerado por /docs)
│
├── planning/
│   ├── spec-[app].md            → Arquivo de trabalho do /spec
│   ├── break-[app].md           → Módulos gerados pelo /break
│   └── plan-[app]-moduloN.md    → Plano de tasks de cada módulo
│
├── issues/
│   ├── 01-nome-da-task.md       → Tasks prontas para o /execute
│   ├── 02-outra-task.md
│   └── ...
│
└── src/                         → Código-fonte
    ├── domain/                  → Regras de negócio puras
    ├── application/             → Casos de uso
    ├── adapters/                → Controllers e Repositories
    ├── infrastructure/          → Config, banco, auth
    └── shared/                  → Utilitários globais
```

---

## Regras que nunca quebram

### Arquitetura
- Controller nunca chama Repository diretamente — passa sempre pelo UseCase
- Domain nunca importa Prisma, HTTP ou Clerk
- Autorização sempre acontece no UseCase, não no Controller
- Toda lógica de negócio fica no backend (o frontend só exibe dados)

### Banco de dados
- Nunca edite uma migration que já foi executada em produção
- Para qualquer mudança no schema, crie uma nova migration: `npx prisma migrate dev --name descricao`
- Nomear: `YYYYMMDD_descricao` (ex: `20260416_add_status_to_projects`)

### Segurança
- Credenciais ficam em `.env.local` — nunca no código, nunca no git
- `.env.example` sempre atualizado com as chaves (sem valores)
- Variáveis sensíveis nunca usam o prefixo `NEXT_PUBLIC_` (esse prefixo é público)
- Todo dado vindo do frontend é validado novamente no backend

### Commits
- Commitar antes de cada `/execute`
- Usar mensagens descritivas: `feat: adiciona listagem de projetos`, `fix: corrige cálculo de deadline`

---

## Erros mais comuns (e como evitar)

**Pular o /plan em módulos complexos**
O Claude Code vai "adivinhar" o comportamento e vai adivinhar errado. Se o módulo tem lógica com casos de borda, planeje antes.

**Ir direto para a interface antes de validar a lógica**
Interface construída em cima de lógica quebrada é retrabalho garantido. A ordem das camadas existe por isso.

**Não fazer o commit antes do /execute**
Um erro de migration sem checkpoint pode exigir recriar o banco. Não tem desfazer.

**Fornecer uma spec vaga para o /break**
Se a spec não define claramente o que está dentro e fora do V1, o /break vai criar módulos com escopo impreciso — e o /execute vai implementar coisas que você não pediu.

**Editar o `CLAUDE.md` manualmente para "ajustar" a IA**
O `CLAUDE.md` é gerado pelo `/docs` a partir dos documentos do projeto. Se algo está errado, corrija o documento de origem (ARCHITECTURE.md, SPEC.md), não o CLAUDE.md diretamente.

---

## Resumo em uma linha por passo

| Skill | Gera | Quando usar |
|---|---|---|
| `/spec` | `planning/spec-[app].md` + `docs/SPEC.md` | Ao receber o PRD |
| `/break` | `planning/break-[app].md` | Depois da spec aprovada |
| `/plan` | `planning/plan-[app]-moduloN.md` + `issues/` | Para módulos complexos |
| `/architecture` | `docs/ARCHITECTURE.md` | Antes de codar |
| `brand-phocus` | `docs/DESIGN.md` | Antes de codar |
| `/docs` | `CLAUDE.md` + `docs/WORKFLOW.md` | Depois de architecture e design |
| `/execute` | Código | A cada issue, do começo ao fim |
