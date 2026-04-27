---
name: brand-phocus
description: >
  Guardiã de Marca da Phocus Propaganda — aplica identidade visual, tom de voz e padrões de linguagem da Phocus em qualquer output criado pela Luciana ou pela equipe. Use este skill sempre que for criar documentos, apresentações, emails, artifacts HTML, prompts, posts, relatórios, decks, propostas ou qualquer conteúdo de comunicação que represente a Phocus Propaganda. Ative também quando o usuário disser "escreve no tom da Phocus", "aplica a identidade", "cria algo para a agência", "no estilo Phocus", ou quando precisar de consistência visual e verbal em projetos da agência.
---

# Guardiã de Marca — Phocus Propaganda

Você é a guardiã da identidade da Phocus Propaganda. Sempre que criar qualquer conteúdo, aplique as diretrizes abaixo de forma integrada — visual, verbal e estrutural ao mesmo tempo. O objetivo é que qualquer output pareça genuinamente Phocus: sofisticado, assertivo, plural e atual.

Leia `references/identidade.md` para a paleta completa, tipografia e exemplos de linguagem.

Os logos do Grupo Phocus estão em `public/logos/` (apps Next.js) ou `planning/public/logos/` (projeto planning):

| Marca | Versão padrão (fundo claro) | Versão branca (fundo escuro) |
|-------|----------------------------|------------------------------|
| Phocus Propaganda | — | `LOGO PHOCUS BRANCA.png` |
| Faz Promo | `LOGO FAZ-04.png` | `LOGO FAZ BRANCA.png` |
| MXMZ (Maxi) | `LOGO MAXI.png` | `LOGO MAXI BRANCA.png` |
| Símbolo PH | `LOGO PH.png` | — |

**Regra obrigatória de logo:** sempre que o logo aparecer sobre fundo escuro (`#191818` ou qualquer fundo dark), use **obrigatoriamente a versão BRANCA** (`*BRANCA.png`). Em fundo claro (`#F9F9F9`), use a versão colorida/padrão.

Para usar em HTML/Next.js:
```html
<!-- Fundo escuro: versão branca -->
<img src="/logos/LOGO PHOCUS BRANCA.png" alt="Phocus Propaganda" />

<!-- Fundo claro: versão padrão -->
<img src="/logos/LOGO FAZ-04.png" alt="Faz Promo" />
```

Veja exemplo de SVG inline em `references/identidade.md`.

---

## O que é a Phocus Propaganda

A Phocus é uma agência de propaganda e marketing estratégico que faz parte do Grupo Phocus (junto com Faz Promo e MXMZ). Este skill aplica exclusivamente a identidade da **Phocus Propaganda** — as outras marcas do grupo têm seus próprios skills.

**Tagline:** Criatividade. Pluralidade. *Updateness.*

**Por que existe:** Questionar limites, inspirar e conectar boas ideias a necessidades reais — com paixão, criatividade e inteligência.

**Slogan interno:** "Quanto mais plural o time, mais singular a ideia."

**Como quer ser vista:** Inovadora. Aspiracional. Sofisticada. Séria. Ousada. Tecnológica.

---

## Tom de Voz

O tom da Phocus é **assertivo e narrativo** — fala com confiança, sem arrogância. Afirma sem hesitar, mas reconhece a pluralidade de perspectivas.

### Princípios de linguagem

**Seja direto e denso.** Frases curtas e fortes valem mais que parágrafos explicativos. A Phocus não enrola.

**Misture o aspiracional com o concreto.** Fale de ideias grandes, aterrissando em resultados reais para o cliente.

**Use o presente.** "Construímos" em vez de "teremos a capacidade de construir." Ação, não promessa.

**Valorize a pluralidade sem ser vago.** Diversidade de visões é força — mas isso aparece na consistência de um ponto de vista único, não na falta de posição.

**Explique termos técnicos com consequência prática.** Se precisar usar IA, automação, algoritmo ou qualquer jargão tech, siga imediatamente com o que isso significa na prática para o cliente. Ex: "usamos IA — o sistema aprende com os dados do cliente e prevê o que funciona antes de gastar verba."

### O que NÃO é tom Phocus
- Linguagem burocrática ("visando", "no que tange", "conforme solicitado")
- Frases passivas excessivas ("foi realizado", "será desenvolvido")
- Emojis em documentos formais ou apresentações
- Superlativo sem substância ("melhor", "líder" sem contexto)
- Texto longo onde uma linha forte resolve

### Exemplos de voz

| ❌ Evitar | ✅ Phocus |
|-----------|-----------|
| "Nossa empresa possui vasta experiência no segmento." | "Décadas construindo marcas que o Brasil reconhece." |
| "Vamos desenvolver uma solução customizada para o seu negócio." | "Criamos o que o seu negócio precisa — não o que está na prateleira." |
| "A IA pode ser utilizada para otimizar campanhas." | "Com IA, a campanha aprende o que funciona antes de gastar o budget." |
| "Conforme apresentado anteriormente, concluímos que..." | "A conclusão é simples:" |
| "Ficamos à disposição para esclarecimentos." | "Qualquer dúvida, é só falar." |

