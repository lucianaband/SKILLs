---
name: phocus-spec
description: >
  Passo A do Phocus App Guide — transforma um PRD em uma Spec técnica de vibe coding, pronta para colar no Claude Code dentro do Antigravity. Use quando o usuário disser "/spec", "quero criar a spec do app", "transformar o PRD em spec", "preciso da spec antes de codar", "montar a spec do projeto", ou quando iniciar um novo app pela metodologia Phocus. Acione também quando o usuário fornecer um PRD e quiser começar a construção via vibe coding. Este skill conduz uma entrevista rápida e estruturada para preencher lacunas do PRD, depois gera um documento de spec otimizado para Claude Code — com visão clara, stack definido, modelo de dados, regras de negócio e o prompt inicial de vibe coding.
---

# /spec — Criadora de Spec Phocus

Você está executando o **Passo A** da Fase 2 do Phocus App Guide.

Seu papel aqui é transformar um PRD (documento de produto) em uma **Spec técnica de vibe coding** — o documento que vai guiar o Claude Code a construir o app corretamente, do início ao fim.

Uma boa spec não é longa. É **precisa**. O Claude Code precisa saber o que construir, em que ordem, com qual stack, e quais são as regras que não podem quebrar. Tudo que for vago vai gerar retrabalho.

---

## Passo 1 — Receba e leia o PRD

O usuário vai fornecer o PRD em uma das formas:
- Arquivo anexado (.md, .pdf, .docx)
- Texto colado diretamente
- Link para documento

Leia tudo antes de perguntar qualquer coisa. Muitas respostas já estão no PRD.

---

## Padrões Técnicos Obrigatórios Phocus

Estes padrões se aplicam a **todos os apps** da Phocus, sem exceção. Não pergunte sobre eles — apenas aplique.

### Banco de Dados — Prisma + Repository Pattern
- Use **Prisma ORM** para toda modelagem, migrations e queries
- Esconda o Prisma atrás do padrão **Repository**: a camada de domínio nunca importa Prisma diretamente
- Estrutura: `domain/` (lógica pura) → `repositories/` (acesso a dados via Prisma) → `controllers/` (HTTP)

### Autenticação e Autorização — Clerk
- Use **Clerk** para todo gerenciamento de usuários
- Valide sessões/tokens nas rotas de API via middleware, nunca inline em cada rota
- Gerencie permissões de forma centralizada (middleware de autorização por papel/permissão)

### APIs RESTful — Padrão Obrigatório
- Verbos HTTP corretos: `GET` (leitura), `POST` (criação), `PUT/PATCH` (atualização), `DELETE` (remoção)
- URLs orientadas a recursos: `/projetos/123/tarefas` — nunca verbos na URL como `/getTarefas`
- Status Codes semânticos obrigatórios:
  - `200 OK` — sucesso em leitura/atualização
  - `201 Created` — recurso criado com sucesso
  - `400 Bad Request` — erro de validação/dados inválidos
  - `401 Unauthorized` — não autenticado
  - `403 Forbidden` — autenticado mas sem permissão
  - `404 Not Found` — recurso não existe
  - `500 Internal Server Error` — erro inesperado no servidor

### Componentes de Interface Padrão — phocus-components

Todo app Phocus com área autenticada usa os mesmos componentes de base. **Não reinvente — reutilize.**

Se o app tiver **login**, inclua automaticamente na spec:
- **Sidebar + Layout** (`app/(dashboard)/layout.tsx`) — menu lateral escuro com logo branca, nav dinâmico por perfil, UserButton Clerk
- **Gestão de Usuários** (`app/(dashboard)/admin/usuarios/`) — para apps com múltiplos perfis de acesso

Se o app for **interno da agência** (campanhas, planejamento, relatórios, checklists), inclua também:
- **Gestão de Clientes** (`app/(dashboard)/clientes/`) — CRUD de carteira de clientes da agência

> O skill `/components` entrega o código pronto desses três módulos com API routes e schema Prisma incluídos. Mencione no Prompt Inicial de Vibe Coding que o Claude Code deve usar `/components` ao implementar esses módulos — não codá-los do zero.

---

## Passo 2 — Faça a Entrevista Rápida

Depois de ler o PRD, identifique o que está faltando e faça **apenas as perguntas necessárias** — não pergunte o que já está respondido. Agrupe em uma única mensagem.

Use este checklist para identificar lacunas:

**Stack técnica** — Next.js + Prisma/SQLite + Clerk já estão definidos. Pergunte apenas:
- Deploy: Vercel, Railway, sem deploy por enquanto?

**Escopo do MVP**
- Quais funcionalidades são obrigatórias para o V1 funcionar?
- O que pode ficar para V2?

**Usuários e acesso**
- Quem usa o app? Só a equipe interna, clientes externos, ou ambos?
- Existem perfis diferentes (admin, usuário comum, etc.)?

**Integrações externas**
- O app precisa se conectar a algum serviço de terceiro (email, WhatsApp, APIs, etc.)?

**Restrições de tempo**
- Existe deadline? Para quando precisa estar funcionando?

> Regra de ouro: se o PRD já responde algo, não pergunte. Se ficar em dúvida, faça uma suposição razoável e avise o usuário ao final.

---

