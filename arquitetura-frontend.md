# Arquitetura Frontend -- Sistema de Joias

## Stack Tecnologica

| Tecnologia | Funcao | Justificativa |
|---|---|---|
| **React 19.2.4** | Biblioteca UI | Server Components, Actions, use() hook, compilador otimizante, ecossistema maduro |
| **TypeScript** | Tipagem estatica | Seguranca em tempo de desenvolvimento, autocomplete rico |
| **Vite 8.0.3** | Build tool | Rolldown (bundler Rust) nativo, HMR ultra-rapido, build otimizado, suporte nativo a TS |
| **React Router v7** | Roteamento | Framework mode, type-safe routes, loader/action pattern, SSR ready |
| **TanStack Query (React Query)** | Cache e estado servidor | Cache inteligente, invalidacao, retry, loading/error states |
| **Zustand** | Estado global cliente | Leve (~1KB), API simples, sem boilerplate, DevTools |
| **Shadcn/ui** | Componentes UI base | Componentes copiados (nao dependencia), customizaveis via Tailwind, acessiveis (Radix), estetica premium compativel com joalheria |
| **Tailwind CSS** | Estilizacao | Utility-first, design system consistente, responsivo nativo |
| **React Hook Form + Zod** | Formularios e validacao | Performance (uncontrolled), schema validation tipada |
| **Recharts** | Graficos/dashboards | Baseado em SVG, responsivo, API declarativa React |
| **date-fns** | Manipulacao de datas | Imutavel, tree-shakeable, locale pt-BR |

### Justificativa Shadcn/ui sobre alternativas

- **vs Ant Design**: Ant e pesado (~1MB), estetica corporativa generica, dificil de customizar para visual premium/luxo.
- **vs Material UI**: MUI tem identidade visual Google muito forte, dificil desassociar. Bundle grande.
- **Shadcn/ui**: Componentes copiados para o projeto (sem dependencia externa), 100% customizaveis, baseados em Radix (acessibilidade WCAG), estilizados com Tailwind. Permite criar identidade visual premium propria com paleta de luxo (preto, dourado, branco).

---

## Design System e Identidade Visual

### Paleta de Cores

```
--color-primary:     #1A1A1A   (Preto profundo -- fundo, textos)
--color-gold:        #C9A84C   (Dourado -- acentos, CTAs, icones)
--color-gold-light:  #E8D5A3   (Dourado claro -- hover, bordas)
--color-bg:          #FAFAFA   (Branco off-white -- fundo principal)
--color-bg-card:     #FFFFFF   (Branco puro -- cards, modais)
--color-muted:       #6B7280   (Cinza -- textos secundarios)
--color-border:      #E5E7EB   (Cinza claro -- bordas, divisores)
--color-success:     #059669   (Verde -- confirmacoes, lucro)
--color-warning:     #D97706   (Amarelo -- alertas, estoque baixo)
--color-danger:      #DC2626   (Vermelho -- erros, cancelamentos)
```

### Tipografia

- **Headings**: Inter (sans-serif, clean, premium)
- **Body**: Inter
- **Monospace (valores, codigos)**: JetBrains Mono

### Espacamento e Grid

- Sistema de grid de 8px
- Container maximo: 1440px
- Sidebar: 280px (desktop), drawer (mobile)
- Breakpoints: sm(640px), md(768px), lg(1024px), xl(1280px), 2xl(1536px)

---

## Estrutura de Pastas

```
src/
├── app/
│   ├── routes/                    # Definicao de rotas (React Router v7)
│   └── providers.tsx              # Providers globais (Query, Auth, Theme)
├── components/
│   ├── ui/                        # Shadcn/ui base (Button, Input, Dialog, etc.)
│   ├── layout/                    # AppShell, Sidebar, Header, Breadcrumb
│   ├── data-display/              # DataTable, StatCard, Badge, Timeline
│   ├── forms/                     # FormField, ImageUpload, FilterPanel, SearchBar
│   └── feedback/                  # Toast, LoadingSpinner, EmptyState, ErrorBoundary
├── features/
│   ├── auth/                      # Login, registro, recuperacao, guards
│   ├── dashboard/                 # Dashboards por perfil
│   ├── catalogo/                  # Listagem, cadastro, detalhes de produto
│   ├── estoque/                   # Visao geral, movimentacoes, inventario
│   ├── vendas/                    # PDV, historico, detalhes
│   ├── consignacao/               # Kits, envio, acompanhamento, acerto
│   ├── representantes/            # Listagem, cadastro, metas, comissoes
│   ├── clientes/                  # Listagem, detalhes, historico
│   ├── relatorios/                # Vendas, estoque, comissoes, ranking
│   └── configuracoes/             # Usuarios, comissoes, cotacoes, empresa
├── hooks/                         # Hooks globais reutilizaveis
├── lib/                           # Utilitarios, formatadores, constantes
├── services/                      # API client (axios/fetch), endpoints
├── stores/                        # Zustand stores (auth, carrinho PDV, UI)
└── types/                         # Tipos TypeScript globais
```

---

## Mapa de Telas Completo

### Telas Publicas (sem autenticacao)

| Rota | Tela | Descricao |
|---|---|---|
| `/login` | Login | Email + senha, link "esqueci senha" |
| `/registro` | Registro | Formulario de cadastro inicial |
| `/recuperar-senha` | Recuperar Senha | Envio de email de reset |
| `/reset-senha/:token` | Redefinir Senha | Nova senha via token |

### Telas Admin/Gerente