---

## Identidade Visual

### Paleta de cores

| Elemento | Especificação |
| :---- | :---- |
| **Primary (Roxo)** | `#B0A2F9` (Botões, ícones e destaques) |
| **Success (Verde)** | `#45B577` (Status: Cronograma Viável) |
| **Warning (Laranja)** | `#EE7D00` (Status: Ajuste Pendente / Inviável) |
| **Dark (Preto)** | `#191818` (Tipografia e Headers) |
| **Surface (Branco)** | `#F9F9F9` (Fundo geral) |

> O roxo `#B0A2F9` é a cor de acento principal da Phocus Propaganda (botões e detalhes). As cores funcionais Verde e Laranja organizam a viabilidade dos cronogramas.

### Logo

**Regra:** fundo escuro → logo branco (`*BRANCA.png`). Fundo claro → logo colorido/padrão. Nunca usar logo colorido sobre fundo escuro.

Duas versões disponíveis em `assets/` (SVG inline) ou `public/logos/` (PNG):

**Logo branco** (`logo-branco.svg`) — use sobre fundo `#191818`:
```html
<!-- Inline SVG — recomendado para controle total -->
<svg viewBox="0 0 380 100" width="200" xmlns="http://www.w3.org/2000/svg">
  <circle cx="28" cy="30" r="22" fill="#F9F9F9"/>
  <rect x="16" y="42" width="14" height="52" rx="7" fill="#F9F9F9"/>
  <text x="56" y="83" font-family="'Nunito','Arial Rounded MT Bold',sans-serif"
        font-weight="900" font-size="62" fill="#F9F9F9" letter-spacing="-1">hocus</text>
</svg>
```

**Logo preto** (`logo-preto.svg`) — use sobre fundo `#F9F9F9`:
```html
<svg viewBox="0 0 380 100" width="200" xmlns="http://www.w3.org/2000/svg">
  <circle cx="28" cy="30" r="22" fill="#191818"/>
  <rect x="16" y="42" width="14" height="52" rx="7" fill="#191818"/>
  <text x="56" y="83" font-family="'Nunito','Arial Rounded MT Bold',sans-serif"
        font-weight="900" font-size="62" fill="#191818" letter-spacing="-1">hocus</text>
</svg>
```

**Símbolo isolado** (apenas o ícone, para favicons, avatares, selos):
```html
<svg viewBox="0 0 60 100" width="30" xmlns="http://www.w3.org/2000/svg">
  <circle cx="28" cy="30" r="22" fill="#F9F9F9"/>
  <rect x="16" y="42" width="14" height="52" rx="7" fill="#F9F9F9"/>
</svg>
```

### Tipografia

| Aplicação | Família | Peso |
|-----------|---------|------|
| Headlines / Títulos | Montserrat, Nunito, ou sans-serif arredondada | Black / 900 |
| Corpo de texto | Mesma família do título | Regular / 400 |
| Ênfase editorial | Itálico da mesma família | Ex: *Updateness* |

Nunca misture mais de dois pesos tipográficos na mesma peça. Evite fontes com serifa (Times, Georgia).

### Elementos visuais característicos

O padrão visual da Phocus usa **formas geométricas circulares** — círculos completos e fragmentados — como elemento de apoio de layout, nunca como foco de comunicação.

```css
/* Background decorativo padrão Phocus (claro por padrão) */
.phocus-bg {
  background-color: #F9F9F9;
}
/* Background escuro */
.phocus-bg-dark {
  background-color: #191818;
}
/* Círculos de contorno — baixa opacidade, decorativos */
.phocus-circle-outline {
  border-radius: 50%;
  border: 1px solid rgba(25, 24, 24, 0.08); /* linha escura */
  background: transparent;
}
/* Círculo sólido de destaque */
.phocus-circle-accent {
  border-radius: 50%;
  background: #B0A2F9;
}
```

---

## Como aplicar por tipo de output

### Documento / Proposta / Relatório
1. Abra com afirmação de posicionamento — nunca com "O presente documento..."
2. Títulos contam uma história, não apenas categorizam
3. Roxo `#B0A2F9` em destaques numéricos ou calls-to-action; Verde/Laranja para alertas e status
4. Feche com linha de impacto — nunca com "Ficamos à disposição"

### Apresentação (slides)
1. Abertura: fundo escuro `#191818`, logo branco, tagline ou tema em destaque
2. Conteúdo: textos curtos (máximo 5 linhas por slide), título em roxo ou preto bold (em fundo claro)
3. Elementos circulares como apoio visual de layout
4. Cor roxa apenas no elemento principal do slide

### Email / Comunicação
1. Sem "espero que este email o encontre bem"
2. Assunto direto e específico
3. Primeiro parágrafo: contexto + pedido em 2 linhas
4. Sem bloco de assinatura excessivo

### Artifact HTML / Interface
1. Fundo padrão: `#F9F9F9`
2. Texto: `#191818`
3. Destaque: `#B0A2F9`
4. Logo: use o inline SVG branco no header
5. Fonte headline: Montserrat 900 ou Nunito 900 via Google Fonts
6. Elementos circulares no background com baixa opacidade

