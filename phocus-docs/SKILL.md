---
name: phocus-docs
description: |
  Gera a documentação de setup do projeto Phocus: CLAUDE.md (raiz) e docs/WORKFLOW.md. Use quando o usuário disser "/docs", "gerar documentação do projeto", "criar CLAUDE.md", "criar WORKFLOW.md", "setup de docs", ou quando estiver configurando um novo projeto pela metodologia Phocus App Guide. Este skill é o Passo D do fluxo — executado após /architecture e antes de começar a codar. Garante que o Claude Code tenha as instruções certas para operar no projeto.
---

# Skill: Phocus Docs

## Propósito

Gerar dois arquivos essenciais para o projeto Phocus:

1. **`CLAUDE.md`** (na raiz do projeto) — instruções para o Claude Code operar corretamente no projeto
2. **`docs/WORKFLOW.md`** — documentação do fluxo de trabalho da equipe: como usar os skills, comandos e ciclo de desenvolvimento

---

## Stack Obrigatória Phocus

Estes valores são **fixos** — nunca pergunte sobre stack:

| Camada | Tecnologia |
|--------|-----------|
| **Frontend** | Next.js (App Router) |
| **Backend/API** | Next.js API Routes |
| **ORM** | Prisma |
| **Banco de Dados** | SQLite (dev) / PostgreSQL (prod) |
| **Autenticação** | Clerk |
| **Linguagem** | TypeScript |

---

## Workflow

### 1. Ler arquivos existentes

Antes de gerar, leia os arquivos do projeto para extrair contexto:
- `docs/SPEC.md` ou `planning/spec-[app].md` — nome, objetivo, stack, funcionalidades
- `docs/ARCHITECTURE.md` — domínios, estrutura de pastas
- `docs/DESIGN.md` — identidade visual (confirmar que existe)

### 2. Perguntar se faltar contexto

Se não encontrar spec ou architecture, pergunte apenas:

```
Para gerar o CLAUDE.md e WORKFLOW.md, preciso saber:

1. **Nome do projeto**: Como se chama o app?
2. **Objetivo**: O que o app faz? (1-2 linhas)
3. **Domínios principais**: Quais as entidades centrais? (ex: Projetos, Tarefas, Usuários)
```

### 3. Gerar `CLAUDE.md`

Criar na **raiz do projeto**. Este arquivo é lido automaticamente pelo Claude Code a cada sessão.