| Rota | Tela | Descricao |
|---|---|---|
| `/` | Dashboard | KPIs: vendas do dia/mes, estoque total, metas da equipe, comissoes, grafico de vendas |
| `/catalogo` | Catalogo - Listagem | Grid/lista de produtos com filtros facetados, busca, paginacao |
| `/catalogo/novo` | Catalogo - Novo Produto | Formulario completo com atributos de joia |
| `/catalogo/:id` | Catalogo - Detalhes | Visualizacao completa do produto |
| `/catalogo/:id/editar` | Catalogo - Editar | Formulario preenchido para edicao |
| `/estoque` | Estoque - Visao Geral | Resumo por categoria/metal, alertas de estoque baixo |
| `/estoque/movimentacoes` | Estoque - Movimentacoes | Historico de entradas/saidas com filtros |
| `/estoque/inventario` | Estoque - Inventario | Conferencia fisica, divergencias |
| `/vendas` | Vendas - Historico | Lista de vendas com filtros (data, vendedor, status) |
| `/vendas/nova` | Vendas - PDV | Ponto de venda: buscar produto, montar carrinho, finalizar |
| `/vendas/:id` | Vendas - Detalhes | Detalhes da venda, itens, pagamento, opcao de cancelar/devolver |
| `/consignacao` | Consignacao - Listagem | Lista de kits com status (ativo, pendente acerto, encerrado) |
| `/consignacao/novo` | Consignacao - Novo Kit | Selecionar representante, adicionar pecas, definir prazo |
| `/consignacao/:id` | Consignacao - Detalhes | Pecas do kit, status individual, acerto financeiro |
| `/consignacao/:id/acerto` | Consignacao - Acerto | Conferencia de pecas vendidas/devolvidas, calculo financeiro |
| `/representantes` | Representantes - Listagem | Lista com nome, status, vendas, comissao acumulada |
| `/representantes/novo` | Representantes - Cadastro | Formulario de cadastro do representante |
| `/representantes/:id` | Representantes - Perfil | Dados, metas, comissoes, historico, pecas em posse |
| `/clientes` | Clientes - Listagem | Lista com busca, filtro por frequencia/valor |
| `/clientes/novo` | Clientes - Cadastro | Formulario de cadastro |
| `/clientes/:id` | Clientes - Detalhes | Dados pessoais, historico de compras, preferencias |
| `/relatorios` | Relatorios - Hub | Menu com tipos de relatorio disponiveis |
| `/relatorios/vendas` | Relatorios - Vendas | Por periodo, vendedor, produto, canal. Graficos + tabela exportavel |
| `/relatorios/estoque` | Relatorios - Estoque | Posicao atual, giro, itens parados, valor em estoque |
| `/relatorios/comissoes` | Relatorios - Comissoes | Por vendedor/representante, periodo, detalhamento |
| `/relatorios/ranking` | Relatorios - Ranking | Top produtos, vendedores, representantes |
| `/configuracoes` | Configuracoes - Hub | Menu de configuracoes |
| `/configuracoes/usuarios` | Config - Usuarios | CRUD de usuarios com perfis/permissoes |
| `/configuracoes/comissoes` | Config - Regras Comissao | CRUD de regras (fixa, escalonada, por meta) com faixas, vinculadas a canal e categoria |
| `/configuracoes/metas` | Config - Metas de Venda | Criar/editar metas por vendedor/representante, com periodo, valor e bonus |
| `/configuracoes/cotacoes` | Config - Cotacoes | Registrar nova cotacao, ver cotacao atual e historico por metal/quilatagem |
| `/configuracoes/empresa` | Config - Dados Empresa | Razao social, CNPJ, endereco, logo (upload via `POST /empresa/logo`) |
| `/comissoes` | Comissoes - Gestao | Listar comissoes por usuario/periodo/status, aprovar, pagar, estornar |
| `/catalogo/importar` | Catalogo - Importacao | Upload CSV/Excel para importacao em lote, resultado com erros |
| `/relatorios/consignacoes` | Relatorios - Consignacoes | Status por periodo/representante, vencimentos proximos |
| `/relatorios/financeiro` | Relatorios - Faturamento | Faturamento geral por periodo com agrupamento dia/semana/mes |

### Telas Vendedor

| Rota | Tela | Descricao |
|---|---|---|
| `/` | Dashboard Pessoal | Metas pessoais, comissoes do mes, vendas recentes |
| `/vendas/nova` | PDV Simplificado | Mesmo PDV do admin |
| `/vendas` | Minhas Vendas | Historico filtrado pelo vendedor logado |
| `/catalogo` | Catalogo (consulta) | Sem opcao de editar/excluir |
| `/clientes` | Clientes | Consulta + cadastro rapido |
| `/clientes/novo` | Novo Cliente | Cadastro simplificado |

### Telas Representante

| Rota | Tela | Descricao |
|---|---|---|
| `/` | Dashboard Pessoal | Metas, comissoes, total de pecas em posse, proximos acertos |
| `/pecas` | Pecas em Posse | Lista de pecas consignadas com status (em posse, vendida, devolvida) |
| `/vendas/nova` | Registrar Venda | Selecionar peca em posse, registrar venda ao cliente |
| `/vendas` | Minhas Vendas | Historico de vendas registradas |
| `/acertos` | Acertos | Historico de acertos, proximo acerto pendente |
| `/acertos/:id` | Detalhe Acerto | Pecas vendidas/devolvidas, saldo financeiro |

---

## Fluxos de Navegacao

### Fluxo 1: Autenticacao (Login)

```
[/login] Email + Senha
   ├── `POST /auth/login` → Credenciais validas → Armazenar accessToken + refreshToken
   │   ├── `GET /auth/me` → Obter perfil e role do usuario
   │   ├── Admin/Gerente → [/ Dashboard Admin]
   │   ├── Vendedor → [/ Dashboard Vendedor]
   │   └── Representante → [/ Dashboard Representante]
   ├── Credenciais invalidas → Mensagem de erro inline, manter na tela
   └── "Esqueci minha senha" → [/recuperar-senha]
       └── `POST /auth/recuperar-senha` → Toast "Email enviado" → [/login]
           └── Link no email → [/reset-senha/:token] → `POST /auth/reset-senha` → [/login]

Token lifecycle:
   ├── Access token expira (15min) → `POST /auth/refresh` automatico via interceptor HTTP
   ├── Refresh token expira (7d) → Redireciona para [/login]
   └── Logout → `POST /auth/logout` (invalida refresh token no servidor) → [/login]
```

### Fluxo 2: Nova Venda (PDV)

