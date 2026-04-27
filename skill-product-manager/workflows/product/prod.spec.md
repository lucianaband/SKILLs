---
name: prod.spec
description: Guia o usuário para construir especificações macro ou micro de produto, como PRDs, histórias, tarefas, RFDs e outros.
auto_execution_mode: 3
env_file: "@/ENV.md"
---

# Iniciar uma especificação

Estas instruções ajudam a orientar o usuário que deseja começar a construir uma especificação. Esta especificação pode variar desde um PRD, história de usuário até tarefas. Devemos orientá-lo nessa construção de forma que a especificação, seja ela macro ou micro seja criada de acordo com as instruções e padrões do projeto.

## Quando usar
- Esse comando serve para unificar a criação de todos os tipos de especificações do projeto.
- Ao iniciar uma nova especificação de projeto ou modificar um PRD existente
- Quando não souber qual tipo de especificação criar
- Quando precisar criar novas ou editar FRDs
- Quando precisar criar novos ou editar épicos, issues, tasks, stories e bugs
- Quando precisar de orientação sobre o tipo de documentação a ser gerada

## Instruções

- **Regras do projeto**: Sempre revise os arquivos de regras invioláveis em `.windsurf/rules/product/global-rules.md`.
- **Projeto existente ou novo**: entenda se o projeto é novo ou se já é um projeto em andamento. Se não tiver certeza, pergunte para o usuário. Para ter certeza, veja se há alguma pasta docs, `$DOCS_FOLDER`.
  - Se for um projeto existente:
    - Leia e analise as documentações do projeto em `$DOCS_FOLDER` se existirem (incluindo sub-pastas)
    - Leia os últimos 5 pushs no repositório para entender as mudanças recentes
    - PRDs, FRDs e ARDs são importantes para entender as bases, motivações e decisões tomadas
    - Guarde as referências de PRDs e seus FRDs para ter em mãos quando usuário precisar criar FRDs e outras especificações.
  - Se for um novo projeto:
    - Avance para iniciar a PRD do projeto com as instruções em `$FLOWS_FOLDER_PROD/prod.spec.prd.md` 
- Utilize as informações que o usuário forneceu e o contexto atual do projeto para direcionar a criação ou modificação de uma especificação. 
- Pergunte para o usuário qual documentação ele deseja criar:
  - PRD
  - FRD
  - ARD
  - Issue (story, task ou bug)
- **Uso adequado dos comandos e instruções**: 
  - Para PRD use as instruções em `$FLOWS_FOLDER_PROD/prod.spec.prd.md`
  - Para FRD use as instruções em `$FLOWS_FOLDER_PROD/prod.spec.frd.md`
  - Para ARD use as instruções em `$FLOWS_FOLDER_PROD/prod.spec.ard.md`
  - Para Issue use as instruções em `$FLOWS_FOLDER_PROD/prod.spec.issue.md`
  - Para clarificar e revisar qualquer tipo de especificação, use as instruções em `$FLOWS_FOLDER_PROD/prod.spec.clarify.md`
  - Para criar épicos, use as instruções em `$FLOWS_FOLDER_PROD/prod.spec.epic.md`
- **Idioma**: Mantenha o mesmo idioma da interação com o usuário, sendo que o idioma padrão é português brasileiro
- **Faça suposições informadas**: Use o contexto, padrões de mercado e padrões comuns para preencher os gaps que a documentação ou a falta de informação não estiver cobrindo
- **Não seja verboso**: Evite fazer muitas perguntas para o usuário ao criar ou atualizar uma especificação. Seja direto e objetivo. Se perguntar, faça perguntas estratégicas e suficientes para que o usuário dê informações relevantes para a criação ou modificação com eficácia.
- **Perguntas para direcionar o usuário**: Use perguntas estratégicas para entender o escopo, objetivo e contexto da especificação que o usuário deseja criar ou modificar, usando a estrutura padrão de perguntas abaixo:

```
Aqui fica a pergunta que você deve fazer para o usuário. Seja objetivo e direto ao ponto:
A: Resposta 1
B: Resposta 2
C: Resposta 3
D: etc etc etc
```

- Se o usuário fornecer informações junto com a resposta da questão, utilize-as para enriquecer o seu contexto.
- Sempre faça perguntas em mensagens distintas, para não complicar o fluxo de conversa. 
- Só faça as perguntas necessárias para entender o contexto e o escopo do produto. 
- Evite fazer perguntas redundantes ou que não agreguem valor à especificação. 
- Se o usuário não responder uma pergunta, não repita a mesma pergunta novamente, mas continue com o fluxo com as informações disponíveis. 
- Se o usuário responder com uma resposta que não se encaixa nas opções, pergunte novamente de forma clara e objetiva, mas guarde a informação que ele forneceu.
