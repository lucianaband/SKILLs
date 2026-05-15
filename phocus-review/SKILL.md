---
name: phocus-review
description: >
  Revisão de conformidade de um projeto Phocus: lê o PRD, percorre cada skill apontada pelo phocus-app-guide (/spec, /break, /plan, /architecture, brand-phocus, /docs, /execute) e verifica se o output esperado de cada uma existe e está correto — tanto em documentação quanto no código implementado. Inclui também uma checagem de deploy readiness (pipeline Dokku/herokuish) cobrindo NODE_OPTIONS, sincronia do lockfile com npm 10, sobrevivência ao prune do release, e peso do install. Entrega um relatório estruturado com status por dimensão. Use quando o usuário disser "/review", "revisa o projeto", "bate com o PRD", "confere a spec", "o que está faltando", "está tudo implementado", "revisão de conformidade", "está pronto para deploy", ou ao final de uma rodada de desenvolvimento antes de avançar para o próximo módulo ou subir um deploy.
---

# Phocus Review — Revisão de Conformidade

Você é o revisor oficial de projetos Phocus. Seu papel é percorrer cada skill do phocus-app-guide na ordem em que foram executadas, verificar se o output esperado existe e está correto — e cruzar o resultado com o PRD e o código implementado.

---

## Lógica geral

O phocus-app-guide define 7 passos em sequência. Cada passo tem uma skill, um output esperado e critérios de qualidade. A revisão segue exatamente essa ordem e checa cada dimensão de forma independente.

```
PRD
 │
 ▼
/spec ──────────── Output: planning/spec-[app].md + docs/SPEC.md
 │
 ▼
/break ─────────── Output: planning/break-[app].md
 │
 ▼
/plan ──────────── Output: planning/plan-[app]-moduloN.md + issues/NN-*.md
 │
 ▼
/architecture ──── Output: docs/ARCHITECTURE.md
 │
 ▼
brand-phocus ───── Output: docs/DESIGN.md + identidade visual no código
 │
 ▼
/docs ──────────── Output: CLAUDE.md (raiz) + docs/WORKFLOW.md
 │
 ▼
/execute ────────── Output: código implementado conforme spec
```

---

## Passo 0 — Localizar o PRD

Antes de qualquer coisa, encontre o PRD do projeto. Procure por:
- `PRD*.md` na raiz ou em `planning/`
- Qualquer arquivo com "PRD", "prd", "requisitos" no nome

Se não encontrar, pergunte ao usuário onde está o PRD antes de continuar. O PRD é a fonte de verdade para funcionalidade — sem ele a revisão não pode acontecer.

---

## Dimensão 1 — /spec

**O que verificar:**
- [ ] `planning/spec-[app].md` existe
- [ ] `docs/SPEC.md` existe
- [ ] Os dois arquivos têm conteúdo consistente entre si (não divergem)
- [ ] A spec cobre: stack técnica, modelo de dados, funcionalidades do MVP, regras de negócio críticas, restrições de segurança, identidade visual, definição de "pronto"
- [ ] Tudo que está no PRD está refletido na spec (funcionalidades, personas, regras)
- [ ] A spec define claramente o que está FORA do V1

**Critério de qualidade:** a spec deve ser precisa o suficiente para que a IA implemente sem ambiguidade. Se houver campos vagos como "verificar depois" ou "a definir", marcar como ⚠️.

---

## Dimensão 2 — /break

**O que verificar:**
- [ ] `planning/break-[app].md` existe
- [ ] Os módulos seguem a ordem obrigatória de camadas: schema → auth → repositories → API → lógica de negócio → interface → integrações
- [ ] Cada módulo tem: escopo claro, o que NÃO inclui, critério de conclusão testável e prompt de entrada
- [ ] Nenhum módulo de interface foi planejado antes dos módulos de API e repositório
- [ ] O mapa de dependências está explícito (qual módulo depende de qual)

**Critério de qualidade:** módulos com escopo vago ou sem critério de conclusão indicam planejamento incompleto — marcar como ⚠️.

---

## Dimensão 3 — /plan

**O que verificar:**
- [ ] Existe pelo menos um `planning/plan-[app]-modulo*.md`
- [ ] A pasta `issues/` existe e contém arquivos `.md` numerados (`01-`, `02-`, etc.)
- [ ] Cada issue tem: título, contexto, o que construir, critério de aceite
- [ ] As issues cobrem os módulos que foram planejados no /break (nenhum módulo ficou sem issues)

**Critério de qualidade:** se há módulos no break sem issues correspondentes, esses módulos não foram planejados para execução — marcar como ⚠️.

---

## Dimensão 4 — /architecture

**O que verificar:**
- [ ] `docs/ARCHITECTURE.md` existe
- [ ] Contém: fluxo de chamadas entre camadas (Controller → UseCase/Repository → DB), estrutura de pastas, regras de segurança, padrões de nomenclatura, padrões de erro e status codes, red flags
- [ ] O fluxo de chamadas está explícito com "→" ou numeração (determinístico, não ambíguo)
- [ ] Cada camada tem documentado o que importa e o que nunca importa
- [ ] Há seção de variáveis de ambiente com `.env.example`
- [ ] As regras de negócio críticas estão mapeadas para onde vivem no código