```
[/vendas/nova]
   1. Buscar/selecionar cliente (autocomplete ou "Novo cliente" → modal de cadastro rapido)
   2. Buscar produtos (barra de busca por nome/codigo/numero de serie)
      └── Cada produto encontrado exibe: foto thumb, nome, metal, pedra, preco
      └── Clicar "Adicionar" → produto vai ao carrinho lateral
   3. Carrinho lateral (drawer em mobile):
      ├── Lista de itens com preco unitario
      ├── Desconto (% ou R$) por item ou total
      ├── Subtotal e total atualizados em tempo real
      └── Observacoes opcionais
   4. "Finalizar Venda" → `POST /vendas` cria venda com status RASCUNHO
   5. Modal de Confirmacao:
      └── "Confirmar Venda" → `PATCH /vendas/:id/confirmar` (move estoque: VITRINE/COFRE → VENDIDO)
   6. Modal de Pagamento:
      ├── Forma: Dinheiro, Cartao (credito/debito), PIX, Boleto
      ├── Parcelamento (se cartao): 1x a 12x
      └── "Registrar Pagamento" → `PATCH /vendas/:id/registrar-pagamento` (dispara calculo de comissao)
   7. Resultado:
      ├── Venda registrada, estoque atualizado, comissao calculada
      ├── Toast de sucesso
      └── Opcao: "Imprimir recibo" ou "Nova venda"
```

### Fluxo 3: Consignacao (Criar Kit → Enviar → Acompanhar → Acertar)

```
[/consignacao/novo] -- Criar Kit
   1. Selecionar representante (dropdown com busca)
   2. Adicionar pecas ao kit:
      ├── Buscar por codigo/nome/categoria
      ├── Filtrar por disponibilidade (apenas pecas em estoque)
      └── Cada peca mostra: foto, nome, preco sugerido, numero de serie
   3. Definir prazo de acerto (data limite)
   4. Definir regra de comissao (selecionar modelo configurado)
   5. "Criar Kit" → `POST /consignacoes` → Status: CRIADA

[/consignacao/:id] -- Acompanhar
   ├── Acao "Enviar" → `PATCH /consignacoes/:id/enviar` (status CRIADA → ENVIADA, estoque → CONSIGNADO)
   ├── Cabecalho: representante, data envio, prazo, status geral
   ├── Lista de pecas com status individual e acoes por item:
   │   ├── Em posse → acoes: "Registrar Venda" → `PATCH /consignacoes/:id/itens/:itemId/vender`
   │   ├── Vendida (data, cliente, valor) → apenas visualizacao
   │   └── Devolvida → pode marcar via `PATCH /consignacoes/:id/itens/:itemId/devolver`
   ├── Resumo financeiro: total enviado, total vendido, comissao, saldo
   └── Acoes globais: "Iniciar Acerto", "Cancelar" → `PATCH /consignacoes/:id/cancelar` (so se nenhum item vendido)

[/consignacao/:id/acerto] -- Acerto Financeiro
   1. Revisao: tela mostra itens vendidos/devolvidos/em posse
   2. Itens em posse devem ser marcados como vendidos ou devolvidos antes do acerto
   3. "Confirmar Acerto" → `PATCH /consignacoes/:id/acertar`
      ├── Sistema calcula comissao sobre valor vendido (conforme regras)
      ├── Gera registro de comissao
      ├── Status → ACERTADA
      └── Pecas devolvidas retornam ao estoque (CONSIGNADO → COFRE)
```

### Fluxo 4: Cadastro de Produto

```
[/catalogo/novo] -- Formulario em etapas (stepper)

   Etapa 1: Informacoes Basicas
   ├── Nome da peca
   ├── Codigo interno / SKU
   ├── Numero de serie (unico)
   ├── Categoria (anel, colar, brinco, etc.)
   ├── Subcategoria
   ├── Tipo de produto (JOIA, SEMIJOIA, BIJUTERIA)
   ├── Genero (MASCULINO, FEMININO, UNISSEX)
   ├── Descricao (rich text)
   ├── Personalizavel (checkbox -- aceita gravacao/ajuste)
   └── Destaque (checkbox -- exibir em destaque no catalogo)

   Etapa 2: Materiais e Especificacoes
   ├── Metal principal (ouro, prata, platina, etc.)
   ├── Quilatagem (10K, 14K, 18K, 24K)
   ├── Cor do metal (amarelo, branco, rose)
   ├── Peso total (g)
   ├── Peso do metal (g)
   ├── Acabamento (polido, fosco, escovado, etc.)
   ├── Tipo de fecho
   └── Tamanho/aro (com conversao BR/US)

   Etapa 3: Pedras
   ├── Adicionar pedra (pode ter multiplas):
   │   ├── Tipo (diamante, rubi, esmeralda, zirconia, etc.)
   │   ├── Quantidade
   │   ├── Peso total (ct)
   │   ├── Tipo de cravacao (garra, pave, canal, etc.)
   │   └── Se diamante: 4Cs (cor, pureza, lapidacao, quilate)
   └── Certificacao: numero, instituicao (GIA, IGI, HRD), upload do PDF

   Etapa 4: Precificacao
   ├── Custo do metal (auto-calculado via cotacao × peso)
   ├── Custo das pedras
   ├── Custo mao de obra
   ├── Custos indiretos
   ├── Markup (%)
   ├── Preco de venda calculado
   └── Preco de venda final (ajustavel manualmente)

   Etapa 5: Imagens
   ├── Upload multiplo (drag & drop)
   ├── Imagem principal (fundo branco)
   ├── Imagens adicionais (em uso, detalhe, certificado)
   ├── Reordenar por drag & drop
   └── Preview com zoom

   "Salvar" → Produto criado → Redireciona para [/catalogo/:id]
```

---

## Wireframes Descritivos

### 1. Dashboard Admin

```
┌─────────────────────────────────────────────────────────┐
│ [HEADER]                                                │
│  Logo  |  Busca global  |  Notificacoes  |  Avatar ▼   │
├──────────┬──────────────────────────────────────────────┤
│ SIDEBAR  │  AREA PRINCIPAL                              │
│          │                                              │
│ Dashboard│  ┌────────┐ ┌────────┐ ┌────────┐ ┌───────┐ │
│ Catalogo │  │Vendas  │ │Vendas  │ │Estoque │ │Meta   │ │
│ Estoque  │  │Hoje    │ │Mes     │ │Total   │ │Equipe │ │
│ Vendas   │  │R$12.5K │ │R$287K  │ │1.247   │ │78%    │ │
│ Consign. │  │▲12%    │ │▲8%     │ │▼3 alrt │ │       │ │
│ Repres.  │  └────────┘ └────────┘ └────────┘ └───────┘ │
│ Clientes │                                              │
│ Relator. │  ┌──────────────────────────────────────────┐│
│ Config.  │  │ GRAFICO: Vendas ultimos 30 dias          ││
│          │  │ (Linha, com meta tracejada)               ││
│          │  │                                           ││
│          │  └──────────────────────────────────────────┘│
│          │                                              │
│          │  ┌──────────────────┐ ┌─────────────────────┐│
│          │  │ TOP VENDEDORES   │ │ VENDAS RECENTES     ││
│          │  │ 1. Maria  R$45K │ │ #1234 Anel soli... ││
│          │  │ 2. Joao   R$38K │ │ #1233 Colar riv... ││
│          │  │ 3. Ana    R$31K │ │ #1232 Par brinc... ││
│          │  └──────────────────┘ └─────────────────────┘│
└──────────┴──────────────────────────────────────────────┘
```

