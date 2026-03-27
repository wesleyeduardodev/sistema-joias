# Prompts para Google Stitch — Sistema de Joias (MVP)

> **Como usar:** Acesse [stitch.withgoogle.com](https://stitch.withgoogle.com), copie cada prompt abaixo e cole no Stitch. Gere uma tela por vez. Exporte como React ou Figma.
>
> **Dica:** O Stitch funciona melhor com prompts que descrevem a "vibe" e o conteudo da tela. Os prompts abaixo ja incluem o design system, layout e dados de exemplo.

---

## Design System Global

**Cole este prompt primeiro para estabelecer a identidade visual. Use como base para todas as telas:**

```
Design system for a premium jewelry management system (admin panel).

BRAND IDENTITY:
- Industry: High-end jewelry store (rings, necklaces, bracelets, earrings, watches)
- Feeling: Luxurious, sophisticated, trustworthy, clean, professional
- Language: Portuguese (Brazil)

COLOR PALETTE:
- Primary/Text: #1A1A1A (deep black)
- Accent/CTA: #C9A84C (gold)
- Accent hover: #E8D5A3 (light gold)
- Background: #FAFAFA (off-white)
- Cards/Modals: #FFFFFF (pure white)
- Muted text: #6B7280 (gray)
- Borders: #E5E7EB (light gray)
- Success: #059669 (green)
- Warning: #D97706 (amber)
- Danger: #DC2626 (red)

TYPOGRAPHY:
- Headings and body: Inter (sans-serif, clean, premium)
- Numbers and codes: JetBrains Mono (monospace)

LAYOUT:
- 8px grid system
- Max container: 1440px
- Left sidebar: 280px with dark background (#1A1A1A), gold accent icons, collapsible
- Top header: white, with global search bar, notification bell, user avatar dropdown
- Content area: off-white background with white cards

COMPONENTS STYLE:
- Buttons: gold background with dark text for primary, outlined for secondary
- Cards: white with subtle shadow, rounded corners 8px
- Tables: clean with alternating row colors, hover highlight
- Badges/Status: colored pills (green=active, amber=pending, red=cancelled)
- Inputs: bordered, rounded 6px, focus ring in gold
```

---

## Fase 1 — Fundacao

### 1.1 Login

```
Login page for a premium jewelry management system called "JoiasGestor".

DESIGN: Luxury, minimal, elegant. Split layout.
- Left side (60%): Full-height hero image of elegant jewelry (gold rings and diamonds on dark velvet). Overlay with semi-transparent dark gradient. Logo "JoiasGestor" in gold text at top-left. Tagline: "Gestao inteligente para sua joalheria" in white.
- Right side (40%): White background, vertically centered login form.

LOGIN FORM:
- Heading: "Entrar" in large bold text
- Subheading: "Acesse sua conta para continuar" in muted gray
- Field: "Email" — input with envelope icon, placeholder "seu@email.com"
- Field: "Senha" — input with lock icon, placeholder "********", toggle visibility eye icon
- Checkbox: "Lembrar de mim" on the left
- Link: "Esqueci minha senha" on the right, gold color
- Button: "Entrar" — full width, gold background (#C9A84C), dark text, bold
- Divider with text "ou"
- Link: "Criar uma conta" — text link, gold color

COLORS: Background #FAFAFA, text #1A1A1A, accent #C9A84C, muted #6B7280
FONT: Inter
RESPONSIVE: On mobile, hero image becomes a small banner at top (20vh), form takes full width below.
```

### 1.2 Registro

```
Registration page for a jewelry management system. Same luxury aesthetic as login.

LAYOUT: Same split layout as login (hero left, form right).

REGISTRATION FORM:
- Heading: "Criar Conta"
- Subheading: "Preencha seus dados para solicitar acesso"
- Fields (stacked, full width):
  - "Nome completo" — text input with user icon
  - "Email" — input with envelope icon
  - "CPF" — input with ID card icon, mask format "000.000.000-00"
  - "Telefone" — input with phone icon, mask "(00) 00000-0000"
  - "Senha" — password input with strength indicator bar below (weak=red, medium=amber, strong=green)
  - "Confirmar senha" — password input
- Button: "Solicitar Acesso" — full width, gold background
- Text below: "Sua conta sera ativada apos aprovacao do administrador."
- Link: "Ja tem conta? Entrar" — gold text

COLORS: #FAFAFA background, #1A1A1A text, #C9A84C accent
FONT: Inter
```

### 1.3 Recuperar Senha

```
Password recovery page for a jewelry system. Minimal, centered layout.

LAYOUT: Centered card (max 480px) on off-white background (#FAFAFA).

CARD CONTENT:
- Gold lock icon at top (48px)
- Heading: "Recuperar Senha"
- Text: "Informe seu email e enviaremos um link para redefinir sua senha."
- Field: "Email" — full width input
- Button: "Enviar Link" — full width, gold (#C9A84C)
- Link below: "Voltar para o login" with left arrow icon

SUCCESS STATE (show as second variant):
- Green check icon
- Text: "Email enviado! Verifique sua caixa de entrada."
- Button: "Voltar para o Login"

COLORS: #FAFAFA bg, #FFFFFF card, #C9A84C accent, #1A1A1A text
FONT: Inter
```

### 1.4 Layout Base (AppShell — Sidebar + Header)

```
Admin panel shell/layout for a premium jewelry management system. This is the base layout that wraps all internal pages.

SIDEBAR (left, fixed, 280px):
- Background: #1A1A1A (dark)
- Top: Logo "JoiasGestor" in gold (#C9A84C) text, elegant font
- Navigation items with icons (gold icons, white text, 16px Inter):
  - Dashboard (grid icon) — ACTIVE state: gold left border, gold text, subtle dark highlight
  - Catalogo (diamond icon)
  - Estoque (package icon)
  - Vendas (shopping-cart icon)
  - Consignacao (truck icon)
  - Representantes (users icon)
  - Clientes (user-plus icon)
  - Configuracoes (gear icon) — at bottom, separated by divider
- Bottom: Collapse button (chevron-left icon)

HEADER (top bar, full width minus sidebar):
- Background: #FFFFFF with bottom border #E5E7EB
- Left: Breadcrumb — "Dashboard" in gray text
- Center: Search bar — rounded, placeholder "Buscar produtos, clientes, vendas...", magnifying glass icon
- Right: Notification bell with red badge "3", then vertical divider, then user avatar circle with dropdown arrow, name "Maria Silva" and role badge "Admin" in small gold pill

MAIN CONTENT AREA:
- Background: #FAFAFA
- Padding: 24px
- Show placeholder text "Conteudo da pagina aqui" centered in gray

RESPONSIVE: On mobile (<768px), sidebar becomes hamburger menu (drawer from left), header stays but search collapses into icon.
```

---

## Fase 2 — Catalogo e Estoque

### 2.1 Dashboard Admin

```
Admin dashboard for a jewelry store management system. Premium look, data-rich.

LAYOUT: Inside admin panel shell (dark sidebar left, white header top). Content area on off-white bg.

ROW 1 — KPI CARDS (4 cards, equal width, horizontal):
- Card 1: "Vendas Hoje" — value "R$ 12.500" in large bold, badge "▲ 12%" in green, small shopping-bag icon in gold
- Card 2: "Vendas do Mes" — value "R$ 287.000", badge "▲ 8%" in green, trending-up icon
- Card 3: "Estoque Total" — value "1.247 pecas", badge "3 alertas" in amber, package icon
- Card 4: "Meta da Equipe" — value "82%", circular progress ring in gold (82% filled), target icon
All cards: white background, subtle shadow, rounded 12px, gold accent icon top-right

ROW 2 — CHART (full width):
- Card title: "Vendas — Ultimos 30 dias"
- Line chart: gold line (#C9A84C) for actual sales, dashed gray line for monthly target
- X-axis: dates (01/03 to 27/03), Y-axis: values in R$
- Hover tooltip showing date and value
- Clean, minimal chart style

ROW 3 — TWO COLUMNS:
- Left card: "Top Vendedores do Mes"
  - Ranked list with avatar circle, name, and value:
    1. Maria Silva — R$ 45.200 (gold bar 90%)
    2. Joao Santos — R$ 38.100 (gold bar 76%)
    3. Ana Costa — R$ 31.400 (gold bar 63%)
    4. Pedro Lima — R$ 28.900 (gold bar 58%)
    5. Carla Reis — R$ 22.700 (gold bar 45%)

- Right card: "Vendas Recentes"
  - Table with columns: Venda | Cliente | Vendedor | Valor | Status
  - Row 1: #1234 | Maria Oliveira | Joao | R$ 4.500 | green badge "Paga"
  - Row 2: #1233 | Pedro Santos | Ana | R$ 1.200 | green badge "Paga"
  - Row 3: #1232 | Julia Costa | Maria | R$ 8.900 | amber badge "Confirmada"
  - Row 4: #1231 | Cliente Avulso | Pedro | R$ 650 | green badge "Paga"
  - Link at bottom: "Ver todas as vendas →" in gold

COLORS: #FAFAFA bg, #FFFFFF cards, #C9A84C gold accents, #1A1A1A text, #059669 success, #D97706 warning
FONT: Inter for text, JetBrains Mono for monetary values
```

### 2.2 Catalogo — Listagem de Produtos

```
Product catalog listing page for a jewelry store system. Grid view with faceted filters.

LAYOUT: Admin panel shell. Content area.

PAGE HEADER:
- Title: "Catalogo de Produtos" (bold, large)
- Right side: Button "Novo Produto" (gold bg, dark text, plus icon) and button "Importar" (outlined, upload icon)

SEARCH BAR (full width):
- Large rounded input: "Buscar por nome, codigo, numero de serie..."
- Magnifying glass icon left, clear X button right

FILTER BAR (horizontal, below search):
- Dropdown chips: [Categoria ▼] [Metal ▼] [Pedra ▼] [Faixa de Preco ▼] [Tamanho ▼] [Disponibilidade ▼]
- Active filter example: "Metal: Ouro 18K ×" as a removable pill
- Right side: "Limpar filtros" link, then toggle [Grid icon] [List icon], then text "1.247 produtos"

PRODUCT GRID (4 columns on desktop, 2 on mobile):
Show 8 product cards:

Card 1: Image of gold solitaire ring | "Anel Solitario Diamante" | "Ouro 18K Amarelo" | "R$ 4.500" | Green dot "Disponivel"
Card 2: Image of silver riviera necklace | "Colar Riviera" | "Prata 925" | "R$ 1.200" | Green dot "Disponivel"
Card 3: Image of gold hoop earrings | "Brinco Argola" | "Ouro 18K Rose" | "R$ 2.100" | Amber dot "Consignado"
Card 4: Image of diamond bracelet | "Pulseira Riviera" | "Ouro 18K Branco" | "R$ 8.900" | Green dot "Disponivel"
Card 5: Image of engagement ring | "Alianca Casamento" | "Ouro 18K Amarelo" | "R$ 3.200" | Green dot "Disponivel"
Card 6: Image of pendant | "Pingente Letra M" | "Ouro 18K Amarelo" | "R$ 890" | Red dot "Vendido"
Card 7: Image of pearl earrings | "Brinco Perola" | "Ouro 18K Branco" | "R$ 1.650" | Green dot "Disponivel"
Card 8: Image of tennis bracelet | "Pulseira Tennis" | "Ouro 18K Branco" | "R$ 12.400" | Amber dot "Reservado"

Each card: white bg, rounded 12px, shadow on hover, image 1:1 ratio at top, info below with name (bold), metal (muted), price (large, JetBrains Mono), status badge

PAGINATION: Bottom center — "← 1 2 3 ... 52 →"

RESPONSIVE: Grid becomes 2 columns on tablet, 1 column on mobile. Filters collapse into "Filtros" button that opens bottom sheet drawer.
```

### 2.3 Catalogo — Cadastro de Produto (Stepper)

```
New product registration form for a jewelry store. Multi-step form (stepper) with 5 stages.

LAYOUT: Admin panel shell. Content area, white card, max-width 900px centered.

PAGE HEADER: "Novo Produto" with breadcrumb "Catalogo > Novo Produto"

STEPPER BAR (horizontal, top of card):
Steps connected by line: ① Basico (active, gold) — ② Material (gray) — ③ Pedras (gray) — ④ Preco (gray) — ⑤ Imagens (gray)
Active step: gold circle with number, gold text
Completed: gold circle with checkmark
Upcoming: gray circle with number, gray text

STEP 1 — INFORMACOES BASICAS (currently active):
Form fields in 2-column grid:
- "Nome da peca *" — full width text input, placeholder "Ex: Anel Solitario Diamante 0.5ct"
- "Codigo interno / SKU *" — half width, placeholder "JOI-ANE-001"
- "Numero de serie *" — half width, placeholder "SER-2026-00001"
- "Categoria *" — dropdown select: Aneis, Aliancas, Brincos, Colares, Pulseiras, Pingentes, Relogios, Conjuntos
- "Subcategoria *" — dropdown (dependent on category): Solitario, Meia-alianca, Cocktail, Aparador...
- "Tipo de produto *" — radio buttons: ● Joia  ○ Semijoia  ○ Bijuteria
- "Genero" — radio buttons: ○ Masculino  ○ Feminino  ● Unissex
- "Descricao" — full width textarea, 3 rows, placeholder "Descricao detalhada da peca..."
- "Personalizavel" — checkbox "Aceita gravacao/ajuste de tamanho"
- "Destaque" — checkbox "Exibir em destaque no catalogo"

BOTTOM BAR (sticky):
- Left: "Cancelar" text button (gray)
- Right: "Proximo →" gold button

COLORS: #FFFFFF card, #FAFAFA bg, #C9A84C stepper active + primary button, #E5E7EB borders
FONT: Inter, labels in 14px muted, inputs in 16px

RESPONSIVE: On mobile, stepper shows only current step name with < > arrows. Form becomes single column.
```

### 2.4 Catalogo — Cadastro de Produto (Etapa 2: Material)

```
Step 2 of the jewelry product registration form — Materials and Specifications.

STEPPER: ✓ Basico — ② Material (active, gold) — ③ Pedras — ④ Preco — ⑤ Imagens

FORM FIELDS (2-column grid):
- "Metal principal *" — dropdown: Ouro Amarelo, Ouro Branco, Ouro Rose, Prata 925, Prata 950, Platina, Aco Inoxidavel, Titanio
- "Quilatagem" — dropdown: 10K, 14K, 18K, 24K, 925, 950 (shown if metal is gold or silver)
- "Cor do metal" — radio buttons with color swatches: ● Amarelo (yellow dot) ○ Branco (silver dot) ○ Rose (pink dot)
- "Peso total da peca (g) *" — number input, placeholder "5.200", suffix "g"
- "Peso do metal (g)" — number input, placeholder "4.800", suffix "g"
- "Acabamento" — dropdown: Polido, Fosco/Acetinado, Escovado, Diamantado, Texturizado, Martelado
- "Tipo de cravacao" — dropdown: Garra, Inglesa/Canal, Pave, Trilho, Invisivel, Nenhuma
- "Tipo de fecho" — dropdown: Trava, Mosquetao, Pressao, Rosca, Gaveta, Nenhum
- "Tamanho / Aro" — input with helper text "Para aneis use numero do aro (ex: 18). Para colares/pulseiras use cm (ex: 45cm)"
  - Small conversion table shown below: "Tabela: BR 12=US 3, BR 14=US 5, BR 16=US 6, BR 18=US 8, BR 20=US 9, BR 22=US 10"

BOTTOM BAR:
- Left: "← Anterior" outlined button
- Right: "Proximo →" gold button

Same premium styling, white card, gold accents.
```

### 2.5 Catalogo — Cadastro de Produto (Etapa 3: Pedras)

```
Step 3 of jewelry product form — Gemstones. Dynamic list where user can add multiple stones.

STEPPER: ✓ Basico — ✓ Material — ③ Pedras (active, gold) — ④ Preco — ⑤ Imagens

SECTION: "Pedras da peca"
- Button at top-right: "+ Adicionar Pedra" (outlined, gold border)

STONE CARD 1 (already added, white card with border):
- Header: "Pedra #1 — Diamante" with trash icon button on right (red on hover)
- 2-column grid fields:
  - "Tipo de pedra *" — dropdown: Diamante (selected), Rubi, Esmeralda, Safira, Ametista, Agua-marinha, Topazio, Zirconia...
  - "Quantidade *" — number input: "1"
  - "Peso total (ct) *" — number input: "0.50", suffix "ct"
  - "Tipo de cravacao" — dropdown: Garra
- Subsection "Classificacao 4Cs (Diamante)" — shown only when stone type is Diamante:
  - "Lapidacao (Cut)" — dropdown: Excellent, Very Good, Good, Fair, Poor
  - "Cor (Color)" — dropdown with visual scale: D, E, F, G, H, I, J... Z
  - "Pureza (Clarity)" — dropdown: FL, IF, VVS1, VVS2, VS1, VS2, SI1, SI2, I1, I2, I3
  - "Quilate (Carat)" — same as peso total above

STONE CARD 2 (minimal, just added):
- "Tipo de pedra *" — dropdown: Zirconia
- "Quantidade *" — "12"
- "Peso total (ct)" — "0.36"
- No 4Cs section (only for diamonds)

SECTION: "Certificacao" (separated by divider)
- "Instituicao certificadora" — dropdown: GIA, IGI, HRD, IBGM, Outra, Nenhuma
- "Numero do certificado" — text input
- "Data de emissao" — date picker
- "Documento" — file upload area: "Arraste o PDF do certificado aqui ou clique para enviar"

BOTTOM BAR: "← Anterior" | "Proximo →"
```

### 2.6 Catalogo — Cadastro de Produto (Etapa 4: Preco)

```
Step 4 of jewelry product form — Pricing. Auto-calculated fields based on metal quotation.

STEPPER: ✓ Basico — ✓ Material — ✓ Pedras — ④ Preco (active, gold) — ⑤ Imagens

SECTION: "Calculo de Custo"

INFO CARD (light gold bg #FFF9EC, gold left border):
- "Cotacao atual do Ouro 18K: R$ 350,00/g" — auto-fetched, with date "Atualizado em 27/03/2026"

FORM (2-column grid with auto-calculation):
- "Custo do metal" — READ-ONLY calculated field, showing "R$ 1.680,00" with formula below in muted text: "= Peso metal (4.800g) x Cotacao (R$ 350,00/g)"
- "Custo das pedras" — currency input: "R$ 3.200,00"
- "Custo mao de obra" — currency input: "R$ 800,00"
- "Custos indiretos" — currency input: "R$ 150,00" with helper "Embalagem, certificacao, seguro"

DIVIDER

- "Custo total" — READ-ONLY bold: "R$ 5.830,00" (sum of above)
- "Markup" — number input with "x" suffix: "2.50" with slider below (range 1.5 to 4.0)
- "Preco de venda calculado" — READ-ONLY large bold gold text: "R$ 14.575,00" with formula "= Custo total x Markup"
- "Preco de venda final *" — editable currency input, default same as calculated: "R$ 14.575,00" with helper "Ajuste manualmente se necessario"
- "Preco sugerido (consignacao)" — currency input: "R$ 14.575,00" with helper "Preco sugerido para representantes"

Visual: Show a small breakdown summary card on the right:
```
Composicao do Preco
├── Metal:     R$ 1.680 (29%)  [gold bar]
├── Pedras:    R$ 3.200 (55%)  [blue bar]
├── Mao obra:  R$   800 (14%)  [gray bar]
└── Indiretos: R$   150 (2%)   [light bar]
Total custo:   R$ 5.830
Markup 2.5x:   R$ 14.575
```

BOTTOM BAR: "← Anterior" | "Proximo →"
```

### 2.7 Catalogo — Cadastro de Produto (Etapa 5: Imagens)

```
Step 5 of jewelry product form — Image upload. Drag and drop, reorderable.

STEPPER: ✓ Basico — ✓ Material — ✓ Pedras — ✓ Preco — ⑤ Imagens (active, gold)

SECTION: "Imagens do Produto"
- Helper text: "Adicione fotos de alta qualidade. A primeira imagem sera a principal."

UPLOAD AREA:
- Large dashed border area (200px height):
  - Cloud-upload icon in gold
  - "Arraste imagens aqui ou clique para selecionar"
  - "PNG, JPG ate 5MB cada. Maximo 10 imagens."

UPLOADED IMAGES (grid of thumbnails, 4 columns):
- Image 1: Ring front view — gold "Principal" badge at top, drag handle (6 dots icon) at top-left, trash icon at top-right on hover. Large thumbnail 200x200px.
- Image 2: Ring on finger (model shot) — gray "Modelo" label
- Image 3: Ring detail/close-up of diamond — gray "Detalhe" label
- Image 4: Certificate scan — gray "Certificado" label
- Each thumbnail: rounded 8px, subtle shadow, reorderable by drag and drop

Hint text below: "Dica: Use fundo branco para a foto principal. Inclua fotos em uso (modelo), detalhes da pedra, e o certificado."

BOTTOM BAR:
- Left: "← Anterior" outlined button
- Right: "Salvar Produto" gold button (larger, with check icon)

Same premium styling.
```

### 2.8 Estoque — Visao Geral

```
Inventory overview page for a jewelry store management system.

LAYOUT: Admin panel shell with sidebar and header.

PAGE HEADER:
- Title: "Estoque"
- Right: Button "Nova Movimentacao" (gold, plus icon) and Button "Inventario" (outlined, clipboard icon)

ROW 1 — SUMMARY CARDS (4 cards):
- "Total em Estoque" — "1.247 pecas" with subtext "R$ 2.4M em valor", package icon
- "No Cofre" — "843 pecas", lock icon in gold
- "Na Vitrine" — "189 pecas", eye icon
- "Consignado" — "215 pecas", truck icon in amber

ROW 2 — ALERTS (if any, amber card with warning icon):
- "3 Alertas de Estoque"
  - "⚠ Anel Solitario 0.3ct — estoque zerado"
  - "⚠ Consignacao #KIT-0032 vence em 3 dias (Rep. Carlos Silva)"
  - "⚠ Colar Riviera Prata — apenas 1 unidade restante"

ROW 3 — TWO COLUMNS:
Left: "Estoque por Categoria" — horizontal bar chart:
  - Aneis: 342
  - Colares: 198
  - Brincos: 267
  - Pulseiras: 156
  - Aliancas: 134
  - Pingentes: 89
  - Outros: 61

Right: "Estoque por Metal" — donut chart:
  - Ouro 18K: 45% (gold color)
  - Prata 925: 28% (silver/gray)
  - Ouro 14K: 12% (light gold)
  - Platina: 8% (dark gray)
  - Outros: 7% (muted)

ROW 4 — RECENT MOVEMENTS TABLE:
- Title: "Movimentacoes Recentes"
- Columns: Data | Tipo | Produto | De | Para | Usuario
- Row 1: 27/03 | green badge "Entrada" | Anel Solitario 0.5ct | — | Cofre | Maria
- Row 2: 27/03 | blue badge "Transferencia" | Colar Riviera | Cofre | Vitrine | Joao
- Row 3: 26/03 | amber badge "Consignacao" | Kit 12 pecas | Cofre | Rep. Carlos | Admin
- Row 4: 26/03 | red badge "Saida (Venda)" | Brinco Argola Rose | Vitrine | Vendido | Ana
- Link: "Ver todas as movimentacoes →"

COLORS: Premium palette, gold accents, clean tables.
```

---

## Fase 3 — Vendas e PDV

### 3.1 PDV — Ponto de Venda

```
Point of Sale (PDV) screen for a jewelry store. Split layout: product search left, cart right.

LAYOUT: Simplified header (no sidebar in PDV mode): Logo left, "PDV — Ponto de Venda" center, "Vendedora: Maria Silva" right with avatar.

SPLIT LAYOUT:
LEFT SIDE (65%):
- "Cliente:" Autocomplete input with search — showing "Maria Oliveira" selected with small X to clear. Button "+" to open quick registration modal.
- Large search bar: "Buscar produto por nome, codigo ou numero de serie..." with magnifying glass icon

SEARCH RESULTS (list of cards):
- Result 1: [Ring photo thumbnail 60x60] | "Anel Solitario Diamante 0.5ct" | "Ouro 18K Amarelo | #SER-2026-00001" | "R$ 4.500,00" | Green "Disponivel" badge | [Adicionar] gold button
- Result 2: [Necklace photo 60x60] | "Colar Riviera Prata 925" | "Prata 925 | #SER-2026-00089" | "R$ 1.200,00" | Green "Disponivel" badge | [Adicionar] button
- Result 3: [Earring photo 60x60] | "Brinco Argola Rose" | "Ouro 18K Rose | #SER-2026-00156" | "R$ 2.100,00" | Amber "Consignado" badge (disabled, can't add)

RIGHT SIDE (35%) — CART PANEL:
- Header: "Carrinho" with item count badge "(2)"
- Cart items:
  - Item 1: "Anel Solitario Diamante 0.5ct" | #SER-2026-00001 | R$ 4.500,00 | trash icon button
  - Item 2: "Colar Riviera Prata 925" | #SER-2026-00089 | R$ 1.200,00 | trash icon button

- Divider
- "Desconto:" row with toggle [%] [R$] and input field — showing "5%" applied
- Subtotal: R$ 5.700,00 (muted text, strikethrough)
- Desconto: -R$ 285,00 (red text)
- TOTAL: R$ 5.415,00 (large, bold, gold, JetBrains Mono font)

- "Observacoes" — small textarea, 2 rows, placeholder "Gravacao, ajuste de tamanho..."

- Button: "FINALIZAR VENDA" — full width, large, gold bg, bold text

RESPONSIVE: On mobile, cart becomes a floating bottom bar with "Carrinho (2) R$ 5.415" that opens as full-screen bottom sheet when tapped.

COLORS: White bg on both sides, subtle divider between them. Gold accents on buttons and total.
```

### 3.2 PDV — Modal de Pagamento

```
Payment modal for the jewelry store PDV. Appears after clicking "Finalizar Venda".

MODAL: Centered, 520px width, white background, rounded 16px, dark overlay behind.

HEADER: "Finalizar Venda" — bold title, X close button top-right.

CONTENT:
- Large total display: "R$ 5.415,00" in bold gold, 28px, JetBrains Mono, centered

SECTION "Forma de Pagamento":
- Toggle button group (horizontal):
  [Cartao Credito] [Cartao Debito] [PIX] [Dinheiro] [Boleto]
  "Cartao Credito" is selected (gold background)

- "Parcelas:" — dropdown showing "6x de R$ 902,50 (sem juros)" with options 1x to 12x

- Checkbox: "Pagamento misto" — unchecked
  (When checked, shows two rows: "Forma 1: PIX — R$ [2.000,00]" and "Forma 2: Cartao — R$ [3.415,00]" with automatic remainder calculation)

SECTION "Resumo":
- Table-like summary:
  - "Cliente: Maria Oliveira"
  - "Itens: 2 pecas"
  - "Subtotal: R$ 5.700,00"
  - "Desconto (5%): -R$ 285,00"
  - "Total: R$ 5.415,00" (bold)
  - "Pagamento: 6x R$ 902,50 Cartao Credito"

- "Observacoes:" text shown if any

FOOTER:
- Left: "Cancelar" outlined button
- Right: "Confirmar Pagamento" large gold button with check icon

COLORS: #FFFFFF modal, #C9A84C gold, #1A1A1A text
```

### 3.3 Vendas — Historico

```
Sales history page for jewelry store. Data table with filters.

LAYOUT: Admin panel shell.

PAGE HEADER: "Historico de Vendas" — right side: "Nova Venda" gold button (opens PDV)

FILTER BAR (horizontal):
- "Periodo:" [01/03/2026] a [27/03/2026] — date range picker
- "Vendedor:" [Todos ▼] dropdown
- "Status:" [Todos ▼] dropdown (Confirmada, Paga, Cancelada, Devolvida)
- "Canal:" [Todos ▼] dropdown (Loja Fisica, Representante, Consignacao)
- Button "Filtrar" (gold) and "Limpar" (text link)
- Right side: total count "125 vendas encontradas"

DATA TABLE:
- Columns: # Venda | Data | Cliente | Vendedor | Itens | Valor | Status | Acoes
- Row 1: #1234 | 27/03/2026 14:32 | Maria Oliveira | Joao Santos | 2 | R$ 5.415 | green "Paga" badge | eye icon
- Row 2: #1233 | 27/03/2026 11:15 | Pedro Lima | Ana Costa | 1 | R$ 8.900 | green "Paga" badge | eye icon
- Row 3: #1232 | 26/03/2026 16:45 | Julia Costa | Maria Silva | 3 | R$ 12.350 | amber "Confirmada" badge | eye icon
- Row 4: #1231 | 26/03/2026 10:20 | Carlos Souza | Joao Santos | 1 | R$ 650 | red "Cancelada" badge | eye icon
- Row 5: #1230 | 25/03/2026 15:00 | Ana Ferreira | Pedro Lima | 2 | R$ 3.800 | green "Paga" badge | eye icon

Table features: Sortable columns (click header), hover highlight, click row to open details.

PAGINATION: "Mostrando 1-20 de 125" — buttons ← 1 2 3 ... 7 →

SUMMARY BAR (above table, subtle gray bg):
- "Total do periodo: R$ 287.000 | Ticket medio: R$ 2.296 | Vendas: 125 | Cancelamentos: 3"

COLORS: Clean, premium. Gold accents on buttons, green/amber/red badges.
```

### 3.4 Clientes — Listagem

```
Client listing page for jewelry store.

LAYOUT: Admin panel shell.

PAGE HEADER: "Clientes" — right: "Novo Cliente" gold button (plus icon)

SEARCH BAR: "Buscar por nome, CPF, email ou telefone..."

DATA TABLE:
- Columns: Nome | CPF/CNPJ | Telefone | Email | Compras | Ultima Compra | Acoes
- Row 1: Maria Oliveira | 123.456.789-00 | (11) 99999-1234 | maria@email.com | 8 compras | 27/03/2026 | edit, view icons
- Row 2: Pedro Santos | 234.567.890-11 | (11) 98888-5678 | pedro@email.com | 3 compras | 25/03/2026 | edit, view
- Row 3: Joalheria Luxo LTDA | 12.345.678/0001-99 | (11) 3333-4444 | contato@luxo.com | 15 compras | 26/03/2026 | edit, view
- Row 4: Julia Costa | 345.678.901-22 | (21) 97777-8901 | julia@email.com | 1 compra | 20/03/2026 | edit, view

Features: Sortable, paginated, click row to open client details.

PAGINATION: "Mostrando 1-20 de 342 clientes"

COLORS: Premium palette, subtle hover effects.
```

---

## Fase 4 — Consignacao e Representantes

### 4.1 Consignacao — Listagem

```
Consignment listing page for jewelry store. Shows kits sent to representatives.

LAYOUT: Admin panel shell.

PAGE HEADER: "Consignacao" — right: "Novo Kit" gold button (plus icon)

FILTER BAR: [Status ▼] [Representante ▼] [Periodo: __ a __] [Filtrar] [Limpar]
- Tabs below: [Todos (45)] [Ativos (12)] [Pendente Acerto (5)] [Vencidos (2)] [Encerrados (26)]
- "Vencidos" tab has red badge

DATA TABLE:
- Columns: # Kit | Representante | Pecas | Enviado em | Prazo | Vendidas | Valor Total | Status | Acoes
- Row 1: #KIT-0045 | Carlos Silva | 20 | 01/03/2026 | 31/03/2026 | 8/20 | R$ 45.000 | green "Ativo" badge | eye icon
- Row 2: #KIT-0044 | Ana Ferreira | 15 | 15/02/2026 | 15/03/2026 | 12/15 | R$ 28.000 | amber "Pendente Acerto" badge | eye icon
- Row 3: #KIT-0043 | Roberto Lima | 10 | 01/02/2026 | 01/03/2026 | 3/10 | R$ 18.500 | red "Vencido" badge with alert icon | eye icon
- Row 4: #KIT-0042 | Lucia Santos | 25 | 01/01/2026 | 31/01/2026 | 20/25 | R$ 62.000 | gray "Acertado" badge | eye icon

COLORS: Premium, gold accents. Red highlight on overdue items.
```

### 4.2 Consignacao — Detalhes do Kit

```
Consignment kit detail page for jewelry store. Shows all pieces in a kit with individual status.

LAYOUT: Admin panel shell.

BREADCRUMB: Consignacao > #KIT-0045

PAGE HEADER:
- Title: "Consignacao #KIT-0045" with green "Ativo" status badge
- Right: buttons "Adicionar Pecas" (outlined), "Estender Prazo" (outlined), "Iniciar Acerto" (gold, large)

INFO BAR (4 stat cards):
- "Total de Pecas: 20" with package icon
- "Vendidas: 8" with green check icon and green text
- "Devolvidas: 2" with blue return icon
- "Prazo: 15 dias restantes" with amber clock icon

DETAILS CARD:
- "Representante: Carlos Silva" | "Telefone: (11) 99999-0000"
- "Enviado em: 01/03/2026" | "Prazo para acerto: 31/03/2026"
- "Regra de comissao: Escalonada (30-50%)"

FILTER TABS: [Todas (20)] [Em Posse (10)] [Vendidas (8)] [Devolvidas (2)]

PIECES TABLE:
- Columns: Num. Serie | Peca | Metal | Preco Sugerido | Status | Data Venda | Valor Venda | Acoes
- Row 1: SER-0001 | Anel Solitario 0.5ct | Ouro 18K | R$ 4.500 | green "Vendida" | 10/03/2026 | R$ 4.500 | —
- Row 2: SER-0002 | Colar Riviera | Prata 925 | R$ 1.200 | blue "Em Posse" | — | — | buttons "Registrar Venda" "Devolver"
- Row 3: SER-0003 | Brinco Argola | Ouro 18K Rose | R$ 2.100 | gray "Devolvida" | — | — | —
- Row 4: SER-0004 | Pulseira Tennis | Ouro 18K | R$ 8.900 | blue "Em Posse" | — | — | buttons "Registrar Venda" "Devolver"
- (more rows...)

FINANCIAL SUMMARY CARD (bottom, highlighted with gold left border):
- "Resumo Financeiro"
- "Total de pecas enviadas: R$ 45.000,00"
- "Total vendido: R$ 18.500,00"
- "Comissao representante (35%): R$ 6.475,00"
- "Saldo a receber: R$ 12.025,00" (bold, gold)

COLORS: Premium, clean table. Green/blue/gray badges for piece status. Gold financial summary.
```

### 4.3 Consignacao — Novo Kit

```
Create new consignment kit page for jewelry store.

LAYOUT: Admin panel shell. White card centered, max-width 900px.

PAGE HEADER: "Novo Kit de Consignacao" — breadcrumb "Consignacao > Novo Kit"

SECTION 1 — "Representante e Prazo":
- "Representante *" — searchable dropdown showing: "Carlos Silva — (11) 99999-0000" selected. Each option shows name + phone + city.
- "Data limite para acerto *" — date picker, showing "31/03/2026"
- "Regra de comissao *" — dropdown: "Escalonada Padrao (30-50%)", "Fixa 10%", "Consignacao Premium (40-50%)"

SECTION 2 — "Pecas do Kit":
- Search bar: "Buscar peca por nome, codigo ou serie..." (only shows available pieces in stock)
- Search results appear as a selectable list

ADDED PIECES TABLE:
- Columns: # | Num. Serie | Peca | Metal | Pedra | Preco Sugerido | Remover
- Row 1: 1 | SER-0001 | Anel Solitario 0.5ct | Ouro 18K | Diamante | R$ 4.500 (editable input) | red X button
- Row 2: 2 | SER-0002 | Colar Riviera | Prata 925 | Zirconia | R$ 1.200 | red X
- Row 3: 3 | SER-0003 | Brinco Argola | Ouro 18K Rose | — | R$ 2.100 | red X

SUMMARY (right-aligned below table):
- "3 pecas adicionadas | Valor total: R$ 7.800,00"

FOOTER:
- Left: "Cancelar" button
- Right: "Criar Kit" gold button

COLORS: Premium, white card, gold buttons.
```

### 4.4 Representantes — Listagem

```
Sales representatives listing page for jewelry store management.

LAYOUT: Admin panel shell.

PAGE HEADER: "Representantes" — right: "Novo Representante" gold button

SUMMARY CARDS (3):
- "Representantes Ativos: 8" with green dot
- "Pecas em Consignacao: 215" with package icon
- "Comissoes do Mes: R$ 18.400" with gold coin icon

DATA TABLE:
- Columns: Nome | Telefone | Cidade/UF | Pecas em Posse | Vendas Mes | Comissao Mes | Status | Acoes
- Row 1: Carlos Silva | (11) 99999-0000 | Sao Paulo/SP | 20 pecas | R$ 18.500 | R$ 6.475 | green "Ativo" | view icon
- Row 2: Ana Ferreira | (21) 98888-1111 | Rio de Janeiro/RJ | 15 pecas | R$ 22.300 | R$ 8.920 | green "Ativo" | view
- Row 3: Roberto Lima | (31) 97777-2222 | Belo Horizonte/MG | 10 pecas | R$ 5.200 | R$ 1.560 | green "Ativo" | view
- Row 4: Lucia Santos | (41) 96666-3333 | Curitiba/PR | 0 pecas | R$ 0 | R$ 0 | gray "Inativo" | view

COLORS: Premium palette, clean.
```

### 4.5 Representante — Dashboard Pessoal

```
Personal dashboard for a sales representative in the jewelry system. This is what the representative sees after logging in.

LAYOUT: Admin panel shell with SIMPLIFIED sidebar showing only: Dashboard, Pecas em Posse, Minhas Vendas, Acertos.

GREETING: "Ola, Carlos!" (large heading) — "Representante Comercial" in muted text

ROW 1 — KPI CARDS (4):
- "Meta do Mes" — "R$ 30.000" with circular progress 62% in gold, target icon
- "Vendas do Mes" — "R$ 18.500" with upward trend icon in green
- "Comissao Acumulada" — "R$ 6.475" with gold coin icon
- "Pecas em Posse" — "12 pecas" with package icon

ROW 2 — TWO COLUMNS:
Left: "Proximos Acertos" — card list:
  - "#KIT-0045 — Prazo: 31/03/2026 (4 dias)" amber badge "Proximo"
  - "#KIT-0041 — Prazo: 15/04/2026 (19 dias)" gray
  - Button: "Ver todos os acertos →"

Right: "Vendas Recentes" — table:
  - Columns: Data | Peca | Cliente | Valor
  - Row 1: 27/03 | Anel Solitario | Maria | R$ 4.500
  - Row 2: 25/03 | Colar Riviera | Pedro | R$ 1.200
  - Row 3: 23/03 | Brinco Perola | Julia | R$ 1.650

ROW 3 — "Pecas em Posse" (preview, 6 cards in grid):
- Each card: small product thumbnail, name, serial, suggested price
- Button: "Ver todas as pecas →"

COLORS: Premium, gold accents, warm and inviting for the representative.
```

### 4.6 Representante — Pecas em Posse

```
"My pieces" page for a sales representative. Shows all consigned jewelry pieces in their possession.

LAYOUT: Admin shell with simplified sidebar (representative view).

PAGE HEADER: "Pecas em Posse" — right: "Registrar Venda" gold button

SUMMARY: "12 pecas em posse | Valor total: R$ 32.400"

FILTER TABS: [Todas (12)] [Em Posse (10)] [Vendidas (2)] — with search bar next to it

PIECES GRID (cards, 3 columns):
Card 1:
- Product image (ring)
- "Anel Solitario Diamante 0.5ct"
- "Ouro 18K Amarelo | #SER-0001"
- "Preco sugerido: R$ 4.500"
- Blue badge "Em Posse"
- Button: "Registrar Venda" (gold, small)

Card 2:
- Product image (necklace)
- "Colar Riviera Prata 925"
- "Prata 925 | #SER-0002"
- "Preco sugerido: R$ 1.200"
- Blue badge "Em Posse"
- Button: "Registrar Venda"

Card 3:
- Product image (earring)
- "Brinco Argola Rose"
- "Ouro 18K Rose | #SER-0003"
- "Preco sugerido: R$ 2.100"
- Green badge "Vendida — 25/03"
- No button (already sold)

(more cards...)

COLORS: Premium, card-based layout. Blue for "em posse", green for sold. Gold CTA buttons.
```

---

## Dicas para Usar os Prompts no Stitch

1. **Cole o Design System Global primeiro** — isso estabelece a identidade visual para todas as telas
2. **Gere uma tela por vez** — prompts muito longos podem perder detalhes
3. **Itere via chat** — apos gerar, use o chat do Stitch para ajustar ("deixe o sidebar mais escuro", "aumente o tamanho do total no carrinho")
4. **Exporte como React** — o codigo gerado serve como referencia visual, mas sera reescrito com Shadcn/ui + Tailwind na implementacao real
5. **Exporte para Figma** — ideal para ter um design system visual de referencia antes de codar
6. **Ordem sugerida de geracao:**
   - Design System Global
   - Layout Base (AppShell)
   - Login → Registro → Recuperar Senha
   - Dashboard Admin
   - Catalogo Listagem → Cadastro (5 etapas)
   - Estoque Visao Geral
   - PDV → Modal Pagamento
   - Vendas Historico → Clientes
   - Consignacao Listagem → Detalhes → Novo Kit
   - Representantes Listagem → Dashboard Representante → Pecas em Posse
