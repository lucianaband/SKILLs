---
name: prod.spec.prd
description: Alterar ou criar um especificação de produto PRD - Product Requirement Document seguindo o template e as melhores prática de mercado, de forma que possa ser utilizado e expandido por agentes de IA e humanos.
auto_execution_mode: 3
env_file: "@/ENV.md"
---

# PRD - Product Requirement Document

Antes de iniciar, revise o arquivo de regras invioláveis em `.windsurf/rules/product/global-rules.md`.

Este fluxo de trabalho orienta o usuário com perguntas para criar ou modificar especificações como um Documento de Requisitos do Produto (PRD). A PRD é usada como base para criar/alterar issues do projeto, que são histórias, tarefas e bugs. Além disso, a PRD orienta o desenvolvimento do código e suas evoluções.

Use as informações fornecidas pelo usuário como ponto de partida. Seu objetivo é criar, atualizar, modificar ou refinar requisitos de produto e técnicos. 

Quando usuário pedir para criar ou modificar uma PRD (Product Requirement Document), você deve fazer seguindo as instruções em `$FLOWS_FOLDER_PROD/prod.spec.prd.md`. O arquivo final deve ser um arquivo de PRD, seguindo o formato, estrutura e estilo do template `/templates/product/prod-prd-template.md`.

## Quando usar
- Ao iniciar uma nova especificação de projeto ou modificar um PRD existente
- Quando é necessária documentação abrangente
- Quando necessário fazer uma documetação ampla sobre a solução do produto conectado com o negócio
- Quando múltiplas partes interessadas estão envolvidas
- Como fonte única de verdade para o desenvolvimento do produto e base para derivar histórias e tarefas
- Quando são necessários requisitos de produto (PRD) para um novo recurso ou produto

## Instruções

- Se não tiver informações suficientes para montar o PRD, pergunte para o usuário.
- Se fizer as perguntas, faça perguntas estratégicas e suficientes para que o usuário dê informações relevantes para a criação ou modificação com eficácia, se baseando no template de PRD.
- Sempre pergunte em partes, nunca faça várias perguntas de uma vez.
- **Template Obrigatório**: Sempre use o template em `/templates/product/prod-prd-template.md` para fazer o arquivo final.
- **Local de Saída**: Salve o PRD em `$DOCS_FOLDER_PROD/prd-{ID}-{prd-name-based-in-prd-content}.md`
  - Exemplo ideal de nome: `prd-001-gestao-de-manutencao.md`

**Informações básicas para começar:**
- Pergunte para o usuário se ele tem imagens, diagramas, links e qualquer informação ou material que possa ajudar a compreender a solução que está construindo.
- Se o usuário não enviar nenhuma informação, de forma que não seja possível criar o documento, não assuma nada, e faça as perguntas abaixo sobre o TL:DR. Faça as perguntas separadas, não de uma vez:
  - Em um parágrafo, o que é a solução que está construindo?
  - O QUE resolver: Quais os problemas que estimularam essa solução? Para quem é essa solução? O que está acontecendo atualmente que precisa ser melhorado?
  - POR QUE resolver: Por que resolver esses problemas é importante? Por que explorar essas oportunidades é importante? Quais os benefícios e impactos positivos esperados?
  - COMO resolver: Qual solução será construída? Qual a nossa abordagem para resolver o problema ou explorar essa oportunidade? Quais possibilidades os usuários poderão executar?
- Se for um PRD para um projeto já existente, faça uma análise do projeto antes, leia a documentação existente, leia o 5 pushs anteriores para entender as últimas atualizações e só depois de analisar o contexto do projeto, sugira para o usuário um TL:DR; e só depois da confirmação dele, para passe para o resto da documentação. 
- A lista de FRDs é muito importante porque eles servem como base para mostrar as funcionalidades existentes e futuras, para criar as issues do projeto, que são histórias, tarefas e bugs.
- Se for um projeto novo, pergunte para o usuário quais são as principais funcionalidades da solução para montar o bloco de FRDs. Se o usuário enviar a lista de FRDs, sugira 7 funcionalidades que serão as FRDs. 


#### Não faça ou evite
- Faça perguntas de esclarecimento muito cedo quando o usuário não tiver fornecido informações suficientes. Apenas siga o modelo de PRD e o arquivo de regras. 
- Não seja verboso ou faça perguntas excessivas. Você pode supor algumas informações, mas com a confirmação do usuário.
- Escrever requisitos excessivamente detalhados (exceto para histórias)
- Não incluir métricas de sucesso
- Deixar de validar suposições
- Não atualizar o PRD quando os requisitos mudarem
- Criar requisitos sem a contribuição do usuário