**Responsividade Mobile:**
- Sidebar colapsa em hamburger menu (drawer deslizante)
- StatCards empilham em 2 colunas (2×2)
- Grafico ocupa largura total
- Top vendedores e vendas recentes empilham verticalmente

**Componentes:** 4× StatCard, 1× LineChart, 1× RankingList, 1× DataTable (vendas recentes)

### 2. Catalogo - Listagem de Produtos

```
┌─────────────────────────────────────────────────────────┐
│ HEADER + SIDEBAR (mesmo layout)                         │
├──────────┬──────────────────────────────────────────────┤
│ SIDEBAR  │  Catalogo de Produtos          [+ Novo]     │
│          │                                              │
│          │  ┌──────────────────────────────────────────┐│
│          │  │ 🔍 Buscar por nome, codigo, serie...    ││
│          │  └──────────────────────────────────────────┘│
│          │                                              │
│          │  FILTROS (horizontal, colapsavel):           │
│          │  [Categoria ▼] [Metal ▼] [Pedra ▼]          │
│          │  [Preco: R$___-R$___] [Tamanho ▼]           │
│          │  [Disponibilidade ▼]  [Limpar filtros]      │
│          │                                              │
│          │  Visualizacao: [Grid] [Lista]   1.247 itens │
│          │                                              │
│          │  ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐           │
│          │  │     │ │     │ │     │ │     │           │
│          │  │ IMG │ │ IMG │ │ IMG │ │ IMG │           │
│          │  │     │ │     │ │     │ │     │           │
│          │  │Nome │ │Nome │ │Nome │ │Nome │           │
│          │  │18K  │ │Prata│ │18K  │ │14K  │           │
│          │  │R$2.5│ │R$890│ │R$5.2│ │R$1.8│           │
│          │  └─────┘ └─────┘ └─────┘ └─────┘           │
│          │                                              │
│          │  ← 1 2 3 ... 52 →                           │
└──────────┴──────────────────────────────────────────────┘
```

**Responsividade Mobile:**
- Grid muda de 4 colunas → 2 colunas
- Filtros colapsam em botao "Filtros" que abre drawer inferior (bottom sheet)
- Busca fica fixa no topo

**Componentes:** SearchBar, FilterPanel, ToggleGroup (grid/lista), ProductCard (grid), DataTable (lista), Pagination

### 3. PDV - Nova Venda

```
┌─────────────────────────────────────────────────────────┐
│ HEADER simplificado: Logo | "PDV" | Vendedor: Maria    │
├──────────────────────────────────┬──────────────────────┤
│ AREA PRINCIPAL (2/3)             │ CARRINHO (1/3)       │
│                                  │                      │
│ Cliente: [Autocomplete____] [+]  │ ITENS:               │
│                                  │                      │
│ ┌──────────────────────────────┐ │ Anel Solitario 18K   │
│ │ 🔍 Buscar produto...        │ │ #SER-0023            │
│ └──────────────────────────────┘ │ R$ 4.500    [×]      │
│                                  │                      │
│ ┌────────────────────────────┐   │ Colar Riviera Prata  │
│ │ IMG | Anel Solitario 18K  │   │ #SER-0089            │
│ │     | Ouro amarelo, diam.  │   │ R$ 1.200    [×]      │
│ │     | R$ 4.500   [Adicionar│   │                      │
│ ├────────────────────────────┤   │─────────────────────│
│ │ IMG | Colar Riviera        │   │ Desconto: [__]% R$__ │
│ │     | Prata 925, zirconia  │   │ Subtotal:  R$ 5.700  │
│ │     | R$ 1.200   [Adicionar│   │ Desconto: -R$ 285    │
│ └────────────────────────────┘   │ TOTAL:     R$ 5.415  │
│                                  │                      │
│                                  │ [FINALIZAR VENDA]    │
└──────────────────────────────────┴──────────────────────┘
```

**Ao clicar "Finalizar Venda" → Modal de pagamento:**
```
┌──────────────────────────────────┐
│ Finalizar Venda                  │
│                                  │
│ Total: R$ 5.415,00               │
│                                  │
│ Forma de pagamento:              │
│ (●) Cartao  ( ) PIX  ( ) Dinhe. │
│                                  │
│ Parcelas: [6x R$ 902,50 ▼]      │
│                                  │
│ [ ] Pagamento misto              │
│                                  │
│ Observacoes: [______________]    │
│                                  │
│ [Cancelar]        [Confirmar]    │
└──────────────────────────────────┘
```

**Responsividade Mobile:**
- Carrinho vira drawer inferior (bottom sheet) com botao flutuante "Carrinho (2)"
- Resultados de busca ocupam tela cheia
- Modal de pagamento ocupa tela inteira (fullscreen dialog)

**Componentes:** AutocompleteInput (cliente), SearchBar, ProductSearchResult, CartDrawer, PaymentModal, NumberInput

### 4. Consignacao - Detalhes do Kit