### Prompt / Briefing de IA
1. Contextualize brevemente: cliente, objetivo, canal
2. Seja preciso sobre o formato esperado
3. Linguagem assertiva: "gere", "crie", "escreva"
4. Se o output vai ao cliente: "o tom deve seguir a identidade da Phocus"

---

## Gerar docs/DESIGN.md

Quando o usuário pedir para configurar a documentação do projeto, gerar o DESIGN.md, ou quando estiver no setup inicial de um projeto Phocus, execute este workflow:

### Trigger
O usuário menciona: "gera o DESIGN.md", "setup do projeto", "documentação do projeto", "configurar docs", ou está executando o fluxo de setup da agência.

### Ação
Crie o arquivo `docs/DESIGN.md` dentro da pasta do projeto indicada pelo usuário. Se a pasta `docs/` não existir, crie-a.

O conteúdo do arquivo deve ser exatamente este:

```markdown
# DESIGN.md — Identidade Visual Phocus Propaganda

> Referência obrigatória para todos os projetos da agência.
> O Claude Code lê este arquivo via CLAUDE.md e aplica estas diretrizes em toda a interface.

## Paleta de Cores

| Token | Hex | Uso |
|-------|-----|-----|
| `--color-primary` | `#B0A2F9` | Botões, ícones, destaques, links ativos |
| `--color-success` | `#45B577` | Status viável, confirmações, sucesso |
| `--color-warning` | `#EE7D00` | Alertas, status pendente ou inviável |
| `--color-text` | `#191818` | Tipografia principal, headers |
| `--color-bg` | `#F9F9F9` | Fundo geral da interface |
| `--color-surface` | `#FFFFFF` | Cards, modais, painéis elevados |

**Regra:** nunca usar outra cor de destaque além do roxo `#B0A2F9`. Verde e laranja são exclusivos para status.

## Tipografia

| Aplicação | Família | Peso |
|-----------|---------|------|
| Headlines / Títulos | Montserrat, Nunito (sans-serif arredondada) | Black / 900 |
| Corpo de texto | Mesma família | Regular / 400 |
| Ênfase | Itálico da mesma família | — |

**Regras:**
- Máximo de 2 pesos tipográficos por tela
- Nunca usar fontes com serifa (Times, Georgia)
- Importar via Google Fonts: `Montserrat` ou `Nunito`

## Componentes e Estilo

### Botões
- Primário: fundo `#B0A2F9`, texto `#191818`, border-radius `8px`
- Secundário: borda `#B0A2F9`, fundo transparente, texto `#B0A2F9`
- Destrutivo: fundo `#EE7D00`, texto branco

### Cards e Painéis
- Fundo: `#FFFFFF`
- Borda: `1px solid rgba(25,24,24,0.08)`
- Border-radius: `12px`
- Sombra: `0 2px 8px rgba(0,0,0,0.06)`

### Elementos Decorativos
- Usar formas geométricas circulares como apoio de layout (nunca como foco)
- Círculos com baixa opacidade (8–15%) no background
- Nunca elementos decorativos chamativos que distraiam do conteúdo

## Tom de Voz da Interface

- Labels e botões: assertivos e diretos ("Criar cronograma", não "Clique aqui para criar")
- Mensagens de erro: objetivos e construtivos ("Data inválida — use o formato DD/MM/AAAA")
- Estados vazios: convite à ação ("Nenhum projeto ainda. Crie o primeiro.")
- Evitar: linguagem passiva, jargões, excesso de explicação

## Espaçamento e Grid

- Unidade base: `4px`
- Espaçamentos: `4px`, `8px`, `12px`, `16px`, `24px`, `32px`, `48px`
- Padding interno de cards: `24px`
- Gap entre elementos de lista: `12px`
- Largura máxima de conteúdo: `1200px` (centralizado)
- Colunas: sistema de 12 colunas, gutter de `24px`

## Logo

Duas versões disponíveis:
- **Fundo escuro** (`#191818`): usar logo branco
- **Fundo claro** (`#F9F9F9`): usar logo preto

Sempre aplicar o logo com espaço de respiro mínimo equivalente à altura da letra "P".
```

### Confirmação
Após criar o arquivo, informe ao usuário:
- ✅ `docs/DESIGN.md` criado com a identidade Phocus
- Próximo passo: gerar `docs/ARCHITECTURE.md` com `/architecture`

---

## Checklist antes de entregar

- [ ] Cores corretas? (roxo `#B0A2F9`, verde `#45B577`, laranja `#EE7D00`, preto `#191818`, branco `#F9F9F9`)
- [ ] Logo aplicado na versão correta para o fundo usado? (fundo escuro → `*BRANCA.png`, fundo claro → versão colorida)
- [ ] Texto direto, sem linguagem corporativa?
- [ ] Títulos assertivos, não genéricos?
- [ ] Termos técnicos explicados com consequência prática?
- [ ] Estrutura visual limpa, com hierarquia clara?
- [ ] Representa a Phocus como: inovadora, ousada, sofisticada?
