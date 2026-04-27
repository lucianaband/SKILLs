---
name: phocus-architecture
description: |
  Cria documentação arquitetural em docs/ARCHITECTURE.md seguindo Clean Architecture, Domain Driven Design, e a stack obrigatória Phocus (Next.js + Prisma/SQLite + Clerk). Use sempre que o usuário quiser estruturar ou documentar a arquitetura de um projeto — mencione "/architecture", "criar architecture", "documentar a estrutura do projeto", "como organizar as pastas", "padrões arquiteturais", "injeção de dependências", ou quando precisar de um ARCHITECTURE.md completo que cubra camadas, segurança, autenticação, migrações e padrões de nomenclatura.
---

# Skill: Phocus Architecture

## Propósito

Gerar o arquivo **`docs/ARCHITECTURE.md`** completo e estruturado para um projeto Phocus, seguindo princípios sólidos de Clean Architecture, Domain Driven Design, Separation of Concerns (SoC), Injeção de Dependências (DI) e Design Patterns.

## Stack Obrigatória Phocus

Estes valores são **fixos em todos os projetos** — nunca pergunte sobre stack, já está definida:

| Camada | Tecnologia |
|--------|-----------|
| **Frontend** | Next.js (App Router) |
| **Backend/API** | Next.js API Routes |
| **ORM** | Prisma |
| **Banco de Dados** | SQLite (dev) / PostgreSQL (prod) |
| **Autenticação** | Clerk |
| **Linguagem** | TypeScript |

## Quando usar

- Você está iniciando um projeto e precisa documentar sua estrutura arquitetural
- Precisa definir como as pastas, camadas e responsabilidades serão organizadas
- Quer estabelecer regras claras de segurança, autenticação e acesso ao banco
- Precisa de um guia para a equipe sobre padrões de nomenclatura e estrutura

---

## Padrões Fundamentais para IA (OBRIGATÓRIO na Skill)

Antes de gerar o ARCHITECTURE.md, a IA deve entender estes padrões:

### Fluxo de Chamadas entre Camadas (NUNCA quebrar)

```
HTTP Request
    ↓
Middleware (validar token Clerk)
    ↓
Controller (traduzir HTTP → DTO)
    ↓
UseCase (orquestração + autorização)
    ↓
Service (lógica de domínio PURA) + Repository (acesso a dados)
    ↓
Database (Prisma executa)
    ↓
HTTP Response (serializar via Presenter)
```

**Regras INQUEBRÁVEIS:**
- ❌ Controller NUNCA chama Repository direto (pula UseCase)
- ❌ UseCase NUNCA chama Controller (ciclo infinito)
- ❌ Domain/Service NUNCA importa Prisma/HTTP/Clerk (violação)
- ❌ Repository NUNCA chama UseCase (inverso)

### Camadas: O que cada uma faz

| Camada | O que faz | Importa | Nunca importa |
|--------|-----------|---------|--------------|
| **Domain** | Lógica PURA, validações, regras de negócio | interfaces, tipos | Prisma, HTTP, Clerk, frameworks |
| **Application (UseCase)** | Orquestra casos de uso, validação de autorização | Domain, interfaces | Prisma direto, Controllers |
| **Adapters (Controller)** | Traduz HTTP ↔ Domain | UseCase, DTO | Lógica de negócio |
| **Adapters (Repository)** | Implementa interfaces com Prisma | Database, Prisma | Regras de negócio (lógica pura) |
| **Infrastructure** | Config, conexões, frameworks | Prisma, Clerk, env vars | Lógica de aplicação |

### Padrão de Autorização (sempre no UseCase)

```
UseCase.execute(user, input):
  1. Chamar permissionValidator.canDoThis(user, resource)
  2. Se false, throw ForbiddenException
  3. Se true, prosseguir com lógica
  4. Persistir no Repository
  5. Retornar DTO
```

**NUNCA** deixar autorização apenas no Controller — pode ser burlada.

### Padrão: UseCase vs Service vs Domain Service