```
┌─────────────────────────────────────────────────────────┐
│ HEADER + SIDEBAR                                        │
├──────────┬──────────────────────────────────────────────┤
│ SIDEBAR  │  ← Voltar | Consignacao #KIT-0045           │
│          │                                              │
│          │  ┌────────┐ ┌────────┐ ┌────────┐ ┌───────┐ │
│          │  │Pecas   │ │Vendidas│ │Devol.  │ │Prazo  │ │
│          │  │20      │ │8       │ │2       │ │15 dias│ │
│          │  └────────┘ └────────┘ └────────┘ └───────┘ │
│          │                                              │
│          │  Representante: Carlos Silva                  │
│          │  Enviado: 01/03/2026 | Prazo: 31/03/2026     │
│          │  Status: ● Ativo                             │
│          │                                              │
│          │  ┌──────────────────────────────────────────┐│
│          │  │ Filtro: [Todas] [Em posse] [Vendidas]    ││
│          │  ├──────────────────────────────────────────┤│
│          │  │ Serie   | Peca         | Status  | Valor ││
│          │  │ SER-001 | Anel Solit.  | Vendida | 4.500 ││
│          │  │ SER-002 | Colar Riv.   | Em posse| 1.200 ││
│          │  │ SER-003 | Brinco Arg.  | Devol.  |   890 ││
│          │  │ ...     | ...          | ...     | ...   ││
│          │  └──────────────────────────────────────────┘│
│          │                                              │
│          │  Resumo Financeiro:                           │
│          │  Total enviado: R$ 45.000                     │
│          │  Total vendido: R$ 18.500                     │
│          │  Comissao (35%): R$ 6.475                     │
│          │  Saldo empresa: R$ 12.025                     │
│          │                                              │
│          │  [Adicionar Pecas] [Estender Prazo]          │
│          │  [INICIAR ACERTO]                            │
└──────────┴──────────────────────────────────────────────┘
```

**Responsividade Mobile:**
- StatCards em 2×2
- Tabela vira lista de cards empilhados (cada peca = 1 card)
- Resumo financeiro fixo no rodape

### 5. Cadastro de Produto (Formulario em Stepper)

```
┌─────────────────────────────────────────────────────────┐
│ HEADER + SIDEBAR                                        │
├──────────┬──────────────────────────────────────────────┤
│ SIDEBAR  │  Novo Produto                                │
│          │                                              │
│          │  ① Basico  ② Material  ③ Pedras  ④ Preco ⑤  │
│          │  ───●────────○─────────○────────○───────○──  │
│          │                                              │
│          │  ┌──────────────────────────────────────────┐│
│          │  │ Nome *          [____________________]   ││
│          │  │ Codigo/SKU *    [________]               ││
│          │  │ Num. Serie *    [________]               ││
│          │  │                                          ││
│          │  │ Categoria *     [Anel           ▼]      ││
│          │  │ Subcategoria *  [Solitario      ▼]      ││
│          │  │                                          ││
│          │  │ Descricao       [____________________]   ││
│          │  │                 [____________________]   ││
│          │  │                                          ││
│          │  │ Tags            [Noivado ×] [Luxo ×] [+]││
│          │  └──────────────────────────────────────────┘│
│          │                                              │
│          │         [Cancelar]         [Proximo →]       │
└──────────┴──────────────────────────────────────────────┘
```

**Responsividade Mobile:**
- Stepper mostra apenas etapa atual com setas < >
- Campos ocupam largura total (1 coluna)
- Botoes fixos no rodape

### 6. Relatorio de Vendas

```
┌─────────────────────────────────────────────────────────┐
│ HEADER + SIDEBAR                                        │
├──────────┬──────────────────────────────────────────────┤
│ SIDEBAR  │  Relatorio de Vendas            [Exportar ▼]│
│          │                                              │
│          │  Periodo: [01/03/2026] a [27/03/2026]        │
│          │  Vendedor: [Todos ▼]  Canal: [Todos ▼]      │
│          │  [Gerar Relatorio]                           │
│          │                                              │
│          │  ┌────────┐ ┌────────┐ ┌────────┐ ┌───────┐ │
│          │  │Total   │ │Ticket  │ │Qtd     │ │Cancel.│ │
│          │  │Vendas  │ │Medio   │ │Vendas  │ │       │ │
│          │  │R$287K  │ │R$2.3K  │ │125     │ │3      │ │
│          │  └────────┘ └────────┘ └────────┘ └───────┘ │
│          │                                              │
│          │  ┌──────────────────────────────────────────┐│
│          │  │ GRAFICO: Vendas por dia (barras)         ││
│          │  │ Tabs: [Diario] [Semanal] [Por vendedor] ││
│          │  └──────────────────────────────────────────┘│
│          │                                              │
│          │  ┌──────────────────────────────────────────┐│
│          │  │ Data   | Venda  | Cliente | Vend. | Vlr  ││
│          │  │ 27/03  | #1234  | Maria   | Joao  | 4.5K ││
│          │  │ 27/03  | #1233  | Pedro   | Ana   | 1.2K ││
│          │  │ ...    | ...    | ...     | ...   | ...  ││
│          │  ├──────────────────────────────────────────┤│
│          │  │ ← 1 2 3 ... 10 →                        ││
│          │  └──────────────────────────────────────────┘│
└──────────┴──────────────────────────────────────────────┘
```

**Responsividade Mobile:**
- Filtros colapsam em accordion
- Grafico fica horizontalmente scrollavel
- Tabela vira cards empilhados

---

## Componentes React Reutilizaveis

### Layout

| Componente | Props principais | Descricao |
|---|---|---|
| `AppShell` | `sidebar`, `header`, `children` | Layout principal com sidebar colapsavel |
| `Sidebar` | `items`, `collapsed`, `onToggle` | Navegacao lateral com icones, agrupamento, badge de contagem |
| `Header` | `title`, `actions`, `breadcrumb` | Barra superior com busca global, notificacoes, avatar |
| `Breadcrumb` | `items: {label, href}[]` | Navegacao hierarquica |
| `PageHeader` | `title`, `subtitle`, `actions` | Titulo da pagina com botoes de acao |

### Data Display

| Componente | Props principais | Descricao |
|---|---|---|
| `DataTable` | `columns`, `data`, `sort`, `pagination`, `onRowClick` | Tabela com ordenacao, paginacao, selecao de linhas, acoes por linha |
| `StatCard` | `title`, `value`, `change`, `icon`, `trend` | Card de KPI com valor, variacao percentual e icone |
| `Badge` | `variant`, `children` | Status visual (ativo, pendente, cancelado, etc.) |
| `Timeline` | `items: {date, title, description}[]` | Linha do tempo de eventos/movimentacoes |
| `ProductCard` | `product`, `onClick`, `variant` | Card de produto com imagem, nome, metal, preco |
| `EmptyState` | `icon`, `title`, `description`, `action` | Estado vazio com ilustracao e CTA |

