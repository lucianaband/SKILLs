# Identidade Visual Grupo Phocus — Referência Completa

## Paleta de Cores

### Cores base (sempre presentes)
| Nome | HEX | Uso |
|------|-----|-----|
| Dark (Preto) | `#191818` | Tipografia, headers, fundos escuros e contrastes |
| Surface (Branco) | `#F9F9F9` | Fundo geral, texto em fundos escuros |

### Cores vibrantes (das submarcas)
| Nome | HEX | Contexto de uso |
|------|-----|-----------------|
| Primary (Roxo) | `#B0A2F9` | Botões, ícones, elementos de destaque Phocus |
| Success (Verde) | `#45B577` | Status de viabilidade: Cronograma Viável |
| Warning (Laranja) | `#EE7D00` | Status de viabilidade: Ajuste Pendente / Inviável |

### Regra das cores complementares
> "A identidade do grupo absorve elementos cromáticos das submarcas. Não como soma literal, mas como síntese. Não se trata de reunir todas as cores, mas de selecionar e reinterpretar aquilo que melhor representa o conjunto."

**Na prática:**
- Use apenas **uma** cor vibrante por peça (a mais adequada ao contexto)
- Nunca use gradientes com as cores vibrantes
- Cores vibrantes em fundo sólido podem exigir **texto escuro** (`#191818`) ou branco para legibilidade, dependendo do contraste
- Fundo escuro + cor vibrante (Roxo) + texto branco = combinação forte para impacto pontual

---

## Tipografia

### Famílias preferidas
- **Títulos / Headlines:** Montserrat Black, Inter Bold, ou qualquer sans-serif de peso alto
- **Corpo de texto:** Montserrat Regular, Inter Regular
- **Destaque editorial:** Itálico da mesma família (ex: *Updateness* no tagline)

### Hierarquia tipográfica
```
Título principal    → Bold/Black, grande, máximo 5 palavras
Subtítulo           → Regular ou Medium, descritivo
Corpo               → Regular, parágrafos curtos (3-5 linhas)
Caption/Label       → Small caps ou tamanho reduzido, uppercase
```

### Não usar
- Fontes com serifa (Times, Georgia) em materiais de marca
- Fontes decorativas ou display em textos corridos
- Mais de 2 pesos tipográficos na mesma peça

---

## Elementos Visuais Característicos

### Formas geométricas
O padrão visual da Phocus usa formas circulares — círculos completos e fragmentados — como elemento decorativo. Nunca como elemento central de comunicação, sempre como apoio de layout.

**Aplicação em CSS/HTML:**
```css
/* Elemento decorativo típico da Phocus */
.phocus-circle {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  background-color: #B0A2F9; /* cor primária */
  opacity: 0.9;
}

.phocus-circle-outline {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  border: 1px solid rgba(249, 249, 249, 0.3);
  background: transparent;
}
```

### Padrão de background geométrico
Para artifacts que precisem de background decorativo, use círculos de outline em baixa opacidade:
```css
.phocus-bg-pattern {
  background-color: #191818;
  background-image: 
    radial-gradient(circle at 15% 50%, rgba(249,249,249,0.04) 80px, transparent 80px),
    radial-gradient(circle at 85% 20%, rgba(249,249,249,0.04) 60px, transparent 60px);
}
```

---

## Exemplos de Aplicação por Canal

### Card / Banner digital (fundo escuro)
```
Background: #191818
Título: #F9F9F9 (bold)
Destaque numérico ou palavra-chave: #B0A2F9
Subtítulo: #F9F9F9 (regular, menor)
Elemento decorativo: círculo outline em branco 15% opacidade
```

### Slide de apresentação — estilo escuro
```
Slide 1 (abertura): fundo #191818, logo branco, tagline em roxo/branco
Slides de conteúdo: fundo #191818, título em roxo, corpo branco
Slides de dados: destaque numérico em cor vibrante, explicação em branco
Slide final: fundo escuro, CTA em cor vibrante bold
```

### Slide de apresentação — estilo claro
```
Fundo: #F9F9F9
Título: #191818 (bold)
Destaque: cor vibrante
Corpo: #191818 (regular)
Elemento decorativo: círculo preenchido em cor vibrante, canto ou margem
```

### Documento Word / Google Docs
```
Título do documento: bold, preto, sem sublinhar
Cabeçalhos H2: bold, preto
Destaques inline: negrito (sem cor, mantém leitura clean)
Call-out box: fundo levemente cinza (#F0F0F0), borda esquerda cor vibrante
Rodapé: logo + nome + cidade | fonte pequena, cinza claro
```

---

## Composição: "Quanto mais plural o time, mais singular a ideia."

Este é o conceito central da identidade. Use-o quando precisar de headline forte em materiais institucionais, propostas de valor, ou abertura de apresentações.

**Variações aprovadas:**
- "Criatividade. Pluralidade. *Updateness.*" (tagline completo)
- "Marcas fortes constroem negócios fortes." (linha de valor)
- "Somos o grupo que junta o que o mercado separa." (posicionamento)

---

## Logos e submarcas

| Marca | Cor principal | Contexto |
|-------|--------------|---------|
| phocus (grupo) | Surface / Dark / Roxo | Materiais institucionais e de agência |
| (Outras) | Variável | (Consulte a identidade específica de outras submarcas se necessário) |

> O "p" do logo Phocus tem um ponto separado — é parte da identidade, não um erro tipográfico.
