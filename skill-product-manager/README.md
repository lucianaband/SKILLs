# Skill Product Manager     

## Visão Geral

Skill para Product Managers criarem especificações de produto seguindo metodologia de Spec Driven Development adaptada para uso em LLM (Claude).

## Estrutura

```
skill-product-manager/
├── SKILL.md                          # Arquivo principal da skill
├── README.md                         # Este arquivo
├── workflows/
│   └── product/
│       ├── prod.spec.prd.md         # Workflow para criar PRDs
│       ├── prod.spec.issue.md       # Workflow para criar Histórias/Tasks/Bugs
│       ├── prod.spec.epic.md        # Workflow para criar Épicos
│       ├── prod.spec.frd.md         # Workflow para criar FRDs
│       ├── prod.spec.clarify.md     # Workflow para esclarecer specs
│       └── outros...
├── rules/
│   └── global-rules.md           # Regras invioláveis e princípios
└── templates/
    └── product/
        ├── prod-prd-template.md     # Template de PRD
        ├── prod-issue-template.md   # Template de Issue
        ├── prod-epic-template.md    # Template de Épico
        └── prod-frd-template.md     # Template de FRD
```

## Como Usar

### Instalação

Faça upload da pasta `skill-product-manager` completa para o Claude via interface web ou API.

### Comandos Naturais

Use comandos conversacionais como:

- "Crie um PRD sobre sistema de pagamentos"
- "Nova história para login do usuário"
- "Crie um épico de onboarding"
- "Novo FRD para autenticação"

Claude identificará automaticamente qual workflow usar.

## Adaptações para LLM

Esta skill foi adaptada das versões originais para IDE com as seguintes mudanças:

### O que mudou
- ✅ Removidos metadados específicos de IDE (`auto_execution_mode`, `env_file`)
- ✅ Variáveis de ambiente adaptadas (ex: `$DOCS_FOLDER` → caminhos relativos)
- ✅ Instruções de filesystem adaptadas para criação local + `present_files`
- ✅ Adicionadas notas explicativas sobre contexto LLM

### O que foi mantido
- ✅ 100% dos templates sem alteração
- ✅ Estrutura de pastas e nomenclatura original
- ✅ Conteúdo completo dos workflows
- ✅ Todas as regras e princípios
- ✅ Exemplos e boas práticas

## Sincronização com Versão IDE

A estrutura foi mantida propositalmente similar à versão IDE para facilitar:
- Atualizações futuras
- Portabilidade entre ambientes
- Consistência de processo

### Para atualizar da versão IDE:
1. Copie novos arquivos de `/workflows`, `/rules`, `/templates`
2. Execute script de adaptação de variáveis (sed)
3. Adicione notas LLM onde apropriado

## Convenções de Nomenclatura

### Arquivos Gerados

- **PRD**: `prd-{ID}.md` (ex: `prd-001.md`)
- **Epic**: `epic-{ID}-{nome}.md` (ex: `epic-001-pagamento.md`)
- **Issue**: `{epic}-{nome}-{id}.md` (ex: `pagamento-validacao-story123.md`)
- **FRD**: `frd-{ID}-{feature}.md` (ex: `frd-001-autenticacao.md`)

## Princípios Fundamentais

### Níveis de Especificação

**PRD (Macro)**
- O QUÊ e POR QUÊ
- Capacidades gerais
- Regras de negócio

**Épico (Médio)**
- Ponte estratégia-implementação
- Requisitos de feature
- Integrações

**História/Task (Micro)**
- Comportamentos específicos
- Baseados em design
- Formato: "Quando... o sistema..."

### Regras Essenciais

1. **Não inventar dados** - sempre perguntar
2. **Seguir templates** à risca
3. **Respeitar níveis** de especificação
4. **Manter consistência** terminológica
5. **Documentos auto-explicativos**

## Próximos Passos

### Funcionalidades Planejadas
- Integração com MCP Atlassian/Jira
- Integração com outros MCPs conforme necessário
- Evolução incremental baseada em feedback

### Suporte

Para dúvidas ou sugestões:
- Consulte `SKILL.md` para detalhes completos
- Veja `rules/global-rules.md` para regras detalhadas
- Examine workflows específicos em `workflows/product/`

## Versão

- **Versão**: 0.5.0
- **Data**: Janeiro 2025
- **Ambiente**: LLM (Claude)