### Formularios

| Componente | Props principais | Descricao |
|---|---|---|
| `FormField` | `label`, `name`, `type`, `rules`, `error` | Wrapper para inputs com label, validacao, erro. Integra com React Hook Form |
| `AutocompleteInput` | `options`, `onSearch`, `onSelect`, `renderOption` | Input com busca e sugestoes (para cliente, produto) |
| `ImageUpload` | `maxFiles`, `accept`, `onUpload`, `preview` | Upload drag&drop com preview, reordenacao, zoom |
| `FilterPanel` | `filters: FilterConfig[]`, `values`, `onChange` | Painel de filtros facetados configuravel |
| `SearchBar` | `placeholder`, `onSearch`, `suggestions` | Barra de busca com debounce e sugestoes |
| `TagInput` | `tags`, `suggestions`, `onAdd`, `onRemove` | Input para adicionar/remover tags |
| `CurrencyInput` | `value`, `onChange`, `currency` | Input monetario com mascara R$ |
| `StepperForm` | `steps`, `currentStep`, `onNext`, `onBack` | Formulario em etapas com navegacao |

### Feedback

| Componente | Props principais | Descricao |
|---|---|---|
| `Toast` | `title`, `description`, `variant` | Notificacao temporaria (sucesso, erro, info) |
| `ConfirmDialog` | `title`, `description`, `onConfirm`, `onCancel` | Modal de confirmacao para acoes destrutivas |
| `LoadingSpinner` | `size`, `fullscreen` | Indicador de carregamento |
| `ErrorBoundary` | `fallback` | Captura erros React e exibe fallback |
| `Skeleton` | `variant`, `lines` | Placeholder de carregamento (shimmer) |

### Especificos do Dominio

| Componente | Props principais | Descricao |
|---|---|---|
| `PriceCalculator` | `metalWeight`, `metalType`, `stones`, `markup` | Calculadora de preco com base em cotacao + custos |
| `SizeGuide` | `category`, `system` | Guia de tamanho com conversao BR/US |
| `StoneEditor` | `stones`, `onAdd`, `onRemove`, `onEdit` | Editor de pedras com campos dos 4Cs para diamantes |
| `CartPanel` | `items`, `discount`, `onRemove`, `onCheckout` | Painel de carrinho do PDV |
| `ConsignmentStatusBadge` | `status` | Badge especifico para status de consignacao |
| `CommissionSummary` | `sales`, `rules`, `period` | Resumo de comissoes calculadas |

---

## Estrutura de Rotas (React Router v7)

```tsx
// src/app/routes/index.tsx

<Routes>
  {/* Publicas */}
  <Route element={<AuthLayout />}>
    <Route path="/login" element={<Login />} />
    <Route path="/registro" element={<Registro />} />
    <Route path="/recuperar-senha" element={<RecuperarSenha />} />
    <Route path="/reset-senha/:token" element={<ResetSenha />} />
  </Route>

  {/* Protegidas - requer autenticacao */}
  <Route element={<AuthGuard />}>
    <Route element={<AppShell />}>

      {/* Dashboard - renderiza por perfil */}
      <Route path="/" element={<Dashboard />} />

      {/* Catalogo */}
      <Route path="/catalogo" element={<CatalogoListagem />} />
      <Route path="/catalogo/novo" element={<CatalogoForm />} />      {/* admin */}
      <Route path="/catalogo/:id" element={<CatalogoDetalhes />} />
      <Route path="/catalogo/:id/editar" element={<CatalogoForm />} /> {/* admin */}

      {/* Estoque (admin) */}
      <Route path="/estoque" element={<EstoqueVisaoGeral />} />
      <Route path="/estoque/movimentacoes" element={<EstoqueMovimentacoes />} />
      <Route path="/estoque/inventario" element={<EstoqueInventario />} />

      {/* Vendas */}
      <Route path="/vendas" element={<VendasHistorico />} />
      <Route path="/vendas/nova" element={<VendasPDV />} />
      <Route path="/vendas/:id" element={<VendasDetalhes />} />

      {/* Consignacao (admin) */}
      <Route path="/consignacao" element={<ConsignacaoListagem />} />
      <Route path="/consignacao/novo" element={<ConsignacaoNovoKit />} />
      <Route path="/consignacao/:id" element={<ConsignacaoDetalhes />} />
      <Route path="/consignacao/:id/acerto" element={<ConsignacaoAcerto />} />

      {/* Representantes (admin) */}
      <Route path="/representantes" element={<RepresentantesListagem />} />
      <Route path="/representantes/novo" element={<RepresentantesForm />} />
      <Route path="/representantes/:id" element={<RepresentantesPerfil />} />

      {/* Clientes */}
      <Route path="/clientes" element={<ClientesListagem />} />
      <Route path="/clientes/novo" element={<ClientesForm />} />
      <Route path="/clientes/:id" element={<ClientesDetalhes />} />

      {/* Representante - telas exclusivas (consome /api/v1/minha-conta/*) */}
      <Route path="/pecas" element={<PecasEmPosse />} />
      <Route path="/minhas-vendas" element={<MinhasVendasRepresentante />} />
      <Route path="/acertos" element={<AcertosListagem />} />
      <Route path="/acertos/:id" element={<AcertoDetalhes />} />

      {/* Comissoes - gestao (admin) */}
      <Route path="/comissoes" element={<ComissoesGestao />} />

      {/* Importacao (admin) */}
      <Route path="/catalogo/importar" element={<CatalogoImportacao />} />

      {/* Relatorios (admin) */}
      <Route path="/relatorios" element={<RelatoriosHub />} />
      <Route path="/relatorios/vendas" element={<RelatorioVendas />} />
      <Route path="/relatorios/estoque" element={<RelatorioEstoque />} />
      <Route path="/relatorios/comissoes" element={<RelatorioComissoes />} />
      <Route path="/relatorios/ranking" element={<RelatorioRanking />} />
      <Route path="/relatorios/consignacoes" element={<RelatorioConsignacoes />} />
      <Route path="/relatorios/financeiro" element={<RelatorioFinanceiro />} />

      {/* Configuracoes (admin) */}
      <Route path="/configuracoes" element={<ConfiguracoesHub />} />
      <Route path="/configuracoes/usuarios" element={<ConfigUsuarios />} />
      <Route path="/configuracoes/comissoes" element={<ConfigComissoes />} />
      <Route path="/configuracoes/metas" element={<ConfigMetas />} />
      <Route path="/configuracoes/cotacoes" element={<ConfigCotacoes />} />
      <Route path="/configuracoes/empresa" element={<ConfigEmpresa />} />

    </Route>
  </Route>

  <Route path="*" element={<NotFound />} />
</Routes>
```

