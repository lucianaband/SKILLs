---
name: prod.spec.frd
description: Fluxo para criação de FRD (Feature Requirements Document), que auxilia na definição detalhada dos requisitos funcionais de uma feature
auto_execution_mode: 3
env_file: "@/ENV.md"
---

# FRD - Functional Requirement Document

Antes de iniciar, revise o arquivo de regras invioláveis em `.windsurf/rules/product/global-rules.md`.

Este fluxo auxilia na criação de novos Functional Requirements Document (FRD) ou na modificação de FRDs existentes, que detalha os requisitos funcionais de uma funcionalidade específica do produto.

Quando usuário pedir para criar ou modificar uma FRD (Functional Requirement Document), você deve fazer seguindo as instruções em `$FLOWS_FOLDER_PROD/prod.spec.frd.md`. O arquivo final deve ser um arquivo de FRD, seguindo o formato, estrutura e estilo do template `/templates/product/prod-frd-template.md`.

A FRD não é um épico, história ou task: ele serve como documento de detalhamento que descreve profundamente os requisitos funcionais e não funcionais da feature, atuando como uma ponte entre a especificação de produto e o código. Ela unifica requisitos e critérios da funcionalidade do ponto de vista de produto, usuário, design e técnico. 

- Poderá existir vários FRDs contemplando as principais features do produto, mas cada FRD deve ter um nome único e um ID único.
- Dentro das FRDs devem ter as descrições micro de ações e sub-funcionalidades.
- Uma FRD descreve a funcionalidade maior, que pode ter várias sub-funcionalidades.
- A FRD precisa descrever o comportamento do usuário e do sistema de forma detalhada, agrupando micro-ações e jobs to be done do usuário. 

## Quando usar
- Quando é necessário documentar requisitos funcionais detalhados para uma feature específica
- Antes do desenvolvimento para garantir clareza sobre o que deve ser construído
- Para alinhar equipes técnicas e de produto sobre os requisitos esperados
- Quando há necessidade de criar um documento detalhado que servirá como base para o desenvolvimento
- Para garantir que todos os stakeholders tenham uma compreensão comum dos requisitos da feature
- Para facilitar a comunicação entre equipes de desenvolvimento, QA e stakeholders
- Para documentar critérios de aceitação claros e testáveis
- Para descrever o funcionamento detalhado da feature a partir do comportamento do usuário a partir de especificações técnicas, especificações de Produto e também de design

## Instruções

Antes de iniciar: 
- procure `$DOCS_FOLDER/**/*` para entender o contexto do projeto e encontrar outras especificações, principalmente PRDs, ARDs e FRDs.
- Se houverem PRDs, liste as PRDs existentes do projeto para que o usuário possa escolher qual o PRD relacionado.
- Se não houverem PRDs ou o usuário não fornecer, pergunte se o usuário quer criar uma PRD nova ou continuar sem PRD.

- Faça perguntas guiadas e faseadas, para obter informações suficientes e detalhadas para preencher o template `/templates/product/prod-frd-template.md` (siga a estrutura de perguntas que estão listadas no arquivo `rules/product/global-rules.md`)
- Sempre pergunte em partes, nunca faça várias perguntas de uma vez.

## Sobre arquivo final
- **Template Obrigatório**: Sempre use o template em `/templates/product/prod-frd-template.md` para fazer o arquivo final.
- **Local de Saída**: Salve o FRD em `$DOCS_FOLDER_PROD/frd-{ID}-{frd-name-based-in-frd-content}.md`
  - Exemplo ideal de nome: `frd-001-gestao-de-oficinas.md`
- Ao preencher o bloco JORNADA DE USUÁRIO DO TEMPLATE, pergunte para o usuário se ele design de layouts, imagens, diagramas, links e qualquer informação ou material que possa ajudar a compreender a a jornada.
- Se for um novo FRD, antes de finalizar, rode as instruções de `FLOWS_FOLDER_PROD/prod.spec.clarify.md` para entender gaps e melhorias no documento.
- Mostre a sugestão do conteúdo final para aprovação do usuário e pergunte se há algo que deva ser modificado.
- Se o usuário aprovar, crie ou salve o arquivo final.
- Atualize a PRD relacionada com as informações necessárias (se for uma nova FRD e não houver link, adicione o link)
- Se não aprovar, aguarde instruções e faça as alterações necessárias.
- A FRD deve ser salva no diretório `$DOCS_FOLDER_PROD/frds/` 

## O que evitar e não fazer?

Elaborar requisitos funcionais é uma etapa crucial no processo de desenvolvimento de produtos. No entanto, mesmo equipes experientes podem cair em armadilhas comuns, resultando em falhas de comunicação, atrasos e erros custosos. Para ajudar você a evitar esses problemas, veja alguns erros típicos, seus impactos potenciais e exemplos práticos.