```markdown
# CLAUDE.md — [Nome do Projeto]

> Instruções para o Claude Code operar neste projeto.
> Leia este arquivo inteiro antes de qualquer ação.

## Protocolo obrigatório antes de qualquer /execute

Este protocolo existe porque o Claude Code abre sessões novas a cada conversa e perde contexto. Sem reorientação ativa, ele começa certo e desvia das regras da SPEC. Siga **sempre**, sem pular passos:

1. **Leia a issue inteira**: abra `issues/<arquivo>.md` e leia da primeira à última linha.
2. **Releia `docs/SPEC.md`**: vá direto à(s) seção(ões) que cobre(m) a feature da issue — regras de negócio, modelo de dados, perfis envolvidos.
3. **Releia `docs/ARCHITECTURE.md`**: confirme a camada da issue e a regra de fluxo (Controller → UseCase → Repository).
4. **Ecoe em 3 bullets, ANTES de escrever qualquer código**:
   - O que a issue pede (em 1 frase, sem copiar o título)
   - Qual regra de negócio da SPEC se aplica (cite o trecho)
   - Qual camada arquitetural toca e qual subagente é o correto
5. **Aguarde validação do usuário** se houver qualquer ambiguidade entre a issue e a SPEC. Não improvise.
6. **Só então implemente** — seguindo issue + SPEC + ARCHITECTURE, nessa ordem de autoridade.

Regra de ouro: **a SPEC vence o resumo deste CLAUDE.md**. Este arquivo é um briefing, não a fonte de verdade. Quando em dúvida, abra o `docs/SPEC.md`.

## Identidade do Projeto

- **Nome:** [Nome do App]
- **Objetivo:** [O que o app faz em 1-2 linhas]
- **Stack:** Next.js (App Router) + Prisma + SQLite + Clerk + TypeScript

## Documentação Obrigatória

Antes de implementar qualquer feature, leia:

- [`docs/SPEC.md`](docs/SPEC.md) — Especificação completa do produto
- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) — Arquitetura, camadas, padrões
- [`docs/DESIGN.md`](docs/DESIGN.md) — Identidade visual Phocus (cores, fontes, componentes)
- [`docs/WORKFLOW.md`](docs/WORKFLOW.md) — Fluxo de trabalho e comandos

## Arquitetura (Resumo Crítico)

### Fluxo de chamadas — NUNCA quebrar

```
HTTP Request → Middleware Clerk → Controller → UseCase → Service/Repository → Database → HTTP Response
```

### Regras inquebráveis

- ❌ Controller NUNCA chama Repository direto
- ❌ Domain/Service NUNCA importa Prisma, HTTP ou Clerk
- ❌ UseCase NUNCA importa Controller
- ✅ Autorização SEMPRE acontece no UseCase (não no Controller)
- ✅ Toda lógica de negócio fica no backend (Thin Client, Fat Server)

### Estrutura de pastas

```
/src
  /domain          # Entidades, interfaces, regras puras de negócio
  /application     # UseCases, DTOs
  /adapters        # Controllers (HTTP), Repositories (Prisma)
  /infrastructure  # Config, auth, database client
  /shared          # Utils, tipos globais
```

## Identidade Visual

- **Cor primária:** `#B0A2F9` (roxo)
- **Fundo:** `#F9F9F9`
- **Texto:** `#191818`
- **Fonte:** Montserrat ou Nunito (900 para títulos, 400 para corpo)
- Ver [`docs/DESIGN.md`](docs/DESIGN.md) para especificação completa

## Padrões de Desenvolvimento

### Migrations (CRÍTICO)

- **Nunca editar** uma migration já executada em produção
- Para alterar schema: criar nova migration com `prisma migrate dev --name descricao`
- Nomear: `YYYYMMDD_descricao_da_mudanca`

### Variáveis de Ambiente

- Credenciais SEMPRE em `.env.local` (nunca commit)
- `.env.example` SEMPRE atualizado com as chaves (sem valores)
- Validar variáveis no startup via `src/infrastructure/config/envConfig.ts`

### Issues e Execução

- Tasks ficam em `issues/XX-nome-da-task.md`
- Executar com `/execute issues/XX-nome.md`
- Fazer commit antes de cada `/execute`

### Nomenclatura

- Arquivos: `camelCase.ts` ou `PascalCase.ts` (classes)
- Classes: `UserRepository`, `CreateUserUseCase`, `UserService`
- Variáveis: `camelCase`
- Constantes: `UPPER_SNAKE_CASE`
- Enums: `PascalCase`

## Segurança — Red Flags

- ❌ Nunca colocar credenciais no código
- ❌ Nunca usar `NEXT_PUBLIC_*` para secrets
- ❌ Nunca aceitar dados sem validar no backend
- ❌ Nunca expor IDs sequenciais em URLs públicas
- ❌ Nunca retornar stack traces em respostas de erro
```

### 4. Gerar `docs/WORKFLOW.md`

Criar em `docs/WORKFLOW.md`. Este arquivo documenta como a equipe trabalha com o projeto usando a metodologia Phocus App Guide.

```markdown
# WORKFLOW.md — [Nome do Projeto]

> Como trabalhamos neste projeto usando a metodologia Phocus App Guide.

## Visão Geral do Fluxo

```
PRD → /spec → /break → /plan → /docs → /execute
```