| Padrão | Quando usar | Exemplo |
|--------|-------------|---------|
| **UseCase** | Orquestra um caso de uso específico (criar projeto) | `CreateProjectUseCase { permissionValidator.can...; repository.create() }` |
| **Service** | Lógica de domínio reutilizável em múltiplos UseCases | `ProjectService { validateProjectName(); calculateDeadline() }` |
| **Domain Service** | Lógica PURA sem efeitos colaterais (math, validação) | `calculateWorkingDays(start, end, holidays)` retorna número |

---

## Workflow

### 1. Extrair contexto existente

Se o usuário forneceu arquivos (PRD, Spec, Break, Plan, etc.), **leia-os primeiro** para entender:
- Tech stack (Frontend, Backend, Banco, Auth)
- Principais funcionalidades
- Modelos de dados
- Fluxos críticos
- Dependências externas

### 2. Fazer perguntas de clarificação

A stack já é definida (Next.js + Prisma/SQLite + Clerk + TypeScript). Só pergunte se faltar:

```
## Para completar o ARCHITECTURE.md, preciso saber:

1. **Nome do projeto**: Como se chama o app?
2. **Contexto do negócio**: O que o app faz? (2 linhas)
3. **Domínios principais**: Quais as entidades/funcionalidades centrais? (ex: Projetos, Usuários, Tarefas)
4. **Integrações externas**: Há APIs externas, serviços de email, pagamento, etc.?
```

### 3. Gerar `docs/ARCHITECTURE.md`

Salvar em `docs/ARCHITECTURE.md` dentro do projeto. Se a pasta `docs/` não existir, criá-la.

Estruturar o documento assim:

```markdown
# ARCHITECTURE — [Nome do Projeto]
> Como o projeto é organizado

## 1. Visão Geral
- Resumo do projeto em 2-3 linhas
- Tech stack
- Princípios arquiteturais aplicados

## 2. Princípios Arquiteturais e Boas Práticas
- **Arquitetura Limpa (Clean Architecture)**: Estruture a aplicação em camadas bem definidas (ex: *Domain, Application/Use Cases, Interface Adapters, Infrastructure*). As regras de negócio devem ser o centro do sistema e não devem depender de frameworks externos ou do banco de dados.
- **Separação de Conceitos (SoC)**: Cada classe, função ou módulo deve ter uma única responsabilidade. O código de roteamento não deve conter regras de negócio, e as regras de negócio não devem conhecer detalhes do protocolo HTTP.
- **Injeção de Dependências (DI)**: Desacople os componentes do sistema. Nunca instancie classes de infraestrutura diretamente dentro de casos de uso; em vez disso, injete-as (preferencialmente via construtor), utilizando interfaces.
- **Design Patterns**: Aplique padrões de projeto (como *Repository*, *Factory*, *Strategy*, *Singleton*, etc.) apenas quando eles fizerem sentido e resolverem problemas reais de acoplamento ou criação, evitando *overengineering* (complexidade desnecessária).
- **Domain Driven**: Organize o código em torno do domínio do problema, utilizando entidades, agregados, repositórios e serviços de domínio para modelar as regras de negócio de forma clara e expressiva. Não por responsabilidade técnica.

## 3. Estrutura de Pastas (por Comportamento)
```
/src
  /domain                    # Regras de negócio, entidades, agregados
    /[feature-name]
      - Entity.ts
      - Repository.interface.ts
      - Service.ts
      - types.ts
  
  /application               # Casos de uso, orquestração
    /[feature-name]
      - UseCase.ts
      - DTO.ts
  
  /adapters                  # Controllers, Presenters
    /http                    # API Routes, Controllers
    /database                # Implementações de Repository
    /external                # Integrações externas
  
  /infrastructure            # Frameworks, bibliotecas
    /config
    /database
    /auth
  
  /shared                    # Utilitários, helpers
    /constants
    /utils
    /types
