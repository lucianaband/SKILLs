# Exemplos de Uso

## Exemplo 1: Criando um PRD

### Comando do Usuário
```
Crie um PRD para um sistema de notificações push no aplicativo mobile
```

### Fluxo Esperado

1. Claude identifica que precisa usar `workflows/product/prod.spec.prd.md`
2. Carrega `rules/global-rules.md` para princípios
3. Usa `templates/product/prod-prd-template.md` como base
4. Faz perguntas essenciais:
   - Por que resolver: Quais problemas das notificações atuais?
   - O que resolver: Objetivos do novo sistema?
   - Como resolver: Abordagem proposta?
5. Cria arquivo `prd-001-notificacoes-push.md`
6. Apresenta ao usuário via `present_files`

### Resultado
Arquivo markdown estruturado seguindo template completo.

---

## Exemplo 2: Criando História de Usuário

### Comando do Usuário
```
Crie uma história para o fluxo de login com Google
```

### Fluxo Esperado

1. Claude usa `workflows/product/prod.spec.issue.md`
2. Pergunta sobre contexto e assets de design
3. Usa `templates/product/prod-issue-template.md`
4. Cria critérios de aceitação detalhados:
   ```
   #### Fluxo de Login com Google
   - Quando usuário clica em "Login com Google", sistema abre modal OAuth
   - Quando autenticação Google sucede, sistema cria ou vincula conta
   - Quando falha, sistema exibe erro específico
   ```
5. Gera arquivo `login-google-story001.md`

---

## Exemplo 3: Criando Épico

### Comando do Usuário
```
Crie um épico para o módulo completo de pagamentos
```

### Fluxo Esperado

1. Claude usa `workflows/product/prod.spec.epic.md`
2. Pergunta se existe PRD relacionado
3. Define contexto e critérios de alto nível
4. Lista histórias relacionadas sugeridas
5. Usa `templates/product/prod-epic-template.md`
6. Gera `epic-001-modulo-pagamentos.md`

### Estrutura Gerada
```markdown
# EPIC-001: Módulo de Pagamentos

## Contexto
[Descrição do problema e importância]

## Critérios de Aceitação
- Sistema deve processar pagamentos com cartão
- Sistema deve validar dados antes de processar
- Sistema deve registrar transações com auditoria

## Histórias Relacionadas
**STORY-001**: Integração com gateway de pagamento
**STORY-002**: Validação de cartão de crédito
**STORY-003**: Dashboard de transações
```

---

## Exemplo 4: Criando FRD

### Comando do Usuário
```
Crie um FRD para a funcionalidade de busca avançada
```

### Fluxo Esperado

1. Claude usa `workflows/product/prod.spec.frd.md`
2. Pergunta sobre PRD relacionado
3. Solicita informações sobre jornada do usuário
4. Define requisitos detalhados com critérios técnicos
5. Usa `templates/product/prod-frd-template.md`
6. Gera `frd-001-busca-avancada.md`

### Requisitos Exemplo
```markdown
### RQT-1: Filtros de Busca
- Quando usuário seleciona categoria, sistema filtra resultados em tempo real
  - Utilizar endpoint GET /api/search com parâmetro category
  - Cache de resultados por 5 minutos
- Quando usuário aplica múltiplos filtros, sistema combina com operador AND
- Quando nenhum resultado encontrado, sistema exibe sugestões alternativas
```

---

## Exemplo 5: Esclarecendo Especificação

### Comando do Usuário
```
Esclareça o PRD de notificações - há ambiguidades sobre frequência e priorização
```

### Fluxo Esperado

1. Claude usa `workflows/product/prod.spec.clarify.md`
2. Carrega PRD existente fornecido pelo usuário
3. Analisa categorias de ambiguidade
4. Faz perguntas direcionadas (máx 5):
   ```
   **Recomendado**: Limitar 10 notificações por hora por usuário
   
   | Opção | Descrição |
   |-------|-----------|
   | A | Limite de 10/hora |
   | B | Limite de 5/hora |
   | C | Sem limite |
   
   Você pode responder com a letra ou aceitar a recomendação dizendo "sim"
   ```
5. Atualiza PRD com esclarecimentos na seção `## Clarifications`
6. Retorna PRD atualizado

---

## Exemplo 6: Fluxo Completo - Do PRD às Histórias

### 1. Criar PRD
```
Crie um PRD para sistema de avaliações de produtos
```

### 2. Criar FRD
```
Baseado no PRD, crie um FRD para a funcionalidade de deixar avaliação
```

### 3. Criar Épico
```
Crie um épico agrupando as features de avaliação
```

### 4. Criar Histórias
```
Crie 3 histórias para o épico de avaliações:
1. Formulário de avaliação
2. Exibição de avaliações
3. Moderação de conteúdo
```

### Resultado
- 1 PRD completo
- 1 FRD detalhado
- 1 Épico estruturado
- 3 Histórias prontas para desenvolvimento
- Todos interligados por referências

---

## Dicas de Uso

### Para Melhores Resultados

1. **Seja específico** no comando inicial
   - ❌ "Crie um PRD"
   - ✅ "Crie um PRD para chat em tempo real no app"

2. **Forneça contexto** quando disponível
   - Mencione se já existe PRD relacionado
   - Indique se há designs prontos
   - Informe prioridade ou deadline se relevante

3. **Itere quando necessário**
   - Primeiro gere a versão inicial
   - Depois peça refinamentos: "Adicione seção sobre escalabilidade"

4. **Use arquivos anteriores como referência**
   - "Baseado no PRD-001, crie um épico para a feature X"
   - "Continue a numeração das histórias do épico anterior"

### Comandos Úteis

```
# Criar nova especificação
"Crie um [PRD/Epic/FRD/História] para [descrição]"

# Editar existente
"Edite o PRD adicionando métricas de sucesso"
"Atualize a história 003 com novos critérios"

# Esclarecer
"Esclareça ambiguidades no PRD sobre segurança"

# Relacionar documentos
"Baseado no PRD-001, crie FRD para feature de busca"
```

---

## Anti-Padrões (Evite)

### ❌ Comando Vago
```
"Crie uma coisa para o projeto"
```
**Problema**: Claude precisa fazer muitas perguntas

### ❌ Modificar Template
```
"Crie um PRD mas sem a seção de métricas"
```
**Problema**: Quebra padronização

### ❌ Inventar Dados
```
"Crie um PRD e invente as métricas de sucesso"
```
**Problema**: Dados não validados

### ✅ Comando Correto
```
"Crie um PRD para sistema de notificações. Ainda não temos métricas definidas, pode deixar como TODO?"
```
**Resultado**: Documento estruturado com placeholders claros

---

## Integração Futura com Jira

### Planejado (em desenvolvimento)

```
# Criar história e já criar issue no Jira
"Crie uma história para login OAuth e crie no Jira no projeto XPTO"

# Atualizar issue existente
"Baseado neste PRD, atualize a descrição da issue ISSUE-123"
```

Por enquanto: Gere o markdown e copie manualmente para Jira ou use Atlassian MCP separadamente.
