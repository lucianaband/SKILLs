---
id: {PRD-001}
name: {nome desse prd}
version: {X.Y.Z - atualizar sempre que houver alteração}
task_link: {URL referêncial no $TASK_MANAGER. Se não existir, remova essa linha}
created_at: {YYYY-MM-DD}
updated_at: {YYYY-MM-DD}
status: {icebox|in_review|in_progress|in_production|deprecated}
---

# {id}: {name}

## TL;DR

- **O que** resolver: 
  - {Declaração do problema 1: Qual é a questão central?}
  - {Declaração do problema 2: Quais são as dores existentes?}
  - {Declaração do problema 3: Qual oportunidade estamos abordando?}
- **Por que** resolver: 
  - {Impacto no negócio 1: Como isso beneficia o negócio?}
  - {Benefício do usuário 1: Como isso ajuda os usuários?}
  - {Valor estratégico: Por que isso é importante agora?}
- **Como** resolver: 
  - {Abordagem da solução 1: Conceito de alto nível da solução}
  - {Abordagem da solução 2: Principais funcionalidades/componentes}
  - {Diferencial: O que torna esta solução única?}

## Contexto

{Forneça 2-3 parágrafos (total máximo de 200 palavras) cobrindo:
1. Situação atual e pontos de dor
2. Impacto comercial de não resolver
3. Perspectiva e necessidades do usuário
4. Importância estratégica}

### Declaração do problema ou da oportunidade
{Escreva 2-3 parágrafos resumindo o problema ou a oportunidade que estamos abordando, do ponto de vista de produto. Depois, liste os 3-6 principais problemas específicos que este PRD aborda. Foque nas dores do usuário e no impacto no negócio.}

```
- {Questão específica e mensurável: Descreva claramente o problema ou oportunidade que impacta diretamente o usuário}
- {Outra questão específica: Descreva um segundo problema ou oportunidade que afeta a experiência do usuário}
- {Terceiro problema chave: Descreva um terceiro problema ou oportunidade que representa um bloqueio significativo}
```

## Solução
{Escreva uma frase curta, com impacto, mas sem jargões e outros exageros, de no máximo 280 caractéres, que resuma a solução. Ex.: Construíremos uma solução que [descreva a solução principal em uma frase clara e direta] para [descreva o público-alvo].}

{Visão geral de 2-3 parágrafos com no máximo 200 palavras no total, respondendo: O que estamos construindo? Por quê? Para quem? Impacto esperado? Descreva qual é a solução e por que é a melhor solução. Qual é o custo de oportunidade de não fazer isso? A descrição da nossa solução e como ela ajudará o usuário a resolver o problema e a empresa a explorar esta oportunidade.} 

*Listagem das funcionalidades que compõem o escopo da solução:*
{Se não houver FRDs criadas ou for um projeto novo, não crie esse bloco. Esse bloco deve ser criado caso houver FRDs criadas.}

- **[FRD-001](<path/to/frd/file.md>) ({status atual da FRD - se não existir, não inclua}):** [capacidade específica de médio/alto nível, por exemplo, "permitir que os usuários façam upload de arquivos médicos e analisem esses dados extraídos"]
- **[FRD-002](<path/to/frd/file.md>) ({status atual da FRD - se não existir, não inclua}):** [capacidade específica de médio/alto nível, por exemplo, "identificar padrões por meio de correlação de dados"]  
- **[FRD-003](<path/to/frd/file.md>) ({status atual da FRD - se não existir, não inclua}):** [interação chave, por exemplo, "cadastrar com email/senha, redes sociais e autenticação de dois fatores"]
- **[FRD-004](<path/to/frd/file.md>) ({status atual da FRD - se não existir, não inclua}):** [requisito de dados, por exemplo, "persistir preferências do usuário"]

{
Evite:
Esses dois pontos poderiam ser unificados, sendo parte de uma funcionalidade única:
- [FRD-001]: O sistema DEVE permitir que usuários façam upload de documentos médicos (PDFs, imagens) de forma simples e segura
- [FRD-002]: O sistema DEVE extrair automaticamente dados estruturados de documentos médicos (nome de exames, valores, datas, laboratórios)

Melhor seria:
- [FRD-001]: Permitir upload de arquivos e documentos médicos, permitindo extração automática de dados estruturados
}

### O que essa iniciativa não é

{Descreva em no máximo 150 palavras, o que essa iniciativa não é, o que nós não resolvemos por que isso faz parte de outro escopo de segmentos ou serviços que não nos propomos a trabalhar. Essa declaração é importante para evitar mal-entendidos e delimitar claramente o escopo. Valide com o usuário.

Exemplo:
Essa solução não é um sistema de adição de informação de saúde como Apple Health, Google Fit, Fitbit, Garmin e outros. Ele é um sistema de gestão de dados pessoais de saúde, que agrega informações de diferentes fontes e permite o usuário gerenciar e visualizar seus dados de saúde de forma centralizada. Não é um sistema de diagnóstico médico, nem um sistema de prescrição médica.}

### Resumo de solução técnica

{Se já houver especificações técnicas no projeto baseadas nas ARDs relacionadas, leia e crie um resumo de 2-3 parágrafos, com no máximo 100 palavras no total. E liste as principais decisões técnicas e de arquitetura no formato abaixo. Se não houver, esse bloco não deve ser incluído.}

```
- {[ARD-001](<path/to/file.md>): Descreva claramente a decisão técnica}
- {[ARD-002](<path/to/file.md>): Descreva uma segunda decisão técnica}
- {[ARD-003](<path/to/file.md>): Descreva uma terceira decisão técnica}
```

## Indicadores de Sucesso
Estes são indicadores que podem ser usados para medir o sucesso dessa solução.

{Se não fornecidos, sugira quais indicadores e métricas de negócio e/ou produto devem ser monitorados para entender o sucesso, e valide com o usuário. Siga o formato abaixo:}

- {Indicador 1: Descreva claramente o indicador de sucesso}
- {Indicador 2: Descreva um segundo indicador de sucesso}
- {Indicador 3: Descreva um terceiro indicador de sucesso}


## Escopo

### Evoluções futuras
{Coloque uma lista seguindo o formato abaixo, de possíveis evoluções futuras. Se o usuário fornecer, insira, se não sugira e peça confirmação para ele. Se ele não enviar nem aceitar suas sugestões, diga que você irá ignorar esse bloco.}

```
- [FRD-005](<path/to/frd/file.md>) ({status padrão para evoluções futuras é ICEBOX - Mude o status caso o usuário pedir ou a FRD mudar de status}): {Considerações futuras relevantes}  
- [FRD-006](<path/to/frd/file.md>) ({status padrão para evoluções futuras é ICEBOX - Mude o status caso o usuário pedir ou a FRD mudar de status}): {Possíveis aprimoramentos e expansões}
```

### Fora do escopo do conceito dessa iniciativa
{Faça essa lista baseada no bloco de O QUE ESSA INICIATIVA NÃO É}

- {Exclusão específica}  
- {Outra exclusão}  
- {Exclusão adicional}

## Artefatos e Documentação
- {Documento 1: Link ou referência ao documento de suporte}
- {Documento 2: Material de referência adicional}