```

## 4. Regras de Segurança e Autenticação

### 4.1 Arquitetura: Thin Client, Fat Server
- **Frontend (Thin Client)**: apenas exibe dados, nunca contém lógica de segurança ou validação crítica
- **Backend (Fat Server)**: toda regra de negócio, validação e segurança acontecem aqui
- Nunca confie no frontend — qualquer pessoa vê o código com 2 cliques no DevTools

### 4.2 O que NUNCA colocar no código (ou no Frontend)
- ❌ Chaves de API (Google Maps, OpenAI, Meta Ads, etc.)
- ❌ Senhas de banco de dados
- ❌ Regras de negócio (ex: "só clientes pagantes veem X")
- ❌ Tokens de acesso de qualquer serviço
- ❌ Dados pessoais ou sensíveis de clientes
- ❌ IDs internos expostos em URLs públicas

### 4.3 Variáveis de Ambiente (.env)
- Todas as credenciais e secrets ficam em `.env`
- `.env` sempre está em `.gitignore` — nunca commit secrets no repo
- Código usa `process.env.VARIAVEL_NAME`, nunca o valor direto
- Exemplo: `const apiKey = process.env.OPENAI_API_KEY`

### 4.4 Autenticação e Autorização
- Qual biblioteca de auth usar (Clerk, Auth0, JWT custom, etc.) e por que foi escolhida
- Onde fica middleware de autenticação (centralizado, nunca por rota)
- Como rotas são protegidas (validação de sessão no servidor)
- Regras de autorização (admin, user, guest, etc.)
- Geração de tokens, expiração, refresh — tudo no backend

## 5. Padrões de Nomenclatura
- Arquivos: `camelCase.ts` ou `PascalCase.ts`?
- Classes: `UserRepository`, `UserService`, `CreateUserUseCase`
- Variáveis: `camelCase`
- Constantes: `UPPER_SNAKE_CASE`
- Tipos/Interfaces: `PascalCase`, prefixar com `I` se interface?

## 6. Banco de Dados e Migrações
- Regras de migrations do banco (como são criadas, versionadas e executadas)
- ORM (Prisma, TypeORM, etc.)
- Convenção de nomes para migrations
- Como seed é executado
- Rollback procedures

## 7. Camadas e Responsabilidades

### Domain Layer (`/domain`)
**Responsabilidade:** Lógica PURA de negócio

**Inclui:**
- Entidades (structures com validações)
- Agregados (grupos de entidades que funcionam juntas)
- Value Objects (endereço, data, moeda)
- Interfaces de Repository (contrato apenas, não implementação)
- Domain Services (lógica pura reutilizável)

**Exemplo válido:** `TaskService.canChangeStatus(task, newStatus) → boolean` (sem efeitos colaterais)

**Exemplo INVÁLIDO:** `TaskService.saveTask(task)` (tem efeito colateral)

**Imports permitidos:** Apenas tipos, enums, outras entidades
**Imports PROIBIDOS:** Prisma, HTTP, Clerk, axios, nodemailer, qualquer framework

---

### Application Layer (`/application`)
**Responsabilidade:** Orquestra casos de uso

**Inclui:**
- UseCases (classes que implementam um caso de uso)
- DTOs (input/output estruturados)
- Exceções de aplicação

**Padrão UseCase:**
```
UseCase.execute(input, repositories, services):
  1. Validar entrada (DTO validation)
  2. Extrair dados do contexto (user, etc)
  3. Chamar domain service para validações
  4. Chamar permission validator para autorização
  5. Se OK, chamar repository para persistir
  6. Chamar notificação ou evento
  7. Retornar DTO de resultado
