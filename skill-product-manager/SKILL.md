---
name: skill-product-manager
description: Skill para Product Managers criarem especificações de produto (PRD, FRD, Épico, Histórias, Tasks) seguindo metodologia de Spec Driven Development. Workflows adaptados para LLM com templates padronizados, regras de qualidade e boas práticas incorporadas. Use quando o usuário precisar criar ou editar documentos de requisitos, épicos, histórias de usuário, ou features, ou quando mencionar termos como PRD, FRD, especificação de produto, user story, ou Spec Driven Development.
---

# Skill Product Manager

## Visão Geral

Skill para criação e gerenciamento de especificações de produto seguindo metodologia de Spec Driven Development. Permite que Product Managers criem PRDs, FRDs, Épicos, Histórias e Tasks com templates padronizados e boas práticas incorporadas.

## Quando Usar

Use esta skill quando precisar:
- Criar ou editar Documentos de Requisitos de Produto (PRD)
- Criar Feature Requirements Documents (FRD)
- Criar Épicos que agrupam histórias relacionadas
- Criar Histórias de Usuário, Tasks ou Bugs
- Esclarecer especificações existentes
- Estabelecer um processo estruturado de documentação de produto

## Como Funciona

Esta skill organiza workflows, regras e templates para criação de especificações:

**Estrutura:**
- `workflows/product/` - Contém os fluxos de trabalho para cada tipo de especificação
- `rules/` - Regras invioláveis e princípios fundamentais
- `templates/product/` - Templates markdown para cada tipo de documento

**Adaptação para LLM:**
- Arquivos são criados no ambiente Claude e disponibilizados para download
- Variáveis de ambiente de IDEs foram adaptadas para contexto de LLM
- Estrutura e conteúdo dos templates mantidos intactos
- Comandos e fluxos preservados com aliases amigáveis

## Comandos Disponíveis

### Aliases Amigáveis

Você pode usar comandos naturais que serão mapeados para os workflows corretos:

**Para PRDs:**
- "crie um PRD sobre..."
- "novo PRD para..."
- "edite o PRD..."

**Para Issues (Histórias/Tasks/Bugs):**
- "crie uma história sobre..."
- "nova task para..."
- "crie um bug..."

**Para Épicos:**
- "crie um épico sobre..."
- "novo épico para..."

**Para FRDs:**
- "crie um FRD sobre..."
- "novo FRD para..."

**Para Esclarecimento:**
- "esclareça a especificação..."
- "clarifique o PRD..."

### Workflows Internos

Internamente, estes comandos mapeiam para os workflows em `workflows/product/`:

- `prod.spec.prd.md` - Criar/editar PRDs
- `prod.spec.issue.md` - Criar histórias, tasks, bugs
- `prod.spec.epic.md` - Criar épicos
- `prod.spec.frd.md` - Criar FRDs
- `prod.spec.clarify.md` - Esclarecer especificações

## Regras Fundamentais

Sempre siga as regras definidas em `rules/global-rules.md`:

### Principais Princípios

1. **Idioma padrão**: Português do Brasil (alterar se solicitado)
2. **Fidelidade aos templates**: Seguir estruturas exatamente como definidas
3. **Não inventar dados**: Sempre perguntar quando não souber
4. **Níveis de especificação**: Respeitar diferenças entre PRD/Epic/Story
5. **Contexto completo**: Documentos auto-explicativos
6. **Terminologia consistente**: Usar mesmos termos em todos os docs

### Diferenças de Níveis

**PRD (Alto Nível):**
- Foco no "O QUÊ" e "POR QUÊ"
- Capacidades gerais do produto
- Regras de negócio
- Métricas de sucesso

**FRD (Feature Level):**
- Detalhamento de funcionalidades específicas
- Requisitos técnicos e de produto combinados
- Jornadas de usuário detalhadas
- Comportamentos esperados do sistema

**Épicos (Nível Médio):**
- Ponte entre estratégia e implementação
- Requisitos de nível de feature
- Pontos de integração
- Requisitos não-funcionais

**Histórias/Tasks (Detalhado):**
- Foco em comportamentos específicos e testáveis
- Baseado em designs
- Formato: "Quando o usuário... o sistema..."

## Templates Disponíveis

Todos os templates estão em `templates/product/`:

- `prod-prd-template.md` - Template de PRD
- `prod-issue-template.md` - Template de Issue (Story/Task/Bug)
- `prod-epic-template.md` - Template de Épico
- `prod-frd-template.md` - Template de FRD

## Fluxo de Trabalho

### Criação de Arquivos

**Importante**: Quando workflows mencionarem variáveis como `$DOCS_FOLDER_PROD` ou instruções para "salvar em X":
- Crie o arquivo localmente em `/home/claude/`
- Siga a convenção de nomenclatura indicada
- Use `present_files` para disponibilizar ao usuário
- Não tente salvar em caminhos de projeto (não existe filesystem fixo)

**Exemplo**: Se workflow diz "Salve o PRD em `$DOCS_FOLDER_PROD/prd-{ID}.md`"
→ Crie `prd-001.md` e apresente ao usuário

### Processo Completo

1. **Receber solicitação do usuário** (ex: "crie um PRD sobre sistema de pagamentos")
2. **Identificar tipo de especificação** necessária
3. **Carregar workflow apropriado** de `workflows/product/`
4. **Aplicar regras** de `rules/global-rules.md`
5. **Seguir template** de `templates/product/`
6. **Fazer perguntas essenciais** (não ser verboso)
7. **Criar arquivo markdown** estruturado
8. **Apresentar ao usuário** via present_files

## Nomenclatura de Arquivos

Mantenha as convenções:

- **PRD**: `prd-{ID}.md` (ex: `prd-001.md`)
- **FRD**: `frd-{ID}-{nome-feature}.md` (ex: `frd-001-autenticacao.md`)
- **Epic**: `epic-{ID}-{nome}.md` (ex: `epic-001-fluxo-pagamento.md`)
- **Issue**: `{issue-name}-{id}.md` (ex: `validacao-cartao-story123.md`)

## Boas Práticas

### Ao Criar Especificações

- ✅ Seja conciso mas completo
- ✅ Use perguntas diretas e objetivas
- ✅ Não seja verboso
- ✅ Faça suposições informadas quando apropriado
- ✅ Peça confirmação do usuário antes de finalizar
- ❌ Não invente dados ou informações
- ❌ Não adicione seções não solicitadas ao template
- ❌ Não faça perguntas excessivas

### Critérios de Aceitação

**PRD/Epic (Macro):**
```
✅ "O usuário pode filtrar resultados por faixa de preço"
✅ "O sistema valida formato de email durante cadastro"
❌ "Mostrar erro em vermelho abaixo do campo"
❌ "Botão azul de submit"
```

**Histórias/Tasks (Micro):**
```
✅ "Quando usuário clica 'Login', sistema exibe modal"
✅ "Quando email inválido, sistema exibe 'Email inválido' abaixo do campo"
❌ "O sistema deve suportar login"
```

## Integração com Outras Ferramentas

Esta skill foi projetada para ser complementada futuramente com:
- MCP Atlassian/Jira (criação direta de issues)
- Outros MCPs conforme necessário

Por enquanto, foque em gerar as especificações em markdown.

## Próximos Passos

Após criar uma especificação:
1. Apresente o arquivo ao usuário
2. Pergunte se deseja fazer ajustes
3. Sugira próximos passos lógicos:
   - PRD criado → Criar FRDs ou Épicos
   - Épico criado → Criar Histórias
   - FRD criado → Criar Histórias

