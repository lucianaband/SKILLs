---
trigger: always_on
---

# Regras de Especificação de Produto

## Contexto de Uso - LLM

Esta skill foi adaptada para uso em LLM (Claude). As principais diferenças em relação às versões IDE:

- **Criação de arquivos**: Claude cria arquivos em `/home/claude/` e os apresenta ao usuário via `present_files`
- **Variáveis de ambiente**: Adaptadas para contexto LLM - não há filesystem fixo do projeto
- **Templates e workflows**: Mantidos intactos em `workflows/product/` e `templates/product/`
- **Nomenclatura**: Convenções de nomes de arquivos preservadas

Quando os workflows mencionarem variáveis como `$DOCS_FOLDER_PROD`, entenda que deve criar o arquivo localmente e apresentá-lo ao usuário.

## Principais Regras
- O idioma padrão é o português do Brasil. Mas mude caso o usuário solicite outro idioma.
- Sempre siga as intruções de criação de especificação de produto e outras ações relacionadas na íntegra, seguindo os templates assim como eles estão. Nunca assuma nada, criando novos blocos ou informações sem que seja definido ou aprovado pelo usuário.
- Nunca invente dados ou informações, se não souber, não assuma nada, pergunte para o usuário mais informações.
- Sempre respeite o nível das especificações para escrever critérios de aceite coerentes com o nível da especificação.
- Se o usuário pedir para refazer completamente uma issue (história, tarefa, bug etc) ou uma PRD, sempre siga as instruções à risca.
- Sempre considere as informações fornecidas pelo usuário quando ele utilizar qualquer comando no chat para executar um agente de IA ou LLM. Se ele não fornecer informações adicionais, use as que já estão presentes.
- Se o usuário fornecer um arquivo ou contexto adicional, utilize essas informações para enriquecer a sua execução.

## Variáveis reutilizáveis de ambiente

**Nota importante para LLM**: As variáveis abaixo são referências conceituais dos workflows originais. No contexto do Claude:
- Ignore caminhos absolutos de filesystem
- Crie arquivos seguindo as convenções de nomenclatura indicadas
- Use `present_files` para disponibilizar ao usuário

**Variáveis de referência** (mantidas para compatibilidade com workflows):

```
# Referências conceituais - não são paths reais no Claude
TEMPLATES_FOLDER=templates
TEMPLATES_FOLDER_PROD=templates/product
WORKFLOWS_FOLDER=workflows
WORKFLOWS_FOLDER_PROD=workflows/product
RULES_FOLDER=rules
```

**Como usar no contexto LLM:**
- Quando workflow mencionar `/templates/product/prod-prd-template.md` → Acesse `templates/product/prod-prd-template.md` dentro da skill
- Quando workflow mencionar salvar em `$DOCS_FOLDER_PROD/prd-001-nome-prd.md` → Crie `prd-001-nome-prd.md` e apresente ao usuário
- Mantenha convenções de nomenclatura dos workflows originais

## Arquivos de instruções e comandos

**Para uso em LLM**: Acesse os workflows diretamente da estrutura da skill em `workflows/product/`.

Sempre siga as instruções de acordo com as relações abaixo:
- `workflows/product/prod.spec.md` serve para iniciar a construção das especificações a partir de opções
- `workflows/product/prod.spec.prd.md` para construir PRDs
- `workflows/product/prod.spec.frd.md` para criar arquivos de FRD que descreve funcionalidades
- `workflows/product/prod.spec.epic.md` para construir épicos
- `workflows/product/prod.spec.issue.md` para construir histórias e tarefas
- `workflows/product/prod.spec.clarify.md` para esclarecer uma especificação existente

Sempre atualize a documentação existente do projeto com as mudanças que forem feitas no projeto, ou seja, em cada atualização de feature, criação de novas features, novas especificações de produto ou técnicas, atualize as documentações existentes principalmente as PRDs e RFDs de forma a manter o projeto atualizado.

Siga sempre o formato markdown para fazer os arquivos finais.

## Princípio: Critérios de aceitação e requisitos

### Critérios de aceitação específicos do documento

#### Para PRDs (nível macro)
Concentre-se no "o quê" e no "porquê":
- Definir capacidades gerais do produto
- Definir regras e restrições de negócios
- Estabelecer métricas de sucesso