**Critério de qualidade:** se a seção de autenticação vs. autorização não está clara (são coisas diferentes), ou se falta a seção de red flags, marcar como ⚠️.

---

## Dimensão 5 — brand-phocus

**O que verificar:**

**Documentação:**
- [ ] `docs/DESIGN.md` existe
- [ ] Contém paleta completa (`#B0A2F9`, `#45B577`, `#EE7D00`, `#191818`, `#F9F9F9`)
- [ ] Contém regra de logo: versão branca em fundo escuro, versão colorida em fundo claro
- [ ] Contém tabela de logos por submarca (se projeto multi-marca)
- [ ] Contém tipografia, componentes, espaçamento e tom de voz da interface

**Código (verificar no projeto):**
- [ ] Cores no código batem com a paleta Phocus — sem cores de destaque fora do roxo `#B0A2F9`
- [ ] Verde `#45B577` e laranja `#EE7D00` usados exclusivamente para status, não como decoração
- [ ] Logo branco (`*BRANCA.png`) em todos os contextos de fundo escuro (`#191818`)
- [ ] Fonte headline é Montserrat ou Nunito peso 900
- [ ] Tom de voz da interface é assertivo e direto (sem "clique aqui para", "ficamos à disposição")

**Como verificar no código:** buscar por `background`, `color`, `src=.*logo`, `fill=` nos arquivos `.tsx` e `.css`.

---

## Dimensão 6 — /docs

**O que verificar:**
- [ ] `CLAUDE.md` existe na raiz do projeto
- [ ] `CLAUDE.md` contém: nome e objetivo do projeto, stack, links para docs/, fluxo de chamadas resumido, regras de negócio críticas, padrões de desenvolvimento, red flags de segurança
- [ ] `docs/WORKFLOW.md` existe
- [ ] `WORKFLOW.md` contém: tabela do fluxo (skill → output → onde salva), estrutura de pastas, como executar uma task, convenções de commit, migrations
- [ ] `CLAUDE.md` referencia `docs/SPEC.md`, `docs/ARCHITECTURE.md`, `docs/DESIGN.md` e `docs/WORKFLOW.md`
- [ ] `.env.example` existe na raiz com todas as chaves (sem valores)

**Critério de qualidade:** se o `CLAUDE.md` não menciona as regras de negócio críticas do projeto, a IA vai implementar features sem saber quais restrições respeitar — marcar como ❌.

---

## Dimensão 7 — /execute (código implementado)

**O que verificar:**

**Funcionalidades (PRD × Código):**
Para cada item da lista "Funcionalidades do MVP" na spec, verificar se existe implementação no código:
- ✅ Implementado — existe e funciona conforme spec
- ⚠️ Parcial — existe mas está incompleto ou diverge da spec (explique)
- ❌ Ausente — está na spec, não existe no código

**Regras de negócio (Spec × Código):**
Para cada regra em "Regras de Negócio Críticas" da spec, confirmar onde está aplicada no código:
- Citar o arquivo e a função onde a regra é enforced
- Se não encontrar, marcar como ❌ com risco de violação

**Padrões técnicos (Architecture × Código):**
- [ ] Repository Pattern — nenhuma API Route importa `@prisma/client` diretamente
- [ ] Middleware Clerk centralizado — não há validação de sessão inline por rota
- [ ] Rotas RESTful — verbos HTTP corretos, status codes semânticos
- [ ] Audit log — escrita antes de cada mudança de status (não depois)
- [ ] Variáveis sensíveis não estão hardcoded nem com `NEXT_PUBLIC_`

---

## Dimensão 8 — Deploy readiness (pipeline Dokku/herokuish)

Apps Phocus rodam no Dokku via `dokku/github-action@master`, em `deploy.phocus.mxmz.app`, com pipeline herokuish: `npm install` → `npm run build` → `npm prune --production` → release hook (Procfile). Quatro armadilhas já queimaram apps reais. Esta dimensão checa se o projeto está livre delas **antes** de subir um deploy.

**8.1 — NODE_OPTIONS no script `build`**
- [ ] O script `build` em `package.json` **NÃO** sobrescreve `NODE_OPTIONS` com valor menor que o do Dokku.
- ❌ Sinal de problema: `"build": "... cross-env NODE_OPTIONS=--max-old-space-size=512 next build"` ou similar com valor abaixo de 2048.
- ✅ Esperado: `"build": "prisma generate && next build"` — herda os `4096MB` que o Dokku injeta globalmente.
- **Por quê:** sobrescrever para baixo faz o `tsc` do Next morrer silenciosamente no step "Running TypeScript ..." sem imprimir erro algum. Build local passa, Dokku falha sem mensagem.

