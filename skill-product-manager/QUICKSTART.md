# Guia Rápido

## 🚀 Início Rápido (2 minutos)

### 1. Instalação
Faça upload da pasta `skill-product-manager` completa no Claude.

### 2. Primeiro Uso
```
Usando a skill Product Manager, crie um PRD para [seu projeto]
```

### 3. Pronto! 
Claude seguirá os workflows automaticamente.

---

## 📋 Checklist de Verificação

Após upload, verifique se a estrutura está completa:

```
skill-product-manager/
├── ✅ SKILL.md               # Arquivo principal
├── ✅ README.md              # Documentação
├── ✅ EXAMPLES.md            # Exemplos de uso
├── ✅ workflows/product/     # 6 workflows
├── ✅ templates/product/     # 6 templates
└── ✅ rules/                 # 1 arquivo de regras
```

---

## 💬 Comandos Essenciais

### Criar Especificações

```bash
# PRD (Product Requirements Document)
"Crie um PRD para sistema de chat em tempo real"

# Épico
"Crie um épico para módulo de pagamentos"

# História de Usuário
"Crie uma história para login com biometria"

# FRD (Feature Requirements Document)  
"Crie um FRD para busca avançada com filtros"

# Esclarecer
"Esclareça o PRD sobre requisitos de segurança"
```

### Editar Especificações

```bash
"Edite o PRD adicionando seção de métricas"
"Atualize a história 003 com novos critérios"
```

---

## 🎯 Fluxo Recomendado

### Para Novo Feature/Produto

1. **PRD** → Visão geral e estratégia
2. **FRD** → Detalhamento de funcionalidades
3. **Épico** → Agrupamento de entregas
4. **Histórias** → Implementação granular

### Exemplo Prático

```
# Passo 1
"Crie um PRD para marketplace de serviços"

# Passo 2  
"Baseado no PRD, crie FRD para sistema de busca"

# Passo 3
"Crie épico de onboarding de prestadores"

# Passo 4
"Crie 3 histórias para o épico de onboarding"
```

---

## ⚙️ Configuração Opcional

### Personalizar Nomenclatura

Por padrão, arquivos são nomeados como:
- PRD: `prd-001.md`
- Epic: `epic-001-nome.md`
- Story: `validacao-story-001.md`

Para personalizar, mencione no comando:
```
"Crie um PRD chamado 'marketplace-v2-prd.md'"
```

### Integração com Jira (Futuro)

Em desenvolvimento. Por enquanto:
1. Gere o markdown com a skill
2. Copie e cole no Jira
3. Ou use Atlassian MCP separadamente

---

## 🔍 Troubleshooting

### Claude não está usando a skill?

**Solução**: Seja explícito no comando
```
❌ "Crie um PRD"
✅ "Usando a skill Product Manager, crie um PRD para..."
```

### Preciso de seção adicional no template?

**Solução**: Confirme com usuário antes
```
"Quero adicionar seção de Custos ao PRD. Podemos incluir?"
```

### Template está incompleto?

**Solução**: Templates estão em `templates/product/`
- Verifique se upload foi completo
- Consulte README.md para estrutura esperada

---

## 📚 Documentação Completa

- **SKILL.md** - Detalhes técnicos da skill
- **README.md** - Documentação completa
- **EXAMPLES.md** - Casos de uso e exemplos
- **rules/global-rules.md** - Regras e princípios

---

## 🆘 Suporte

### Dúvidas Comuns

**P: Posso modificar os templates?**
R: Sim, mas mantenha estrutura base para consistência.

**P: Como atualizar skill com novas versões?**
R: Substitua arquivos mantendo estrutura de pastas.

**P: Funciona com outros idiomas?**
R: Sim, mencione o idioma desejado no comando.

**P: Preciso seguir ordem PRD → Epic → Story?**
R: Não é obrigatório, mas é recomendado para contexto.

### Problemas?

1. Verifique estrutura de arquivos
2. Confirme que SKILL.md está presente
3. Tente comando explícito mencionando a skill
4. Consulte EXAMPLES.md para padrões

---

## 🔄 Atualizações

### Versão Atual: 1.0.0

**Próximas Features**:
- ✨ Integração direta com Jira via MCP
- ✨ Templates customizáveis por projeto
- ✨ Análise automática de qualidade de specs
- ✨ Sugestões baseadas em histórico

### Changelog

**v1.0.0** (Janeiro 2025)
- ✅ Primeira versão adaptada para LLM
- ✅ Workflows completos (PRD, Epic, Story, FRD, Clarify)
- ✅ Templates preservados da versão IDE
- ✅ Documentação completa
- ✅ Exemplos de uso

---

## ✅ Checklist de Primeiro Uso

- [ ] Upload da skill completo
- [ ] SKILL.md presente
- [ ] Testar comando: "Usando skill Product Manager, crie um PRD para teste"
- [ ] Verificar arquivo gerado
- [ ] Consultar EXAMPLES.md para casos de uso
- [ ] Pronto para usar em projetos reais!

---

**Pronto para começar? Teste agora:**

```
Usando a skill Product Manager, crie um PRD para [seu primeiro projeto]
```