#### Para épicos (nível médio)
Ponte entre estratégia e implementação:
- Definir requisitos de nível de recurso
- Definir limites para histórias relacionadas
- Incluir pontos de integração
- Definir requisitos não funcionais

```
#### Obrigatório (P0)
1. **Autenticação do usuário**
   - Descrição: os usuários devem poder fazer login com segurança
   - Valor do usuário: protege os dados do usuário e permite a personalização
   - Critérios de Aceitação:
     - O sistema deve suportar autenticação de e-mail/senha
     - O sistema deve validar o formato do email
     - O sistema deve impor regras de complexidade de senha
     - O sistema deve fornecer funcionalidade de redefinição de senha
```

### Para histórias (detalhadas, funcionais)

Concentre-se em comportamentos específicos e testáveis com base em designs:

```
### Fluxo de autenticação
- Quando o usuário clica no botão "Login" na página inicial, o sistema exibe o modal de login
- Quando o usuário insere o email no campo email, o sistema valida o formato em tempo real
- Quando o usuário insere um formato de e-mail inválido, o sistema exibe o erro "Insira um e-mail válido" abaixo do campo
- Quando o usuário insere a senha e clica em "Enviar", o sistema tenta a autenticação
- Quando a autenticação é bem-sucedida, o sistema fecha o modal e redireciona para o painel
- Quando a autenticação falha, o sistema exibe o erro "Credenciais inválidas" acima do formulário
- Quando o usuário clica no link "Esqueci minha senha", o sistema exibe o modo de redefinição de senha

### Fluxo de redefinição de senha
- Quando o usuário insere o e-mail no formulário de redefinição e clica em "Enviar link de redefinição", o sistema envia o e-mail de redefinição
- Quando o sistema envia um e-mail, o sistema exibe a confirmação "Verifique seu e-mail para obter o link de redefinição"
- Quando o usuário não recebe o e-mail em 1 minuto, o sistema mostra o botão "Reenviar e-mail"
```

**Observe a diferença:**
- PRD/Epics: "O sistema deve suportar autenticação" (o quê, por que)
- Histórias/Tasks: "Quando o usuário clica no botão 'Login'..." (específicas, testáveis, baseadas em design)

**Detalhamento Progressivo**:
1. **PRD (Porquê)**: "O sistema deve suportar autenticação do usuário para proteger os dados do usuário"
2. **Epic (What)**: "Implementar sistema de autenticação com e-mail/senha e login social"
3. **História (Como)**: "Como usuário, posso fazer login com meu e-mail e senha para poder acessar minha conta"

**Principais diferenças**:
- **PRD**: objetivos de negócios e requisitos de alto nível
- **Épico**: escopo em nível de recurso e abordagem técnica
- **História/Tarefa**: detalhes específicos de implementação e comportamento da UI

## Princípio: Contexto completo

**Princípio**: Cada documento deve ser compreensível por si só, sem exigir conhecimento tribal.

**O que isso significa**:
- Incluir todos os antecedentes necessários
- Definir abreviações e jargões na primeira utilização
- Link para documentos relacionados utilizando formato markdown
- Explique “por que” as decisões foram tomadas
- Documentar suposições
- Não presuma que todos estavam na reunião

**Por que é importante**:
As pessoas juntam-se em equipas, o contexto perde-se, as memórias desaparecem. Documentos independentes garantem que todos possam contribuir, independentemente de quando aderiram.

**Perguntas de validação**:
- Um novo membro da equipe poderia entender isso?
- Todas as abreviaturas estão definidas?
- O “porquê” está explicado?
- Os documentos relacionados estão vinculados?
- Isso faria sentido em 6 meses?

**Exemplos**:

✅ **Bom**:
- "Estamos criando notificações por e-mail porque 73% dos usuários relataram a falta de atualizações importantes (consulte o documento de pesquisa do usuário). Este recurso abordará a reclamação número 1 dos usuários nas pesquisas do terceiro trimestre."
- "A latência da API (Interface de Programação de Aplicativo) deve ser <200 ms porque nosso SLA (Acordo de Nível de Serviço) se compromete com tempos de resposta inferiores a um segundo."

❌ **Ruim**:
- "Pela reunião, estamos fazendo notificações"
- "Porque Bob disse isso"
- "Todo mundo sabe porque isso é importante"
- Referências a discussões sem links ou resumos
- Decisões técnicas inexplicáveis