```

**Imports permitidos:** Domain, Repositories (interfaces), DTOs
**Imports PROIBIDOS:** HTTP direto, Prisma direto, Controllers

---

### Adapters Layer (`/adapters`)
**Responsabilidade:** Traduz entre camadas

**Subdivisões:**

**Controllers (HTTP):**
- Recebe HTTP request
- Extrai user do token Clerk
- Chama UseCase
- Retorna HTTP response
- Nunca tem lógica de negócio

**Repositories (Database):**
- Implementa interface do Domain
- Usa Prisma para query
- Retorna entidades do Domain (não DTOs)
- Exemplo: `class UserRepository implements IUserRepository { create(user) { return prisma.users.create(...) } }`

**Presenters (Serialization):**
- Serializa Domain entities → JSON
- Remove campos sensíveis
- Estrutura para frontend

**Imports:**
- Controllers: UseCase, DTO
- Repositories: Prisma, Domain entities
- Presenters: Domain entities, Response DTOs

---

### Infrastructure Layer (`/infrastructure`)
**Responsabilidade:** Implementações concretas de frameworks

**Inclui:**
- Database client (singleton Prisma)
- Config (environment variables)
- Middleware Clerk
- Serviços externos (email, PDF)

**Imports:** Frameworks, bibliotecas, env vars

## 8. Design Patterns Aplicados
- Repository: acesso a dados
- Factory: criação de objetos complexos
- Strategy: múltiplas implementações de um comportamento
- Singleton: instâncias únicas (config, logger)
- Dependency Injection: tudo vem por construtor/parâmetro

## 9. Padrões de Erro e Exceção

### Exceções por Camada

**Domain (exceções de negócio):**
```
class InvalidStatusTransitionException extends Error {}
class InsufficientPermissionException extends Error {}
class DuplicateProjectNameException extends Error {}
```

**Application (exceções de caso de uso):**
```
class UnauthorizedException extends Error {}      // 401
class ForbiddenException extends Error {}         // 403
class ValidationException extends Error {}        // 400
class NotFoundException extends Error {}          // 404
```

**HTTP (Controllers - mapear exception → HTTP):**
```
try {
  await createProjectUseCase.execute(input)
  return res.status(201).json({ success: true })
} catch (error) {
  if (error instanceof ValidationException) {
    return res.status(400).json({ error: error.message })
  } else if (error instanceof UnauthorizedException) {
    return res.status(401).json({ error: "Token inválido" })
  } else if (error instanceof ForbiddenException) {
    return res.status(403).json({ error: "Sem permissão" })
  } else if (error instanceof NotFoundException) {
    return res.status(404).json({ error: "Não encontrado" })
  } else {
    return res.status(500).json({ error: "Erro interno" })
  }
}
```

### Regra: NUNCA deixar exceção sem tratamento

Toda exception lançada em Domain/Application deve ser **explicitamente tratada em Controller**.

**INVÁLIDO (deixa stack trace vazar):**
```
controller chama useCase
useCase lança DatabaseException
Controller não trata → retorna 500 genérico
```

**VÁLIDO:**
```
domain lança UnauthorizedException
application não trata (propaga)
controller trata e retorna 403
```

---

## 10. Exemplo de Caso de Uso
Mostrar como uma funcionalidade flui pelas camadas.

## 11. Gestão de Secrets e Variáveis de Ambiente

### Arquivo `.env` e `.env.local`

**`.env.local` (NUNCA commit):**
```env
DATABASE_URL="postgresql://user:password@localhost:5432/db"
CLERK_SECRET_KEY=sk_test_xxx
API_KEY_EXTERNAL_SERVICE=key_xxx
```

**`.env.example` (SEMPRE commit):**
```env
DATABASE_URL=
CLERK_SECRET_KEY=
API_KEY_EXTERNAL_SERVICE=
```

**`.gitignore` (OBRIGATÓRIO):**
```
.env
.env.local
.env.*.local
```

### Validação de Variáveis de Ambiente

**OBRIGATÓRIO criar arquivo `src/infrastructure/config/envConfig.ts`:**

```typescript
// Validar e carregar .env no startup

const requiredEnvVars = [
  'DATABASE_URL',
  'CLERK_SECRET_KEY',
  'NEXT_PUBLIC_APP_URL',
];

function validateEnv() {
  const missing = requiredEnvVars.filter(
    (v) => !process.env[v]
  );
  
  if (missing.length > 0) {
    throw new Error(
      `Variáveis de ambiente faltando: ${missing.join(', ')}`
    );
  }
}

// Chamar validateEnv() no startup (app.ts ou middleware)
validateEnv();
```

**Checklist de Segurança:**
- ❌ Nunca logar valores de `process.env` em logs públicos
- ❌ Nunca serializar `process.env` em JSON responses
- ❌ Nunca usar `NEXT_PUBLIC_*` para secrets (são públicas!)
- ✅ Variáveis sensíveis (secrets) SEMPRE em `.env` não-public
- ✅ Variáveis públicas (URLs, IDs) com `NEXT_PUBLIC_` prefix
- ✅ Validar que variáveis foram carregadas antes de usar

### Em Produção (CI/CD)

**GitHub Actions exemplo:**
```yaml
env:
  DATABASE_URL: ${{ secrets.DATABASE_URL }}
  CLERK_SECRET_KEY: ${{ secrets.CLERK_SECRET_KEY }}