### Controle de Acesso por Perfil

```tsx
// Componente de guarda por role
<RoleGuard roles={["admin", "gerente"]}>
  <Route ... />
</RoleGuard>
```

A sidebar renderiza itens condicionalmente com base no perfil:
- **Admin/Gerente**: todos os modulos
- **Vendedor**: Dashboard, Vendas, Catalogo (somente leitura), Clientes
- **Representante**: Dashboard, Pecas em Posse, Vendas, Acertos

---

## Gerenciamento de Estado

### Zustand Stores (Estado Global do Cliente)

```typescript
// stores/authStore.ts
interface AuthStore {
  user: User | null;
  token: string | null;
  role: 'admin' | 'gerente' | 'vendedor' | 'representante';
  login: (credentials: LoginDTO) => Promise<void>;
  logout: () => void;
}

// stores/pdvStore.ts
interface PDVStore {
  cliente: Cliente | null;
  itens: CartItem[];
  desconto: { tipo: 'percentual' | 'valor'; valor: number };
  addItem: (produto: Produto) => void;
  removeItem: (produtoId: string) => void;
  setCliente: (cliente: Cliente) => void;
  setDesconto: (desconto: Desconto) => void;
  getTotal: () => number;
  limpar: () => void;
}

// stores/uiStore.ts
interface UIStore {
  sidebarCollapsed: boolean;
  toggleSidebar: () => void;
  theme: 'light' | 'dark';
}
```

### TanStack Query (Estado do Servidor)

Cada feature define seus proprios hooks de query:

```typescript
// features/catalogo/hooks/useProdutos.ts
export function useProdutos(filtros: ProdutoFiltros) {
  return useQuery({
    queryKey: ['produtos', filtros],
    queryFn: () => api.produtos.listar(filtros),
    staleTime: 5 * 60 * 1000, // 5 min cache
  });
}

// features/catalogo/hooks/useProdutoMutation.ts
export function useCriarProduto() {
  const queryClient = useQueryClient();
  return useMutation({
    mutationFn: api.produtos.criar,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['produtos'] });
    },
  });
}
```

**Principio**: Zustand apenas para estado puramente do cliente (auth, carrinho PDV, preferencias de UI). Dados vindos da API sempre via TanStack Query (cache, revalidacao, loading states automaticos).

---

## Padroes e Convencoes

### Organizacao de Features

Cada feature segue a estrutura:
```
features/catalogo/
├── components/        # Componentes especificos da feature
├── hooks/             # Hooks (queries, mutations, logica)
├── pages/             # Paginas/telas da feature
├── types.ts           # Tipos da feature
└── index.ts           # Re-export publico
```

### Formularios (React Hook Form + Zod)

```typescript
// Todos os formularios seguem este padrao:
const schema = z.object({
  nome: z.string().min(1, 'Nome obrigatorio'),
  preco: z.number().positive('Preco deve ser positivo'),
  // ...
});

type FormData = z.infer<typeof schema>;

function ProdutoForm() {
  const form = useForm<FormData>({
    resolver: zodResolver(schema),
  });
  // ...
}
```

### API Client

```typescript
// services/api.ts
// Cliente HTTP centralizado com interceptors para:
// - Token de autenticacao (Authorization header)
// - Refresh token automatico
// - Tratamento global de erros (401 → logout, 500 → toast)
// - Base URL configuravel por ambiente
```

### Responsividade

- Mobile-first: estilos base para mobile, media queries para desktop
- Breakpoint principal: `lg (1024px)` — abaixo disso, sidebar colapsa, tabelas viram cards
- Touch targets minimos: 44×44px em mobile
- Imagens com `srcset` e lazy loading

### Performance

- Code splitting por rota (lazy routes via React Router v7 + `Suspense`)
- Imagens otimizadas (WebP, lazy load, placeholder blur)
- Virtualizacao de listas longas (`@tanstack/react-virtual`)
- Debounce em buscas (300ms)
- Paginacao server-side para todas as listagens

---

## Mapeamento Frontend ↔ Backend (Endpoints Reais da API)

Todos os endpoints abaixo usam o prefixo `/api/v1`. Os paths refletem exatamente os endpoints definidos na arquitetura backend.