**Aplicação a Fluxos de Trabalho**:
- **PRDs**: inclua o contexto do problema, por que agora, iniciativas relacionadas
- **Planos**: consulte o PRD, explique as decisões de faseamento
- **Histórias**: Inclui plano de fundo, ajuste-se ao contexto épico
- **Histórias rápidas**: forneça contexto suficiente para executar sem fazer perguntas
- **Bugs rápidos**: ambiente do documento, etapas, comportamento esperado
- **Tarefas rápidas**: explique por que esse trabalho é importante

## Princípio: Terminologia Consistente

**Princípio**: Use os mesmos termos para os mesmos conceitos em toda a documentação.

**O que isso significa**:
- Escolha um termo e cumpra-o
- Crie um glossário para os principais conceitos
- Não use sinônimos para entidades principais
- Alinhe a terminologia entre PRD → Plano → Histórias
- Use termos padrão do setor quando aplicável

**Por que é importante**:
A terminologia inconsistente cria confusão. "Cliente" vs "Usuário" vs "Cliente" - são iguais? Diferente? Ninguém sabe, então todo mundo perde tempo esclarecendo.

**Perguntas de validação**:
- Estamos usando o mesmo termo que usamos no PRD?
- Temos vários termos para a mesma coisa?
- Um novo membro da equipe ficaria confuso?
- Existe um glossário se os termos forem complexos?

**Exemplos**:

✅ **Bom**:
- **Consistente**: Sempre use "espaço de trabalho" (às vezes não "espaço", "área", "sala")
- **Glossário**: "Espaço de trabalho: uma área colaborativa onde os membros da equipe compartilham documentos"
- **Termos padrão**: use termos do setor como "API", "SaaS", "MRR"

❌ **Ruim**:
- **Inconsistente**: “espaço de trabalho” no PRD, “espaço” no Plano, “sala” nas histórias
- **Termos inventados**: "doohickey" em vez do "widget" padrão
- **Jargão indefinido**: uso de abreviações específicas da empresa sem definição

**Aplicação a Fluxos de Trabalho**:
- **PRDs**: definir termos-chave no glossário
- **Planos**: use os termos exatos do PRD
- **Histórias**: Combine a terminologia do PRD e do Plano
- **Questões rápidas**: use terminologia estabelecida ou defina novos termos




## Fortalecimento de princípios

### Como os princípios são aplicados

**Durante a Criação**:
- A habilidade verifica ativamente violações de princípios
- Elementos ausentes geram perguntas ao usuário
- Os modelos incluem lembretes principais

**Durante a revisão**:
- A lista de verificação inclui validação da constituição
- Analisar princípios de verificações cruzadas de fluxo de trabalho
- A não conformidade é sinalizada antes da finalização

**Mensagens de erro**:

Se o princípio for violado, a habilidade responde:

**Princípio: Valor do Usuário**:
```
⚠️ Violação da Constituição: valor do usuário em primeiro lugar

Esta [história/requisito/épico] não articula claramente o valor do usuário.

Atual: "[texto atual]"

Obrigatório: Explicar os benefícios da OMS e COMO a sua vida/trabalho melhora.

Pergunta: Como os usuários se beneficiarão especificamente com isso?
```

**Princípio: Critérios de aceitação e requisitos**:
```
⚠️ Violação da Constituição: Critérios Testáveis

Este critério de aceitação usa linguagem vaga: "[termo vago]"

Termos vagos como “rápido”, “fácil”, “intuitivo” não são testáveis.

Obrigatório: Use critérios específicos e mensuráveis.

Exemplo:
❌ "O sistema deve ser rápido"
✅ "Tempo de resposta da API < 200 ms para o percentil 95"

Você pode fornecer um critério específico e mensurável?
```

**Princípio: Contexto Completo**:

```
⚠️ Violação da Constituição: Contexto Completo

Este documento faz referência a "[termo/abreviatura indefinido]" sem definição.

Obrigatório: Defina todas as abreviações e forneça contexto.

Pergunta: Você pode explicar o que significa "[termo]"?
```

**Princípio: Terminologia Consistente**:
```
⚠️ Violação da Constituição: Terminologia Consistente

Você está usando "[term1]" aqui, mas usou "[term2]" em [documento anterior].

São o mesmo conceito?

Obrigatório: Use terminologia consistente em todos os documentos.

Pergunta: Qual termo devemos usar consistentemente?
```