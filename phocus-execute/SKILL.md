---
name: phocus-execute
description: |
  Passo F do Phocus App Guide — executa uma issue específica da pasta issues/ usando o subagente correto. Use quando o usuário disser "/execute", "executar issue", "implementar a task", "rodar o modulo", "execute issues/XX", ou quando estiver pronto para codar após o /plan. Este skill lê o arquivo de issue, identifica o tipo de task, aciona o subagente especializado correto (component-writer, route-writer, model-writer, integration-writer, test-writer) e garante que a implementação siga Clean Architecture + stack Phocus. Sempre valida git checkpoint antes de executar e instrui sobre como revisar o resultado.
---

# /execute — Executor de Issues

Você está executando o **Passo F** da Fase 2 do Phocus App Guide.

Seu papel é pegar um arquivo de issue da pasta `issues/` e implementar o que está descrito, usando o subagente especializado correto, seguindo rigorosamente a stack e os padrões arquiteturais Phocus.

---

## Stack Obrigatória Phocus

Fixo em todos os projetos — nunca questione ou altere:

| Camada | Tecnologia |
|--------|-----------|
| **Frontend** | Next.js (App Router) + TypeScript |
| **Backend/API** | Next.js API Routes |
| **ORM** | Prisma — acesso SEMPRE via Repository Pattern |
| **Banco** | SQLite (dev) / PostgreSQL (prod) |
| **Autenticação** | Clerk — middleware centralizado |
| **Arquitetura** | Clean Architecture: Domain → Application → Adapters → Infrastructure |

---

## Fluxo de Chamadas — NUNCA quebrar

```
HTTP Request
    ↓
Middleware Clerk (valida sessão)
    ↓
Controller (traduz HTTP → DTO)
    ↓
UseCase (orquestra + autoriza)
    ↓
Service/Repository (lógica + dados)
    ↓
Database (Prisma)
    ↓
HTTP Response
```

**Regras absolutas:**
- ❌ Controller NUNCA chama Repository diretamente
- ❌ Domain/Service NUNCA importa Prisma, HTTP ou Clerk
- ✅ Autorização SEMPRE no UseCase (nunca só no Controller)
- ✅ Todo dado do frontend é validado no backend

---

## Passo 1 — Receba a issue

O usuário vai fornecer o arquivo de issue de uma das formas:
- `/execute issues/01-nome-da-task.md`
- Conteúdo colado diretamente
- Nome da issue sem o caminho

**Leia o arquivo completo antes de agir.** Identifique:
- O que construir
- Arquivos a criar ou modificar
- Critérios de aceite
- Subagente recomendado
- Alertas de risco

---

## Passo 2 — Verificar pré-condições

Antes de implementar, confirme:

```
✅ Checklist pré-execução:
- [ ] git status está limpo (ou usuário fez git commit antes)
- [ ] A issue anterior foi validada (se houver dependência)
- [ ] CLAUDE.md existe na raiz do projeto
- [ ] docs/ARCHITECTURE.md existe e foi lido
```

Se `CLAUDE.md` não existir: avise o usuário e sugira rodar `/docs` primeiro.

Se houver mudanças não commitadas: **avise** e recomende fazer checkpoint:
```bash
git add . && git commit -m "checkpoint antes de executar issue XX"
```

---

## Passo 3 — Identificar o subagente correto

Selecione o subagente com base no tipo de task descrito na issue:

| Tipo de task | Subagente | Quando usar |
|-------------|-----------|-------------|
| Componente React, página, layout, UI | `component-writer` | Qualquer arquivo em `/app`, `/components`, `/pages` |
| API Route, Controller, middleware | `route-writer` | Arquivos em `/app/api/`, rotas Next.js |
| Schema Prisma, Model, migration, seed | `model-writer` | `schema.prisma`, migration files, seed scripts |
| Integração externa (email, PDF, API terceiro) | `integration-writer` | Serviços externos: Resend, Puppeteer, APIs |
| Testes unitários ou de integração | `test-writer` | Arquivos `*.test.ts`, `*.spec.ts` |

Se a issue envolver múltiplos tipos (ex: criar model + API), divida em sub-etapas e comece pelo model.

---

## Passo 4 — Executar a implementação

### Para cada arquivo da issue, siga esta ordem:

**1. Model/Schema** (se a issue envolve banco):
- Editar `prisma/schema.prisma` com as novas entidades
- Nunca editar migrations já executadas em produção
- Criar nova migration: `npx prisma migrate dev --name nome-da-mudanca`
- Atualizar seed se necessário

**2. Domain** (`/src/domain/[feature]/`):
- Criar entidades com validações puras
- Criar interface do Repository (`IFeatureRepository.ts`)
- Criar Domain Service se necessário (lógica pura, sem efeitos colaterais)

**3. Application** (`/src/application/[feature]/`):
- Criar UseCase com:
  1. Validação de entrada (DTO)
  2. Autorização via `permissionValidator.can()`
  3. Chamada ao Repository
  4. Retorno de DTO
- Criar DTOs de input e output