| Tela | Endpoints consumidos |
|---|---|
| **Dashboard Admin** | `GET /dashboard/admin`, `GET /dashboard/admin/grafico-vendas?periodo=30d`, `GET /dashboard/admin/vendas-recentes`, `GET /dashboard/admin/ranking-vendedores` |
| **Dashboard Vendedor** | `GET /dashboard/vendedor` (usa token do usuario logado) |
| **Dashboard Representante** | `GET /dashboard/representante` (usa token do usuario logado) |
| **Catalogo Listagem** | `GET /produtos?q=&categoriaId=&materialId=&pedraId=&tipoProduto=&genero=&precoMin=&precoMax=&acabamento=&destaque=&ativo=&page=&size=&sort=` |
| **Catalogo Form (criar)** | `POST /produtos`, `POST /produtos/:id/imagens`, `POST /produtos/:id/certificados`, `GET /categorias`, `GET /materiais`, `GET /pedras`, `GET /cotacoes/atual` |
| **Catalogo Form (editar)** | `GET /produtos/:id`, `PUT /produtos/:id`, `POST /produtos/:id/imagens`, `DELETE /produtos/:id/imagens/:imgId`, `POST /produtos/:id/certificados` |
| **Catalogo Detalhes** | `GET /produtos/:id` (retorna ProdutoDetalheDTO com pedras, imagens, certificados, estoqueDisponivel, localizacao), `POST /produtos/:id/recalcular-preco` |
| **Estoque Visao Geral** | `GET /estoque?localizacao=&categoriaId=&page=&size=`, `GET /relatorios/estoque/valor`, `GET /consignacoes/vencidas` |
| **Estoque Movimentacoes** | `GET /estoque/produto/:produtoId?page=&size=`, `POST /estoque/entrada`, `POST /estoque/saida`, `POST /estoque/transferencia`, `POST /estoque/ajuste` |
| **Estoque Inventario** | `GET /estoque/inventario?localizacao=&formato=json`, `POST /estoque/ajuste` |
| **Vendas PDV** | `GET /clientes?q=`, `GET /produtos?q=&ativo=true`, `POST /vendas` (cria com status RASCUNHO), `PATCH /vendas/:id/confirmar`, `PATCH /vendas/:id/registrar-pagamento` |
| **Vendas Historico** | `GET /vendas?status=&canalVenda=&vendedorId=&clienteId=&de=&ate=&page=&size=` |
| **Vendas Detalhes** | `GET /vendas/:id` (retorna VendaDetalheDTO), `PATCH /vendas/:id/cancelar`, `PATCH /vendas/:id/devolver` |
| **Consignacao Listagem** | `GET /consignacoes?status=&representanteId=&de=&ate=&page=&size=`, `GET /consignacoes/vencidas` |
| **Consignacao Novo Kit** | `POST /consignacoes`, `GET /representantes/ativos`, `GET /produtos?ativo=true` (filtrar disponiveis) |
| **Consignacao Detalhes** | `GET /consignacoes/:id` (retorna ConsignacaoDetalheDTO com itens e status individual), `PATCH /consignacoes/:id/enviar`, `PATCH /consignacoes/:id/itens/:itemId/vender`, `PATCH /consignacoes/:id/itens/:itemId/devolver`, `PATCH /consignacoes/:id/cancelar` |
| **Consignacao Acerto** | `GET /consignacoes/:id` (revisar itens), `PATCH /consignacoes/:id/acertar` |
| **Representantes Listagem** | `GET /representantes?ativo=&q=&page=&size=` |
| **Representantes CRUD** | `POST /representantes`, `PUT /representantes/:id` |
| **Representante Perfil** | `GET /representantes/:id`, `GET /representantes/:id/metas`, `GET /representantes/:id/comissoes?periodo=`, `GET /representantes/:id/pecas`, `GET /representantes/:id/consignacoes?status=&page=&size=` |
| **Clientes Listagem** | `GET /clientes?q=&page=&size=` |
| **Clientes CRUD** | `POST /clientes`, `PUT /clientes/:id`, `DELETE /clientes/:id` |
| **Clientes Detalhes** | `GET /clientes/:id`, `GET /clientes/:id/historico-compras?page=&size=`, `POST /clientes/:id/enderecos`, `PUT /clientes/:id/enderecos/:endId`, `DELETE /clientes/:id/enderecos/:endId` |
| **Pecas em Posse (Rep.)** | `GET /minha-conta/pecas` |
| **Registrar Venda (Rep.)** | `POST /minha-conta/vendas` |
| **Minhas Vendas (Rep.)** | `GET /minha-conta/vendas?de=&ate=&page=&size=` |
| **Acertos (Rep.)** | `GET /minha-conta/acertos?page=&size=`, `GET /minha-conta/acertos/:consignacaoId` |
| **Relatorio Vendas** | `GET /relatorios/vendas?de=&ate=&canalVenda=&vendedorId=&agrupamento=dia|semana|mes`, `GET /relatorios/vendas/por-categoria?de=&ate=`, `GET /relatorios/vendas/por-vendedor?de=&ate=` |
| **Relatorio Estoque** | `GET /relatorios/estoque/posicao?localizacao=&categoriaId=`, `GET /relatorios/estoque/giro?de=&ate=&categoriaId=`, `GET /relatorios/estoque/valor` |
| **Relatorio Comissoes** | `GET /relatorios/comissoes?de=&ate=&usuarioId=`, `GET /relatorios/comissoes/ranking?de=&ate=` |
| **Relatorio Ranking** | `GET /relatorios/produtos/mais-vendidos?de=&ate=&limit=20`, `GET /relatorios/clientes/mais-ativos?de=&ate=&limit=20`, `GET /relatorios/vendas/por-vendedor?de=&ate=` |
| **Relatorio Consignacoes** | `GET /relatorios/consignacoes?de=&ate=&representanteId=`, `GET /relatorios/consignacoes/vencimentos?diasAteVencimento=7` |
| **Relatorio Financeiro** | `GET /relatorios/financeiro/faturamento?de=&ate=&agrupamento=dia|semana|mes` |
| **Config Usuarios** | `GET /usuarios?role=&ativo=&page=&size=`, `GET /usuarios/:id`, `POST /usuarios`, `PUT /usuarios/:id`, `PATCH /usuarios/:id/ativar`, `PATCH /usuarios/:id/desativar` |
| **Config Comissoes** | `GET /regras-comissao`, `GET /regras-comissao/:id`, `POST /regras-comissao`, `PUT /regras-comissao/:id`, `DELETE /regras-comissao/:id` |
| **Config Metas** | `GET /metas?usuarioId=&periodo=`, `POST /metas`, `PUT /metas/:id`, `GET /metas/:id/progresso` |
| **Config Cotacoes** | `GET /cotacoes/atual`, `GET /cotacoes/historico?tipoMetal=&de=&ate=`, `POST /cotacoes` |
| **Config Empresa** | `GET /empresa`, `PUT /empresa`, `POST /empresa/logo` |
| **Auth** | `POST /auth/login`, `POST /auth/registro`, `POST /auth/refresh`, `POST /auth/logout`, `POST /auth/recuperar-senha`, `POST /auth/reset-senha`, `GET /auth/me`, `PUT /auth/alterar-senha` |
| **Importacao Produtos** | `POST /produtos/importar` (CSV/Excel), `POST /produtos/recalcular-precos` (lote por categoria/material) |
| **Comissoes (gestao)** | `GET /comissoes?usuarioId=&status=&periodo=&page=&size=`, `GET /comissoes/resumo?periodo=&usuarioId=`, `PATCH /comissoes/:id/aprovar`, `PATCH /comissoes/:id/pagar`, `PATCH /comissoes/:id/estornar` |