| Passo | Skill/Comando | Output | Onde salva |
|-------|--------------|--------|-----------|
| A | `/spec` | Especificação técnica | `planning/spec-[app].md` + `docs/SPEC.md` |
| B | `/break` | Módulos de desenvolvimento | `planning/break-[app].md` |
| C | `/plan` | Tasks detalhadas + issues | `planning/plan-[app]-moduloN.md` + `issues/` |
| D | `/docs` | CLAUDE.md + WORKFLOW.md | raiz + `docs/` |
| E | `/architecture` | Arquitetura do projeto | `docs/ARCHITECTURE.md` |
| F | `/execute` | Implementação | código no projeto |

## Estrutura de Pastas do Projeto

```
[nome-do-projeto]/
├── CLAUDE.md                    # Instruções para o Claude Code
├── .env.local                   # Secrets (nunca commit)
├── .env.example                 # Template de variáveis (sempre commit)
├── docs/
│   ├── SPEC.md                  # Especificação completa
│   ├── ARCHITECTURE.md          # Arquitetura e padrões
│   ├── DESIGN.md                # Identidade visual Phocus
│   └── WORKFLOW.md              # Este arquivo
├── planning/
│   ├── spec-[app].md            # Spec gerada pelo /spec
│   ├── break-[app].md           # Módulos gerados pelo /break
│   └── plan-[app]-moduloN.md    # Plano de tasks do módulo N
├── issues/
│   ├── 01-nome-da-task.md       # Tasks geradas pelo /plan
│   ├── 02-outra-task.md
│   └── ...
└── src/                         # Código-fonte
```

## Como Executar uma Task

1. Abrir o Claude Code no terminal na pasta do projeto
2. Verificar se há issues pendentes: `ls issues/`
3. Fazer git checkpoint: `git add . && git commit -m "checkpoint antes da task XX"`
4. Rodar: `/execute issues/XX-nome-da-task.md`
5. Revisar o que foi feito antes de aprovar

## Modos de Execução no Claude Code

| Modo | Como ativar | Quando usar |
|------|------------|-------------|
| **Automático** | `/play` | Deixar o Claude executar sem parar |
| **Controlado** | `Shift+Tab → Edit` | Revisar cada passo antes de aprovar |

> **Recomendação:** Use o modo controlado para tasks de banco, autenticação e integrações externas.

## Convenções de Commit

```
feat: adiciona [funcionalidade]
fix: corrige [bug]
refactor: reorganiza [módulo]
docs: atualiza documentação
chore: configuração, deps, setup
```

## Migrations de Banco

**Regra crítica: nunca editar migrations já executadas em produção.**

Para qualquer alteração no schema:
```bash
npx prisma migrate dev --name descricao-da-mudanca
```

Nomeação: `YYYYMMDD_descricao` (ex: `20260416_add_status_to_projects`)

## Variáveis de Ambiente

1. Copiar `.env.example` para `.env.local`
2. Preencher os valores reais
3. Nunca commitar `.env.local`
4. Ao adicionar nova variável: atualizar `.env.example` com a chave (sem valor)

## Subagentes Disponíveis no /execute

O `/execute` aciona automaticamente o subagente correto por tipo de task:

| Tipo de task | Subagente |
|-------------|-----------|
| Componente React/UI | `component-writer` |
| API Route / Controller | `route-writer` |
| Schema Prisma / Model | `model-writer` |
| Integração externa (API, email) | `integration-writer` |
| Testes | `test-writer` |

## Contato e Documentação

- Metodologia completa: Phocus App Guide
- Stack de referência: Next.js + Prisma + SQLite + Clerk + TypeScript
- Dúvidas de padrões: consultar `docs/ARCHITECTURE.md`
```

---

## Confirmação ao entregar

Após criar os arquivos, informe ao usuário:

- ✅ `CLAUDE.md` criado na raiz do projeto
- ✅ `docs/WORKFLOW.md` criado
- 🔜 Próximo passo: começar a codar com `/execute issues/01-nome-da-task.md`
- 💡 Lembre de fazer `git init` e o primeiro commit antes do primeiro `/execute`