### 1. Escrever Requisitos Vagamente ou com Ambiguidade
Um dos erros mais comuns é não redigir requisitos de forma clara e específica. A ambiguidade pode causar confusão entre desenvolvedores, testadores e stakeholders, resultando em implementações inconsistentes.


**Exemplo de Erro**  
"Requisito: O sistema deve processar solicitações rapidamente."

**Impacto:** Sem um padrão mensurável, “rapidamente” pode significar segundos para uma pessoa e minutos para outra, gerando expectativas não atendidas.

**Como Evitar**  
Seja preciso. Reformule o requisito como:  
"O sistema deve processar solicitações e entregar uma resposta em até **0,3 segundos**."

***

### 2. Combinar Múltiplos Requisitos em um Só

Sobrecarregar uma única declaração de requisito pode dificultar sua implementação, compreensão ou testes adequados.

**Exemplo de Erro**  
"Requisito: O sistema deve validar as credenciais do usuário, exibir uma mensagem de boas-vindas e enviar um e-mail de verificação."

**Impacto:** Essa afirmação cobre várias ações distintas, tornando difícil o rastreamento e a testagem.

**Como Evitar**  
Divida em vários requisitos:  
- O sistema deve validar as credenciais de login do usuário.  
- Se o login for bem-sucedido, o sistema deve exibir uma mensagem de boas-vindas.  
- O sistema deve enviar um e-mail de verificação após o registro bem-sucedido.

***

### 3. Misturar Requisitos Funcionais e Não Funcionais

Outro erro frequente é combinar requisitos funcionais (o que o sistema faz) com requisitos não funcionais (como o sistema se comporta).

**Exemplo de Erro**  
"Requisito: O sistema deve processar até 1.000 transações por segundo e garantir a segurança dos dados do usuário."

**Impacto:** Requisitos de desempenho (não funcionais) e medidas de segurança (funcionais) são combinados, complicando o design e a verificação.

**Como Evitar**  
Separe em requisitos distintos:  

**Requisito funcional:**  
O sistema deve garantir que os dados do usuário sejam criptografados durante a transmissão.  

**Requisito não funcional:**  
O sistema deve processar até 1.000 transações por segundo.  


### 4. Ignorar a Testabilidade

Requisitos que não podem ser verificados geram ineficiências no processo de desenvolvimento e teste.

**Exemplo de Erro**  
"Requisito: O sistema deve oferecer uma experiência de usuário excepcional."

**Impacto:** “Experiência excepcional” é subjetiva e não pode ser testada diretamente.

**Como Evitar**  
Defina critérios mensuráveis:  
O sistema deve atingir uma pontuação de **90 ou mais** em testes de usabilidade padronizados.

### 5. Excesso de Jargões Técnicos

O uso excessivo de linguagem técnica ou muito específica pode afastar stakeholders não técnicos e criar barreiras de colaboração.

**Exemplo de Erro**  
"Requisito: O backend da aplicação deve suportar enfileiramento assíncrono de mensagens através de uma interface compatível com JMS."

**Impacto:** Essa formulação pode confundir stakeholders que não conhecem esses termos, atrasando aprovações.

**Como Evitar**  
Use linguagem clara e direta sem perder a precisão técnica:  
O sistema deve permitir o enfileiramento de mensagens com suporte a processamento assíncrono.

### 6. Não Incluir a Justificativa

Quando o propósito de um requisito não é claro, isso abre espaço para interpretações erradas ou dificuldades na implementação.

**Exemplo de Erro**  
"Requisito: O sistema deve registrar todas as ações do usuário."

**Impacto:** Os desenvolvedores podem implementar logs extensivos que afetam o desempenho, já que o motivo da exigência não foi explicado.

**Como Evitar**  
Inclua a justificativa:  
O sistema deve registrar todas as ações do usuário para auditoria de segurança e conformidade.

### 7. Não Revisar Regularmente os Requisitos

Requisitos que não são revisados e atualizados ao longo do ciclo do projeto correm o risco de se tornarem obsoletos ou incompletos.

**Exemplo de Erro**  
Não considerar alterações regulatórias que exigem ajustes nos requisitos existentes.  

**Impacto:** Isso pode gerar retrabalho caro se as atualizações forem necessárias tardiamente no desenvolvimento.

**Como Evitar**  
Agende revisões regulares com os principais stakeholders para garantir que os requisitos continuem relevantes e alinhados aos objetivos do projeto.

### 8. Superengenheirar os Requisitos

Incluir detalhes excessivos ou casos extremos nos requisitos pode sobrecarregar o desenvolvimento e prolongar prazos desnecessariamente.

**Exemplo de Erro**  
"Requisito: O sistema deve exibir 20 estilos de fonte diferentes para cada campo de texto personalizável."

**Impacto:** Requisitos complexos demais podem desviar recursos de funcionalidades críticas e comprometer os prazos.

**Como Evitar**  
Concentre-se nas necessidades principais e colete feedback dos usuários para determinar o escopo adequado.
