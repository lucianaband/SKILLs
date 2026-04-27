---
name: phocus-plan
description: >
  Passo C do Phocus App Guide — pega um módulo específico do /break e detalha as tasks de desenvolvimento antes de executar no Claude Code. Use quando o usuário disser "/plan", "planejar o módulo", "detalhar as tasks", "o que preciso fazer no módulo X", "como atacar esse módulo", ou quando estiver prestes a iniciar uma sessão de vibe coding e quiser um roteiro granular. Acione também quando o usuário quiser saber a ordem exata dos passos dentro de um módulo antes de colar o prompt no Antigravity. Este skill transforma um módulo do /break em uma lista ordenada de tasks técnicas com critérios de aceite, estimativas e alertas de risco — evitando surpresas no meio da sessão de vibe coding.
---

# /plan — Planejador de Módulo

Você está executando o **Passo C** da Fase 2 do Phocus App Guide.

Seu papel é pegar um módulo específico do /break e detalhar as **tasks técnicas** que o compõem — na ordem certa, com critérios de aceite claros e alertas para os pontos que podem travar.

O /plan é opcional para módulos simples, mas essencial para módulos complexos. Se o módulo tem mais de 3 componentes interdependentes ou envolve lógica crítica de negócio, planeje antes de executar.

---

## Passo 1 — Receba o Módulo

O usuário vai fornecer:
- O módulo específico do break (número + nome)
- O arquivo `break-[app].md` com o contexto completo, **ou**
- A spec `spec-[app].md` se quiser replanejar do zero

Leia o escopo do módulo, o que inclui, o que **não** inclui e o critério de "pronto" antes de começar.

---

## Passo 2 — Identifique os Riscos Antes de Planejar

Para cada módulo, avalie mentalmente antes de escrever as tasks:

**Riscos técnicos comuns:**
- Lógica de cálculo com casos de borda não mapeados (ex: feriado na data de início)
- Dependências entre tasks dentro do mesmo módulo (task B não pode começar antes de A terminar)
- Integrações externas com comportamento incerto (email, PDF, APIs de terceiros)
- Estado global que afeta múltiplos componentes ao mesmo tempo

Se identificar risco alto, marque a task com `⚠️` e adicione uma nota de alerta.

---

## Passo 3 — Gere o Plano do Módulo

Use este template exato:

---

```markdown
# Plan — [Nome do App] · Módulo [N]: [Nome do Módulo]

## Contexto rápido
**O que este módulo entrega:** [1-2 linhas do break]
**Depende de:** [módulos anteriores ou "nenhum"]
**Tempo estimado:** [X horas de vibe coding]

---

## Tasks em ordem de execução

### Task [N.1] — [Nome da task]
**O que fazer:** [Descrição técnica objetiva — o suficiente para o Claude Code entender sem ambiguidade]
**Critério de aceite:** [Como saber que esta task está concluída — verificável sem interpretação]
**Risco:** [Baixo / Médio / Alto + 1 linha explicando se for Médio ou Alto]

---

### Task [N.2] — [Nome da task]
**O que fazer:** [Descrição técnica]
**Critério de aceite:** [Verificável]
**Risco:** Baixo

---

[Repita para cada task]

---

## Prompt consolidado para o Claude Code

[Um único prompt que engloba todas as tasks do módulo em sequência.
Mais detalhado que o prompt do /break — inclui a ordem das tasks e os critérios de aceite embutidos.]

---
Contexto: [app] — [o que já foi construído nos módulos anteriores].

Módulo [N]: [nome]. Execute as tasks nesta ordem exata:

**Task 1 — [nome]**
[Instrução técnica específica]
Aceite: [critério verificável]

**Task 2 — [nome]**
[Instrução técnica específica]
Aceite: [critério verificável]

[...]

⚠️ Atenção: [alertas de risco identificados]

Não avance para o próximo módulo sem que todos os critérios de aceite estejam satisfeitos.
---

## Checklist final do módulo
[Reproduz o checklist do /break + adiciona os critérios de aceite das tasks críticas]

- [ ] [Critério do break 1]
- [ ] [Critério do break 2]
- [ ] [Task crítica N.X validada]
```

---

## Passo 4 — Gere os arquivos de issues

Além do plano, gere um arquivo de issue para cada task. Esses arquivos são consumidos diretamente pelo `/execute`.

**Formato de cada issue** (`issues/XX-nome-da-task.md`):

```markdown
# Issue [XX] — [Nome da Task]

## Contexto
**Módulo:** [N] — [Nome do Módulo]
**App:** [Nome do App]
**Depende de:** [issue anterior, ou "nenhuma"]

## O que construir
[Descrição técnica objetiva — o suficiente para o Claude Code executar sem ambiguidade]

## Stack e padrões obrigatórios
- Next.js (App Router) + TypeScript
- Prisma ORM — acesso ao banco exclusivamente via Repository
- Clerk — autenticação via middleware, nunca inline
- Clean Architecture: Domain → Application → Adapters → Infrastructure

## Arquivos a criar ou modificar
- `[caminho/arquivo1]` — [o que faz]
- `[caminho/arquivo2]` — [o que faz]

## Critério de aceite
- [ ] [Verificação 1 — objetiva e testável]
- [ ] [Verificação 2]

## ⚠️ Alertas
[Riscos identificados no /plan, ou "nenhum" se risco baixo]

## Subagente recomendado
[component-writer / route-writer / model-writer / integration-writer / test-writer]
```

**Numeração das issues:** sequencial a partir do último número existente na pasta `issues/`. Se a pasta estiver vazia, começar do `01`.

## Passo 5 — Entregue e Oriente

1. **Salve o plano** em `planning/plan-[app]-modulo[N].md`
   - Se a pasta `planning/` não existir, criá-la
2. **Salve as issues** em `issues/XX-nome-da-task.md` (uma por task)
   - Se a pasta `issues/` não existir, criá-la
3. **Oriente** o próximo passo:

```
✅ Plano salvo em planning/
✅ [N] issues criadas em issues/

Para executar:
  git add . && git commit -m "checkpoint antes do modulo [N]"
  /execute issues/XX-nome-da-task.md

Execute uma issue por vez. Valide o checklist antes de avançar.
```

---

## Quando pular o /plan

Para módulos simples — fundação de banco, seed de dados, rotas CRUD básicas — o prompt do /break já é suficiente. Use o /plan quando:

- O módulo envolve **lógica de negócio** complexa (cálculos, regras com casos de borda)
- O módulo tem **mais de 4 tasks** interdependentes
- Existe **risco alto** de o Claude Code "adivinhar" o comportamento errado
- É a primeira vez que você trabalha com aquela tecnologia ou padrão

Em caso de dúvida, planeja. Custa 5 minutos e pode poupar 2 horas de retrabalho.

---

## Princípios que guiam este skill

**Tasks são atômicas.** Cada task faz uma coisa só e tem um critério de aceite verificável. "Implementar o módulo de tarefas" não é uma task — é um módulo inteiro.

**Ordem importa.** Tasks dentro de um módulo têm dependências assim como módulos têm dependências entre si. A ordem no plano não é sugestão.

**Risco declarado é risco gerenciado.** Marcar `⚠️` numa task não é pessimismo — é honestidade que evita surpresa no meio da sessão.

**O prompt consolidado é o produto final.** O plano existe para gerar um prompt melhor. Um prompt bem estruturado com tasks em ordem e critérios embutidos produz código mais correto que um prompt genérico.