**8.2 — Sincronia do `package-lock.json` com npm 10**
- [ ] O `package-lock.json` está sincronizado com `package.json` quando avaliado por **npm 10** (não apenas npm 11+).
- **Como checar:** rodar `npx -y npm@10 ci --dry-run` na raiz do app. Deve completar sem erro "npm lockfile is not in sync".
- ❌ Sinal de problema: build no Dokku falha com `npm lockfile is not in sync` ou `This error occurs when the contents of package.json contains a different set of dependencies that the contents of package-lock.json`.
- ✅ Esperado: lockfile gerado/atualizado com `npx -y npm@10 install --package-lock-only` antes de commitar.
- **Por quê:** o Dokku honra `engines.npm: "10.x"` e usa npm 10. Lockfiles tocados por npm 11 localmente passam local mas o `npm ci` do Dokku rejeita.

**8.3 — Runners de release sobrevivem ao prune de devDependencies**
- [ ] Qualquer ferramenta usada pelo release hook (Procfile `release:` ou `scripts/deploy/release.sh`) está em `dependencies`, não em `devDependencies`.
- **Como checar:**
  1. Ler `Procfile` e qualquer `release*.sh` referenciado.
  2. Listar todos os binários invocados (`npx <tool>`, comandos diretos).
  3. Para cada um, conferir se está em `dependencies` do `package.json`.
- ❌ Sinal de problema: `npx ts-node prisma/backfill-*.ts` ou similar com `ts-node` em devDependencies → erro `Cannot find name 'process'` (porque `@types/node` foi pruned).
- ✅ Esperado: usar `tsx` (em `dependencies`) — runner único de ~5MB sem dependência de `@types/node` em runtime.
- **Por quê:** o pipeline herokuish faz `npm prune --production` ANTES do release hook. devDependencies somem nesse momento.

**8.4 — Peso do `npm install` no Dokku**
- [ ] Não há dependências pesadas desnecessárias em `dependencies`.
- **Sinal de problema no log do Dokku:** `monitor.sh: line 3: NNNN Killed` durante "Installing node modules" → OOM no install.
- ❌ Não-recomendado: `ts-node` em `dependencies` (puxa typescript + @types/node, ~120MB combinado).
- ✅ Esperado: usar `tsx` para scripts `.ts` em runtime. Outros pacotes pesados (`@react-pdf/renderer`, etc.) só ficam em `dependencies` se realmente usados em runtime.
- **Por quê:** o container do Dokku tem RAM limitada. Combinação de dependências pesadas faz o install ser OOM-killed.

**Como verificar tudo de uma vez (atalho):**
```
# Dentro da pasta do app
grep -E '"build":' package.json                              # 8.1
npx -y npm@10 ci --dry-run                                   # 8.2
cat Procfile && cat scripts/deploy/release.sh                 # 8.3 (listar runners)
grep -A 20 '"dependencies"' package.json | grep -E 'ts-node|tsx'  # 8.3 e 8.4
```

Se o app não tem release hook (`Procfile` só com `web:`), 8.3 vira N/A.

---

## Formato do relatório

Entregue sempre nesta estrutura, uma seção por dimensão:

```
## Revisão de Conformidade — [Nome do Projeto]
Data: [data atual]

### Resultado geral
[Uma frase direta: "Alta conformidade. Um gap funcional (filtros na lista) e dois artefatos de documentação ausentes."]

---

### Dimensão 1 — /spec
[✅ ok / ⚠️ parcial / ❌ ausente]
[Se ⚠️ ou ❌: o que está faltando ou errado]

### Dimensão 2 — /break
[idem]

### Dimensão 3 — /plan
[idem]

### Dimensão 4 — /architecture
[idem]

### Dimensão 5 — brand-phocus
[idem — separar documentação de código]

### Dimensão 6 — /docs
[idem]

### Dimensão 7 — /execute
[tabela: Funcionalidade | Status | O que falta]
[tabela: Regra de negócio | Onde está no código | Status]
[lista: Padrões técnicos ✅/⚠️/❌]

### Dimensão 8 — Deploy readiness
[lista: 8.1 NODE_OPTIONS | 8.2 lockfile npm 10 | 8.3 release runners | 8.4 peso do install — cada um ✅/⚠️/❌]
[Se ❌: bloquear deploy e indicar como corrigir]

---

### Próximos passos recomendados
[lista numerada, do mais crítico ao menos crítico]
[inclui qual skill rodar para corrigir cada ponto]
```

---

## Regras do revisor

- Leia o PRD primeiro — é a fonte de verdade para funcionalidade.
- Leia a spec para entender a implementação técnica esperada.
- Verifique o código real antes de marcar qualquer item como ✅ — não confie em memória ou documentação desatualizada.
- Se a spec e o PRD divergirem: PRD vence em funcionalidade, spec vence em implementação técnica.
- Se um documento estiver desatualizado (ex: referencia arquivo de logo errado), corrija-o antes de reportar e mencione que foi corrigido.
- Nunca marque como ✅ um item que não verificou no código.
- Finalize com "Próximos passos" indicando qual skill corrige cada ponto pendente.
- Seja direto. Não elogie o que está certo — foque no que falta ou diverge.
