---
name: phocus-break
description: >
  Passo B do Phocus App Guide — quebra uma Spec técnica em módulos de desenvolvimento independentes e sequenciais, prontos para alimentar o /plan e o /execute no Claude Code. Use quando o usuário disser "/break", "quebrar a spec em módulos", "dividir o projeto em partes", "quero modularizar o app", "como dividir o desenvolvimento", ou logo após concluir uma spec pelo /spec. Acione também quando o usuário quiser saber por onde começar a codar ou como organizar as sessões de vibe coding. Este skill lê a spec, identifica dependências entre funcionalidades e gera um plano de módulos com ordem de execução clara — cada módulo é pequeno o suficiente para uma sessão de Claude Code, testável de forma isolada, e com critério objetivo de "pronto".
---

# /break — Quebradora de Spec em Módulos

Você está executando o **Passo B** da Fase 2 do Phocus App Guide.

Seu papel é pegar a Spec gerada no Passo A e quebrá-la em **módulos de desenvolvimento independentes e sequenciais**. Cada módulo é uma unidade de trabalho que o Claude Code consegue executar em uma sessão sem se perder.

O erro mais comum no vibe coding é jogar a spec inteira de uma vez. O Claude Code começa bem, mas perde o fio depois de 30% do código. O /break resolve isso: cada sessão tem escopo claro, começo e fim.

---

## Passo 1 — Receba a Spec

O usuário vai fornecer a spec em uma das formas:
- Arquivo `.md` gerado pelo Passo A (/spec)
- Texto colado diretamente

Leia tudo antes de começar. O modelo de dados, as regras de negócio e a ordem de construção recomendada na spec são o ponto de partida para definir os módulos.

---

## Passo 2 — Identifique as Camadas de Dependência

Antes de definir os módulos, mapeie mentalmente as dependências. A regra é simples: **nenhum módulo pode depender de algo que ainda não foi construído.**

Organize sempre nessa sequência de camadas — nunca pule uma:

1. **Fundação** — Schema do banco, migrations, seed de dados iniciais
2. **Lógica de negócio** — Utility functions, cálculos, regras críticas (sem UI, sem API)
3. **Acesso a dados** — Repository layer (Prisma por trás dos Repositories)
4. **API** — Rotas RESTful, middleware de autenticação (Clerk)
5. **Interface** — Componentes, páginas, fluxos de UI
6. **Integrações** — Email, PDF, serviços externos

Cada módulo pertence a uma camada. Módulos de camadas inferiores sempre vêm antes.

---

## Passo 3 — Defina os Módulos

Crie módulos com estas características:

- **Tamanho:** uma sessão de vibe coding (estimativa: 1-3h de Claude Code)
- **Escopo:** uma responsabilidade clara, sem fazer muita coisa ao mesmo tempo
- **Testável:** o GC consegue verificar que funcionou sem precisar do módulo seguinte
- **Nome:** verbo + substantivo que descreve a entrega (ex: "Calcular dias úteis", não "Módulo 2")

Se um módulo parecer grande demais, divida. Se dois módulos parecerem inseparáveis, una.

---

## Passo 4 — Gere o Documento de Módulos

Use este template exato para cada módulo:

---

```markdown
# Break — [Nome do App]

## Visão Geral
[1-2 linhas resumindo a estratégia de divisão — por que esses módulos, nessa ordem]

**Total de módulos:** [N]
**Tempo estimado:** [X sessões de vibe coding]

---

## Módulo [N] — [Nome do Módulo]
**Camada:** [Fundação / Lógica de negócio / Acesso a dados / API / Interface / Integrações]
**Depende de:** [Módulo X, Y — ou "nenhum" se for o primeiro]

### O que este módulo entrega
[Descrição objetiva do que será construído — 2-4 linhas]

### Inclui
- [Item específico 1]
- [Item específico 2]
- [Item específico 3]

### NÃO inclui (fica para módulos seguintes)
- [O que explicitamente não entra aqui]
- [Evita que o Claude Code "adiante" trabalho]

### Prompt de entrada para o Claude Code
[Prompt direto e específico para iniciar este módulo no Claude Code.
Deve mencionar: o que já existe (módulos anteriores), o que construir agora, e o que NÃO fazer.]

---
Contexto: [nome do app]. [1 linha sobre o que já foi construído nos módulos anteriores, ou "projeto novo" se for o primeiro.]

Construa agora: [descrição clara do escopo deste módulo]

Não construa ainda: [o que deve ser ignorado nesta sessão]

Critério de conclusão: [como saber que este módulo está pronto]
---

### Como testar antes de avançar
[Checklist simples — o que o usuário deve verificar antes de iniciar o próximo módulo]
- [ ] [Teste 1]
- [ ] [Teste 2]

---
```

Repita o bloco acima para cada módulo, na ordem de execução.

---

## Passo 5 — Adicione o Mapa de Dependências

Ao final do documento, inclua um mapa visual simples das dependências entre módulos:

```markdown
## Mapa de Dependências

Módulo 1 → Módulo 2 → Módulo 3
                    ↘ Módulo 4 → Módulo 5
```

Se o projeto for linear (um módulo após o outro), diga isso explicitamente. Se houver módulos que podem ser executados em paralelo após certo ponto, sinalize.

---

## Passo 6 — Entregue e Oriente

1. **Salve** o arquivo em `planning/break-[nome-do-app].md`
   - Se a pasta `planning/` não existir, criá-la
2. **Oriente** o próximo passo:

```
✅ Break salvo em planning/break-[app].md

Próximos passos:
  → /plan  — detalhar as tasks do Módulo 1 antes de executar
  → /execute  — se o módulo for simples, pode ir direto
```

---

## Princípios que guiam este skill

**Módulos pequenos ganham.** Um módulo que o Claude Code completa em uma sessão vale mais que um módulo grande que trava no meio. Em caso de dúvida, divida.

**Nunca pule camadas.** Interface antes de API, API antes de Repository, Repository antes de Schema — isso sempre gera retrabalho. A ordem das camadas não é sugestão, é lei.

**Prompts de entrada são contratos.** O prompt de cada módulo deve deixar claro o que já existe e o que não deve ser feito. O Claude Code trabalha melhor com escopo delimitado do que com liberdade total.

**O critério de "pronto" protege você.** Sem um critério claro, o usuário avança para o próximo módulo com bugs escondidos. O checklist de teste é a porta de saída de cada módulo.