## Passo 3 — Gere a Spec

Com o PRD lido e as lacunas preenchidas, gere a spec no formato abaixo. Seja denso e preciso — frases longas demais são sinal de imprecisão.

Use este template exato:

---

```markdown
# Spec — [Nome do App]

## Visão em 3 linhas
[O que é o app em 1 linha.]
[Para quem é e qual problema resolve em 1 linha.]
[O que torna este app diferente ou importante em 1 linha.]

## Stack Técnica
- **Frontend:** Next.js (App Router)
- **Backend:** Next.js API Routes
- **Banco de dados:** Prisma ORM + SQLite (dev) / PostgreSQL (prod) — acesso exclusivamente via Repository Pattern
- **Autenticação:** Clerk — validação de sessão via middleware em todas as rotas protegidas
- **API:** RESTful — verbos HTTP corretos, URLs orientadas a recurso, status codes semânticos
- **Linguagem:** TypeScript
- **Deploy:** [onde vai rodar — ex: Vercel]
- **Outras dependências críticas:** [libs, APIs, serviços externos]

## Modelo de Dados (Schema Resumido)
[Liste as tabelas/coleções principais e seus campos mais importantes.
Não precisa ser SQL completo — o Claude Code vai expandir isso.
Foque nas relações e nos campos que têm lógica de negócio.]

Exemplo:
- **projetos** (id, nome_cliente, nome_campanha, data_entrada, data_deadline, status, marca_id)
- **tarefas** (id, projeto_id, nome, sla_dias, dependencia_id, data_inicio, data_fim)

## Funcionalidades do MVP (V1)
[Liste as funcionalidades que DEVEM existir para o app ser utilizável.
Seja específico: não "CRUD de tarefas", mas "criar, editar, reordenar e excluir tarefas com dependência linear".
Priorize do mais crítico ao menos crítico.]

1. [Funcionalidade crítica 1]
2. [Funcionalidade crítica 2]
...

## O que NÃO está no V1
[Funcionalidades do PRD que ficam para depois — seja explícito para o Claude Code não implementar.]

- [Feature fora do escopo 1]
- [Feature fora do escopo 2]

## Regras de Negócio Críticas
[As lógicas que NÃO podem quebrar. Se o Claude Code errar aqui, o app não funciona.
Descreva cada regra em linguagem clara, não técnica.]

- **[Nome da regra]:** [Descrição do comportamento esperado]
- **[Nome da regra]:** [Descrição do comportamento esperado]

## Restrições de Segurança
[O que precisa ser protegido desde o início — não deixe para depois.]

- [Restrição de segurança 1]
- [Restrição de segurança 2]

## Identidade Visual
[Cores, fontes e marca a aplicar. Se for uma marca do Grupo Phocus, especifique qual.]

- **Marca:** [Phocus / Maximize / Faz Promo / outra]
- **Cor primária:** [hex]
- **Cor de sucesso:** [hex]
- **Cor de alerta:** [hex]
- **Fundo:** [hex]
- **Texto:** [hex]

## Ordem de Construção Recomendada
[Oriente o Claude Code sobre por onde começar — não pela UI, sempre pela lógica.]

1. Schema do banco e migrations
2. [Próxima camada lógica — ex: utility functions, API routes]
3. [Próxima camada]
4. Interface (só depois da lógica validada)

## Prompt Inicial de Vibe Coding
[O primeiro prompt que o usuário vai colar no Claude Code para iniciar a construção.
Deve ser direto, específico e conter as instruções críticas de ordem de execução.]

---
IA, vamos construir o **[Nome do App]**.

[2-3 linhas resumindo o app e seu propósito]

**Stack:**
- Frontend: [tecnologia]
- Backend: [tecnologia]
- Banco: Prisma ORM + [banco subjacente]
- Auth: Clerk

**Padrões obrigatórios (não negociáveis):**
- Banco de dados exclusivamente via **Prisma ORM**, escondido atrás do padrão **Repository**. A camada de domínio não importa Prisma diretamente.
- Autenticação via **Clerk** com validação de sessão em middleware centralizado — não inline por rota.
- APIs estritamente **RESTful**: verbos HTTP corretos, URLs orientadas a recurso (`/recursos/id/sub-recursos`), status codes semânticos (`200`, `201`, `400`, `401`, `403`, `404`, `500`).
- Sidebar, Clientes e Usuários: use o skill `/components` para obter o código padrão Phocus — não implemente do zero.

**Comece por:**
1. Schema Prisma + migrations (tabelas: [listar principais])
2. [Próxima camada — ex: utility functions críticas de negócio]
3. Repository layer + rotas de API
4. Interface (só após validarmos a lógica)

**Regras de negócio que não podem quebrar:**
- [Regra crítica 1]
- [Regra crítica 2]

Não avance para a interface antes de validarmos a lógica central.
---

## Definição de "Pronto" para o V1
[Como o usuário vai saber que o V1 está completo e funcionando?
Liste 3-5 critérios objetivos e verificáveis.]

- [ ] [Critério 1]
- [ ] [Critério 2]
- [ ] [Critério 3]
```

---

## Passo 4 — Entregue e Oriente

Depois de gerar a spec:

1. **Salve em dois lugares:**
   - `planning/spec-[nome-do-app].md