**4. Adapters/Repository** (`/src/adapters/database/`):
- Implementar a interface `IFeatureRepository`
- Usar Prisma internamente
- Retornar entidades de domínio (não Prisma models diretos)

**5. Adapters/Controller** (`/src/adapters/http/` ou `/app/api/`):
- Extrair user do token Clerk
- Chamar UseCase
- Mapear exceções → HTTP status codes
- Nunca conter lógica de negócio

**6. Interface** (`/app/` ou `/components/`):
- Componente ou página que consome a API
- Apenas exibe dados, não contém lógica de negócio
- Chamar backend via fetch/API route

---

## Passo 5 — Padrões de código obrigatórios

### Tratamento de erros em Controller (obrigatório)

```typescript
try {
  const result = await createFeatureUseCase.execute(input)
  return NextResponse.json(result, { status: 201 })
} catch (error) {
  if (error instanceof ValidationException) {
    return NextResponse.json({ error: error.message }, { status: 400 })
  } else if (error instanceof UnauthorizedException) {
    return NextResponse.json({ error: 'Token inválido' }, { status: 401 })
  } else if (error instanceof ForbiddenException) {
    return NextResponse.json({ error: 'Sem permissão' }, { status: 403 })
  } else if (error instanceof NotFoundException) {
    return NextResponse.json({ error: 'Não encontrado' }, { status: 404 })
  } else {
    return NextResponse.json({ error: 'Erro interno' }, { status: 500 })
  }
}
```

### Autenticação Clerk no Controller (obrigatório)

```typescript
import { auth } from '@clerk/nextjs/server'

export async function POST(request: Request) {
  const { userId } = auth()
  if (!userId) {
    return NextResponse.json({ error: 'Não autorizado' }, { status: 401 })
  }
  // ... chamar UseCase
}
```

### Repository Pattern (obrigatório)

```typescript
// Interface no Domain (sem importar Prisma)
export interface IProjectRepository {
  create(project: Project): Promise<Project>
  findById(id: string): Promise<Project | null>
}

// Implementação nos Adapters (usa Prisma)
export class ProjectRepository implements IProjectRepository {
  async create(project: Project): Promise<Project> {
    const result = await prisma.project.create({ data: project })
    return result
  }
}
```

### Variáveis de ambiente (obrigatório)

```typescript
// ✅ Correto
const apiKey = process.env.OPENAI_API_KEY

// ❌ Proibido
const apiKey = 'sk-abc123...'
```

---

## Passo 6 — Validar critérios de aceite

Após implementar, verificar cada item do checklist da issue:

```
## Validação pós-execução:
- [ ] Todos os critérios de aceite da issue estão satisfeitos?
- [ ] Nenhuma regra de arquitetura foi violada?
- [ ] Erros são tratados e retornam HTTP status correto?
- [ ] Variáveis sensíveis estão em .env (não hardcoded)?
- [ ] A camada Domain não importa Prisma, HTTP ou Clerk?
```

Se algum critério falhar: **não marque como concluído**. Corrija antes de avançar.

---

## Passo 7 — Confirmar e orientar

Após validar:

```
✅ Issue [XX] — [Nome da Task] implementada

Arquivos criados/modificados:
  - [lista dos arquivos]

Critérios de aceite:
  - ✅ [Critério 1]
  - ✅ [Critério 2]

Próximos passos:
  git add . && git commit -m "feat: [descrição do que foi feito]"
  /execute issues/[XX+1]-proxima-task.md
```

---

## Modos de execução

| Modo | Como ativar | Quando usar |
|------|------------|-------------|
| **Automático (/play)** | Usuário diz `/play` | Tasks simples, Claude executa sem pausar |
| **Controlado** | `Shift+Tab → Edit` no Claude Code | Tasks de banco, auth, integrações — revisar cada passo |

> **Recomendação:** use o modo controlado para qualquer issue que envolva migrations, autenticação ou integração externa. Uma migration executada errada não tem desfazer simples.

---

## Red Flags — Parar e alertar o usuário

Se durante a implementação você encontrar qualquer um destes, **pare** e avise antes de continuar:

- ❌ A issue pede para editar uma migration já existente
- ❌ A issue pede para colocar credencial no código
- ❌ A issue pede para validar autenticação no frontend
- ❌ A issue criaria dependência circular entre camadas
- ❌ A issue pede para importar Prisma diretamente na camada de domínio
- ❌ A pasta `issues/` está vazia mas o usuário pediu `/execute` sem especificar arquivo

---

## Princípios que guiam este skill

**Leia a issue inteira antes de codar.** Metade dos erros vêm de começar sem ler os alertas e critérios de aceite.

**Um arquivo de cada vez.** Não crie tudo ao mesmo tempo. Siga a ordem: schema → domain → application → adapters → interface.

**Critério de aceite é lei.** A issue define o que "pronto" significa. Não decida por conta própria que está bom.

**Git antes de executar.** Commit é o botão de desfazer do vibe coding. Sem ele, um erro de migration pode exigir recriar o banco.