steps:
  - name: Deploy
    run: npm run build && npm run deploy
```

**Vercel:**
```
Project Settings → Environment Variables
→ Adicionar cada SECRET individualmente
```

**Regra:** Secrets NUNCA aparecem em logs ou output do build

## 12. Proibições e Red Flags
- ❌ Nunca colocar credenciais no código
- ❌ Nunca importar Prisma (ou ORM) na camada de domínio
- ❌ Nunca referenciar HTTP direto em regras de negócio
- ❌ Nunca aceitar dados sem validação (sempre validar no backend)
- ❌ Nunca expor IDs internos ou sequenciais em respostas públicas
- ❌ Nunca colocar chaves de API no frontend
- ❌ Nunca confiar na validação do frontend (sempre revalidar no servidor)

---

## Linguagem e Tom

- **Simples e acessível**: evite jargão técnico pesado; explique por que cada padrão importa
- **Prático**: mostre exemplos reais de estrutura
- **Imperativo**: use "Faça X" em vez de "X deve ser feito"
- **Direto**: cada seção tem um propósito claro

---

## O que NÃO incluir

- Código específico de implementação
- Exemplos em linguagem de programação (a skill é agnóstica)
- Decisões de UI/UX (foco é arquitetura backend/estrutura)
- Configurações sensíveis (senhas, tokens, etc.)

---

## Checklist de Qualidade para IA

Antes de entregar o ARCHITECTURE.md:

**Fluxo e Camadas (crítico para vibe coding)**
- [ ] Fluxo de chamadas está explícito: Controller → UseCase → Service/Repository
- [ ] Seção "O que cada camada faz" está documentada (o que importa, nunca importa)
- [ ] Domain layer NUNCA importa Prisma/HTTP/Clerk
- [ ] UseCase SEMPRE faz autorização ANTES de persistir
- [ ] Diferença entre UseCase, Service, Domain Service está clara

**Padrões de Erro (importante para IA tratar exceções)**
- [ ] Seção "Padrões de Erro" mostra como cada camada lança exceções
- [ ] Controllers mapeiam exceções → HTTP status codes (400, 401, 403, 404, 500)
- [ ] Exemplo de try-catch em Controller mostra tratamento

**Segurança**
- [ ] Thin Client, Fat Server está explícito (toda lógica no backend)
- [ ] Seção de "O que NUNCA colocar no código" está clara
- [ ] Variáveis de ambiente (.env) documentadas com validação
- [ ] `.gitignore` protege `.env` explicitamente
- [ ] Há arquivo exemplo `envConfig.ts` para validação de startup
- [ ] Autenticação (Clerk middleware) vs Autorização (UseCase permissionValidator) distintos
- [ ] Validação sempre no backend, exemplo mostra try-catch

**Estrutura e Padrões**
- [ ] Estrutura de pastas é por domínio, não por tipo técnico
- [ ] Padrões de nomenclatura cobrem arquivos, classes, variáveis, constantes, enums
- [ ] Design Patterns (Repository, Factory, Strategy) estão com exemplos
- [ ] Há lista de Red Flags (❌ NUNCA, ✅ SEMPRE)

**Acessibilidade para IA**
- [ ] Fluxo de chamadas usa "→" ou números (determinístico)
- [ ] Cada camada tem tabela "Imports permitidos | Imports PROIBIDOS"
- [ ] Exemplos de código mostram padrão (sem depender de linguagem específica)
- [ ] Exceções têm nomes explícitos (UnauthorizedException, ForbiddenException)
- [ ] Linguagem evita ambiguidade (nunca "pode", sempre "deve" ou "nunca")

---

## Confirmação ao entregar

Após criar o arquivo, informe ao usuário:

- ✅ `docs/ARCHITECTURE.md` criado com Clean Architecture + stack Phocus
- 🔜 Próximo passo: gerar `docs/WORKFLOW.md` e `CLAUDE.md` com `/docs`
