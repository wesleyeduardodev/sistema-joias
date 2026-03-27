# PRD — Sistema de Gestao e Venda de Joias

> **Versao:** 1.0
> **Data:** 27/03/2026
> **Status:** Aprovado para desenvolvimento

---

## 1. Visao Geral

### 1.1 Problema

O mercado brasileiro de joias carece de um sistema integrado que atenda a todas as necessidades do setor. ERPs generalistas (Bling, Tiny) nao possuem campos especificos para joias (quilate, pedra, certificacao). Sistemas especializados internacionais (PIRO, Jewel360) nao atendem requisitos fiscais brasileiros (NF-e, SPED). Sistemas brasileiros especializados (CDS, Revendus) sao fragmentados — cada um atende apenas uma parte do fluxo.

### 1.2 Solucao

Sistema web completo de gestao e venda de joias que integra: catalogo com atributos especificos do setor, controle de estoque com rastreamento individual por numero de serie, PDV, gestao de consignacao, comissionamento flexivel de representantes, e relatorios gerenciais — tudo em uma unica plataforma.

### 1.3 Stack Tecnologica

| Camada | Tecnologia | Versao |
|---|---|---|
| **Backend** | Spring Boot | 4.0.3 |
| **Linguagem Backend** | Java | 25 |
| **Banco de Dados** | PostgreSQL | 18.3 |
| **Frontend** | React + TypeScript | 19.2.4 |
| **Build Tool** | Vite (Rolldown bundler) | 8.0.3 |
| **Roteamento** | React Router | v7 |
| **UI Components** | Shadcn/ui + Tailwind CSS | Mais recente |
| **Estado Servidor** | TanStack Query (React Query) | Mais recente |
| **Estado Cliente** | Zustand | Mais recente |
| **Formularios** | React Hook Form + Zod | Mais recente |
| **Graficos** | Recharts | Mais recente |
| **Seguranca** | Spring Security + JWT | Incluso no Spring Boot 4.0.3 |
| **ORM** | Spring Data JPA / Hibernate | Incluso no Spring Boot 4.0.3 |
| **Migrations** | Flyway | Mais recente compativel |
| **Build Backend** | Maven | Mais recente |
| **Testes** | JUnit 5 + Mockito + Testcontainers | Mais recente |

---

## 2. Personas

### 2.1 Administrador/Gerente
- **Quem:** Dono da joalheria ou gerente de loja
- **Necessidades:** Visao geral do negocio, controle total de produtos/estoque/vendas, gestao de equipe, relatorios, configuracoes
- **Acesso:** Total — todos os modulos

### 2.2 Vendedor
- **Quem:** Funcionario da loja que atende clientes presencialmente
- **Necessidades:** Registrar vendas (PDV), consultar catalogo, cadastrar clientes, acompanhar metas e comissoes pessoais
- **Acesso:** PDV, catalogo (consulta), clientes, dashboard pessoal

### 2.3 Representante Comercial
- **Quem:** Autonomo que vende joias externamente (B2C ou B2B), frequentemente por consignacao
- **Necessidades:** Ver pecas em sua posse, registrar vendas, acompanhar acertos, metas e comissoes
- **Acesso:** Pecas em posse, registro de vendas, acertos, dashboard pessoal

---

## 3. User Stories (Priorizadas por MoSCoW)

### MUST HAVE (MVP)

| ID | Persona | User Story |
|---|---|---|
| US-01 | Admin | Como admin, quero cadastrar produtos com atributos especificos de joias (metal, quilate, pedra, peso, certificacao, numero de serie) para manter um catalogo completo |
| US-02 | Admin | Como admin, quero controlar o estoque com rastreamento individual de pecas por numero de serie e localizacao (cofre, vitrine, representante) |
| US-03 | Vendedor | Como vendedor, quero registrar vendas pelo PDV buscando produtos e clientes, aplicando descontos e registrando pagamento |
| US-04 | Admin | Como admin, quero gerenciar consignacoes: criar kits de pecas, enviar a representantes, acompanhar status e realizar acerto financeiro |
| US-05 | Representante | Como representante, quero ver as pecas em minha posse, registrar vendas realizadas e acompanhar meus acertos |
| US-06 | Admin | Como admin, quero configurar regras de comissao (fixa, escalonada, por meta) e ter o calculo automatico |
| US-07 | Todos | Como usuario, quero fazer login seguro com email/senha e ter acesso apenas as funcionalidades do meu perfil |
| US-08 | Admin | Como admin, quero ver um dashboard com KPIs: vendas do dia/mes, estoque, metas da equipe, ranking de vendedores |
| US-09 | Admin | Como admin, quero cadastrar e gerenciar clientes com historico de compras |
| US-10 | Admin | Como admin, quero gerenciar categorias e subcategorias de produtos |
| US-11 | Admin | Como admin, quero registrar movimentacoes de estoque (entrada, saida, transferencia, ajuste) |
| US-12 | Admin | Como admin, quero atualizar a cotacao de metais (ouro, prata, platina) e ter o preco dos produtos recalculado |

### SHOULD HAVE

| ID | Persona | User Story |
|---|---|---|
| US-13 | Admin | Como admin, quero gerar relatorios de vendas por periodo, vendedor, categoria e canal |
| US-14 | Admin | Como admin, quero gerar relatorios de estoque: posicao atual, giro, valor em estoque |
| US-15 | Admin | Como admin, quero gerar relatorios de comissoes por vendedor/representante e periodo |
| US-16 | Vendedor | Como vendedor, quero ver meu dashboard pessoal com metas, comissoes e vendas recentes |
| US-17 | Admin | Como admin, quero definir metas de venda para vendedores e representantes e acompanhar progresso |
| US-18 | Admin | Como admin, quero importar produtos em lote via CSV/Excel |
| US-19 | Admin | Como admin, quero cancelar vendas e ter estorno automatico de estoque e comissao |
| US-20 | Admin | Como admin, quero exportar relatorios em PDF e CSV |

### COULD HAVE

| ID | Persona | User Story |
|---|---|---|
| US-21 | Admin | Como admin, quero receber alertas de estoque baixo e consignacoes proximas do vencimento |
| US-22 | Admin | Como admin, quero ver ranking dos produtos mais vendidos e clientes mais ativos |
| US-23 | Admin | Como admin, quero estender prazo e adicionar pecas a consignacoes ativas |
| US-24 | Representante | Como representante, quero ver o historico dos meus acertos e saldo financeiro |

### WON'T HAVE (Futuro)

| ID | Persona | User Story |
|---|---|---|
| US-25 | — | Integracao com e-commerce (Shopify, VTEX, marketplaces) |
| US-26 | — | Emissao de NF-e / integracao com SPED |
| US-27 | — | Modulo de oficina/reparos (ordens de servico) |
| US-28 | — | App mobile nativo para representantes |
| US-29 | — | Virtual try-on (AR) |
| US-30 | — | Integracao com gateway de pagamento |

---

## 4. Requisitos Funcionais

### 4.1 Autenticacao e Autorizacao
- Login com email/senha, tokens JWT (access + refresh)
- 4 roles: ADMIN, GERENTE, VENDEDOR, REPRESENTANTE
- Registro com aprovacao do admin
- Recuperacao de senha por email
- Controle de acesso por rota e endpoint

### 4.2 Catalogo de Produtos
- CRUD completo de produtos com atributos especificos de joias:
  - Metal principal (ouro 10K/14K/18K/24K, prata 925/950, platina, aco, titanio)
  - Cor do metal (amarelo, branco, rose)
  - Pedras (multiplas por peca): tipo, quantidade, quilates, lapidacao, 4Cs para diamantes
  - Peso total e do metal (gramas), peso das pedras (quilates)
  - Acabamento (polido, fosco, escovado, diamantado, texturizado, martelado)
  - Tipo de cravacao (garra, inglesa, pave, trilho, invisivel)
  - Tipo de fecho (trava, mosquetao, pressao, rosca, gaveta)
  - Tamanho/aro com conversao BR/US
  - Numero de serie unico para rastreamento individual
  - Certificacoes (GIA, IGI, HRD, IBGM)
- Upload de multiplas imagens (principal, detalhe, modelo, certificado)
- Categorias e subcategorias (aneis, aliancas, brincos, colares, pulseiras, pingentes, relogios, conjuntos)
- Tipo: JOIA, SEMIJOIA, BIJUTERIA
- Genero: MASCULINO, FEMININO, UNISSEX
- Busca com filtros facetados (categoria, metal, pedra, preco, tamanho, acabamento)
- Importacao em lote (CSV/Excel)
- Recalculo de preco em lote ao atualizar cotacao

### 4.3 Precificacao
- **Formula:** (Custo Metal + Custo Pedras + Mao de Obra + Custos Indiretos) x Markup
- Custo do metal calculado automaticamente: cotacao da grama x peso
- Markup configuravel por produto (tipico: 2.0 a 3.0 para joias finas)
- Preco de venda ajustavel manualmente
- Preco sugerido para consignacao
- Historico de cotacoes de metais (ouro por quilatagem, prata, platina)

### 4.4 Controle de Estoque
- Controle por movimentacoes (entrada, saida, transferencia, ajuste, reserva)
- Localizacoes: COFRE, VITRINE, REPRESENTANTE, TRANSITO, VENDIDO, CONSIGNADO
- Rastreamento individual por numero de serie
- View materializada para saldo atual por produto/localizacao
- Inventario fisico com deteccao de divergencias
- Alertas de estoque baixo

### 4.5 Vendas (PDV)
- Fluxo: selecionar cliente → buscar produtos → montar carrinho → aplicar desconto → finalizar → registrar pagamento
- Formas de pagamento: Dinheiro, PIX, Cartao Credito/Debito, Boleto, Transferencia
- Parcelamento (cartao): 1x a 12x
- Pagamento misto
- Desconto por item ou total (% ou R$)
- Status: RASCUNHO → CONFIRMADA → PAGA → (CANCELADA | DEVOLVIDA)
- Ao confirmar: movimentacao de estoque automatica
- Ao pagar: calculo automatico de comissao
- Cancelamento: estorno de estoque e comissao

### 4.6 Consignacao
- **Fluxo:** Criar kit → Enviar → Acompanhar → Acertar
- Criar kit: selecionar representante, adicionar pecas, definir prazo e regra de comissao
- Enviar: movimenta estoque para CONSIGNADO
- Acompanhar: status individual de cada peca (EM_POSSE, VENDIDO, DEVOLVIDO)
- Acertar: conferir pecas vendidas/devolvidas, calcular comissao (30-50% escalonada), gerar registro financeiro
- Pecas devolvidas retornam ao estoque (CONSIGNADO → COFRE)
- Estender prazo de acerto
- Adicionar pecas a kit ativo
- Listar consignacoes vencidas

### 4.7 Comissionamento
- **Modelos suportados:**
  - Fixa: percentual unico (ex: 3%)
  - Escalonada: faixas por volume (ex: ate R$10K = 5%, R$10K-20K = 8%, acima = 10%)
  - Por meta: bonus ao atingir meta do periodo
- Regras configuraveis por canal de venda e categoria de produto
- Calculo automatico apos confirmacao de pagamento
- Estorno automatico em caso de cancelamento/devolucao
- Periodo de apuracao quinzenal ou mensal
- Fluxo: PENDENTE → APROVADA → PAGA (ou ESTORNADA)

### 4.8 Representantes
- CRUD de representantes (vinculados como usuarios com role REPRESENTANTE)
- Visao de pecas em posse (consignacao ativa)
- Self-service: registrar vendas, consultar acertos, ver historico
- Metas e desempenho por periodo

### 4.9 Clientes
- CRUD com tipo pessoa (fisica/juridica), CPF/CNPJ, enderecos multiplos
- Historico de compras
- Busca com autocomplete para o PDV

### 4.10 Dashboard
- **Admin/Gerente:** vendas hoje/mes (valor + variacao), estoque total + alertas, meta da equipe, grafico de vendas (30 dias), ranking de vendedores, vendas recentes
- **Vendedor:** meta pessoal + progresso, comissoes do mes, vendas recentes
- **Representante:** meta pessoal, comissoes, pecas em posse, proximo acerto

### 4.11 Relatorios
- Vendas por periodo/vendedor/categoria/canal com graficos
- Estoque: posicao atual, giro, valor em estoque por localizacao
- Comissoes por vendedor/representante e periodo
- Consignacoes: status, vencimentos proximos
- Ranking: produtos mais vendidos, clientes mais ativos
- Exportacao em PDF e CSV

---

## 5. Requisitos Nao Funcionais

| Requisito | Especificacao |
|---|---|
| **Performance** | Listagens paginadas (max 20 itens/pagina). Dashboard carrega em < 2s |
| **Seguranca** | JWT com expiracao curta (15min access, 7d refresh). Senhas BCrypt. HTTPS obrigatorio |
| **Responsividade** | Mobile-first. Funcional em telas >= 320px |
| **Acessibilidade** | Componentes Radix (Shadcn/ui) compatíveis com WCAG 2.1 AA |
| **Auditoria** | Campos criadoEm, atualizadoEm, criadoPor, atualizadoPor em todas as entidades |
| **Soft Delete** | Entidades criticas (Produto, Cliente, Venda) usam flag ativo em vez de exclusao |
| **Banco** | PostgreSQL 18.3 com Flyway para versionamento de schema |
| **API** | REST, versionada (/api/v1), documentada com OpenAPI 3 (Swagger UI) |
| **Validacao** | Bean Validation no backend, Zod no frontend |
| **Tratamento de Erros** | @RestControllerAdvice centralizado, ErrorBoundary no React |

---

## 6. Modelo de Dados

### 6.1 Diagrama de Entidades

```
Usuario ──< Venda >── Cliente
   │                    │
   │                    └──< EnderecoCliente
   │
   ├──< Comissao
   │
   └──< Consignacao ──< ItemConsignacao >── Produto
                                              │
Venda ──< ItemVenda >── Produto              │
                          │                   │
                          ├── Categoria ──< Subcategoria
                          ├──< ProdutoPedra >── Pedra
                          ├── Material
                          ├──< ProdutoImagem
                          └──< Certificado

MovimentacaoEstoque >── Produto
CotacaoMetal (historico)
RegraComissao ──< FaixaComissao
MetaVenda >── Usuario
Empresa (registro unico)
```

### 6.2 Entidades Principais

**21 entidades no total:**

| Entidade | Descricao | Chave Primaria |
|---|---|---|
| Usuario | Admin, gerente, vendedor, representante | UUID |
| Cliente | Pessoa fisica ou juridica | UUID |
| EnderecoCliente | Enderecos do cliente (multiplos) | UUID |
| Categoria | Aneis, colares, brincos, etc. | SERIAL |
| Subcategoria | Solitario, riviera, argola, etc. | SERIAL |
| Material | Metais: ouro 18K amarelo, prata 925, etc. | SERIAL |
| Pedra | Tipos: diamante, rubi, zirconia, etc. | SERIAL |
| Produto | Peca individual com numero de serie | UUID |
| ProdutoPedra | Pedras de uma peca especifica (N:N com 4Cs) | UUID |
| ProdutoImagem | Imagens do produto (principal, detalhe, modelo) | UUID |
| Certificado | Certificacao GIA/IGI/HRD da peca | UUID |
| MovimentacaoEstoque | Entrada/saida/transferencia/ajuste | UUID |
| CotacaoMetal | Cotacao diaria do ouro/prata/platina | SERIAL |
| Venda | Venda com status, canal, pagamento | UUID |
| ItemVenda | Itens de uma venda | UUID |
| Consignacao | Kit de consignacao para representante | UUID |
| ItemConsignacao | Pecas do kit com status individual | UUID |
| RegraComissao | Regra de comissao (fixa, escalonada, meta) | SERIAL |
| FaixaComissao | Faixas para comissao escalonada | SERIAL |
| MetaVenda | Meta de venda por periodo/usuario | SERIAL |
| Comissao | Registro de comissao calculada | UUID |
| Empresa | Dados da empresa (registro unico) | SERIAL |

> **Detalhamento completo dos atributos de cada entidade:** ver `arquitetura-backend.md` secao 1.2

---

## 7. API REST

**Base URL:** `/api/v1`

### 7.1 Resumo dos Endpoints (~100+)

| Modulo | Path Base | Endpoints | Roles |
|---|---|---|---|
| Auth | `/auth` | 8 (login, registro, refresh, logout, recuperar/reset senha, me, alterar senha) | Publico / Autenticado |
| Usuarios | `/usuarios` | 6 (CRUD + ativar/desativar) | ADMIN, GERENTE |
| Clientes | `/clientes` | 9 (CRUD + historico + enderecos) | ADMIN, GERENTE, VENDEDOR |
| Categorias | `/categorias` | 5 (CRUD categorias + subcategorias) | ADMIN |
| Materiais | `/materiais` | 3 (CRUD) | ADMIN |
| Pedras | `/pedras` | 3 (CRUD) | ADMIN |
| Produtos | `/produtos` | 12 (CRUD + imagens + certificados + recalculo + importacao) | ADMIN (escrita), Todos (leitura) |
| Estoque | `/estoque` | 9 (posicao + movimentacoes + inventario + resumo + alertas) | ADMIN, GERENTE |
| Cotacoes | `/cotacoes` | 3 (atual + historico + registrar) | ADMIN |
| Vendas | `/vendas` | 8 (CRUD + confirmar + pagar + cancelar + devolver) | ADMIN, GERENTE, VENDEDOR |
| Consignacao | `/consignacoes` | 12 (CRUD + enviar + vender item + devolver item + acertar + estender + adicionar pecas) | ADMIN, GERENTE |
| Comissoes | `/comissoes` | 5 (listar + resumo + aprovar + pagar + estornar) | ADMIN, GERENTE |
| Regras Comissao | `/regras-comissao` | 5 (CRUD com faixas) | ADMIN |
| Metas | `/metas` | 4 (CRUD + progresso) | ADMIN, GERENTE |
| Relatorios | `/relatorios` | 13 (vendas, estoque, comissoes, consignacoes, financeiro, ranking) | ADMIN, GERENTE |
| Dashboard | `/dashboard` | 6 (admin, vendedor, representante + graficos) | Por perfil |
| Representantes | `/representantes` | 7 (CRUD + metas + comissoes + pecas + consignacoes) | ADMIN, GERENTE |
| Self-Service | `/minha-conta` | 5 (pecas, vendas, acertos — para representante logado) | REPRESENTANTE |
| Empresa | `/empresa` | 3 (get + update + logo) | ADMIN |

> **Detalhamento completo dos endpoints com request/response:** ver `arquitetura-backend.md` secao 2

---

## 8. Frontend — Telas e Fluxos

### 8.1 Mapa de Telas (42 telas)

**Publicas (4):** Login, Registro, Recuperar Senha, Redefinir Senha

**Admin/Gerente (28):**
- Dashboard, Catalogo (listagem, novo, detalhes, editar)
- Estoque (visao geral, movimentacoes, inventario)
- Vendas (historico, PDV, detalhes)
- Consignacao (listagem, novo kit, detalhes, acerto)
- Representantes (listagem, cadastro, perfil)
- Clientes (listagem, cadastro, detalhes)
- Relatorios (hub, vendas, estoque, comissoes, ranking)
- Configuracoes (hub, usuarios, regras comissao, cotacoes, empresa)

**Vendedor (6):** Dashboard pessoal, PDV, Minhas Vendas, Catalogo (consulta), Clientes

**Representante (6):** Dashboard pessoal, Pecas em Posse, Registrar Venda, Minhas Vendas, Acertos, Detalhe Acerto

### 8.2 Fluxos Criticos

**Fluxo de Venda (PDV):**
1. Selecionar/cadastrar cliente (autocomplete)
2. Buscar produtos (por nome, codigo, numero de serie)
3. Adicionar ao carrinho com desconto opcional
4. Finalizar → modal de pagamento (forma, parcelas, pagamento misto)
5. Confirmar → estoque atualizado + comissao calculada

**Fluxo de Consignacao:**
1. Criar kit: selecionar representante + pecas + prazo + regra comissao
2. Enviar → pecas movem para CONSIGNADO
3. Acompanhar: status individual (em posse / vendida / devolvida)
4. Acertar: conferir, calcular comissao, registrar financeiro
5. Pecas devolvidas retornam ao estoque

**Fluxo de Cadastro de Produto (Stepper 5 etapas):**
1. Basico: nome, SKU, serie, categoria, subcategoria, tipo, genero
2. Material: metal, quilatagem, cor, pesos, acabamento, fecho, tamanho
3. Pedras: tipo, quantidade, quilates, 4Cs (diamante), certificacao
4. Preco: custo metal (auto), custo pedras, mao de obra, markup, preco final
5. Imagens: upload multiplo drag & drop, reordenacao, preview com zoom

### 8.3 Design System

- **Paleta premium:** preto (#1A1A1A), dourado (#C9A84C), branco off-white (#FAFAFA)
- **Tipografia:** Inter (headings + body), JetBrains Mono (valores)
- **Grid:** sistema de 8px, container max 1440px
- **Sidebar:** 280px desktop, drawer em mobile
- **Mobile-first:** breakpoints sm(640), md(768), lg(1024), xl(1280)

### 8.4 Componentes Reutilizaveis (30+)

**Layout:** AppShell, Sidebar, Header, Breadcrumb, PageHeader
**Data Display:** DataTable, StatCard, Badge, Timeline, ProductCard, EmptyState
**Formularios:** FormField, AutocompleteInput, ImageUpload, FilterPanel, SearchBar, TagInput, CurrencyInput, StepperForm
**Feedback:** Toast, ConfirmDialog, LoadingSpinner, ErrorBoundary, Skeleton
**Dominio:** PriceCalculator, SizeGuide, StoneEditor, CartPanel, ConsignmentStatusBadge, CommissionSummary

> **Detalhamento completo de telas, wireframes e componentes:** ver `arquitetura-frontend.md`

---

## 9. Plano de Fases de Implementacao

### Fase 1 — Fundacao (Semanas 1-2)
**Objetivo:** Estrutura base dos dois projetos

**Backend:**
- Inicializar projeto Spring Boot 4.0.3 com Maven
- Configurar PostgreSQL 18.3, Flyway, Spring Security + JWT
- Implementar entidades base: Usuario, autenticacao (login, registro, refresh, roles)
- Configurar @RestControllerAdvice, DTOs, paginacao
- Migrations V1-V3: usuario, empresa

**Frontend:**
- Inicializar projeto React 19 + Vite 8 + TypeScript
- Configurar Tailwind, Shadcn/ui, React Router v7, TanStack Query, Zustand
- Implementar layout base: AppShell, Sidebar, Header, Breadcrumb
- Implementar autenticacao: Login, Registro, AuthGuard, AuthStore
- Configurar API client (axios/fetch) com interceptor JWT

**Entregavel:** Login funcional, layout base navegavel, deploy de dev

### Fase 2 — Catalogo e Estoque (Semanas 3-5)
**Objetivo:** CRUD de produtos com atributos de joias e controle de estoque

**Backend:**
- Entidades: Categoria, Subcategoria, Material, Pedra, Produto, ProdutoPedra, ProdutoImagem, Certificado
- Entidades de estoque: MovimentacaoEstoque, CotacaoMetal, view estoque_atual
- Endpoints: categorias, materiais, pedras, produtos (CRUD + filtros + imagens + certificados)
- Endpoints: estoque (movimentacoes, posicao, resumo, alertas), cotacoes
- Logica de precificacao: formula custo metal + pedras + mao de obra x markup
- Migrations V4-V7

**Frontend:**
- Telas de catalogo: listagem com filtros facetados, cadastro em stepper (5 etapas), detalhes, edicao
- Telas de estoque: visao geral, movimentacoes, inventario
- Componentes: DataTable, FilterPanel, ImageUpload, StepperForm, PriceCalculator, SearchBar
- Tela de cotacoes (configuracoes)

**Entregavel:** Catalogo completo com busca, filtros e estoque funcional

### Fase 3 — Vendas e PDV (Semanas 6-7)
**Objetivo:** Fluxo completo de vendas

**Backend:**
- Entidades: Venda, ItemVenda, Cliente, EnderecoCliente
- Endpoints: vendas (CRUD + confirmar + pagar + cancelar + devolver)
- Endpoints: clientes (CRUD + historico + enderecos)
- Integracao: movimentacao de estoque automatica ao confirmar venda
- Migrations V8

**Frontend:**
- PDV: busca de produto, carrinho lateral, modal de pagamento, pagamento misto
- Historico de vendas com filtros, detalhes da venda
- Clientes: listagem, cadastro, detalhes com historico
- Componentes: AutocompleteInput, CartPanel, CurrencyInput

**Entregavel:** PDV funcional, gestao de clientes

### Fase 4 — Consignacao e Representantes (Semanas 8-10)
**Objetivo:** Fluxo completo de consignacao e gestao de representantes

**Backend:**
- Entidades: Consignacao, ItemConsignacao
- Endpoints: consignacoes (CRUD + enviar + vender item + devolver + acertar + estender + adicionar pecas)
- Endpoints: representantes (CRUD + metas + pecas + consignacoes)
- Endpoints: self-service /minha-conta (pecas, vendas, acertos)
- Integracao: movimentacao de estoque ao enviar/devolver pecas
- Migrations V9

**Frontend:**
- Consignacao: criar kit, detalhes com status por peca, acerto financeiro
- Representantes: listagem, perfil com metas/comissoes/pecas
- Telas do representante: dashboard, pecas em posse, registrar venda, acertos
- Componentes: ConsignmentStatusBadge

**Entregavel:** Fluxo completo de consignacao, painel do representante

### Fase 5 — Comissionamento e Metas (Semanas 11-12)
**Objetivo:** Calculo automatico de comissoes e metas

**Backend:**
- Entidades: RegraComissao, FaixaComissao, MetaVenda, Comissao
- Endpoints: regras-comissao (CRUD com faixas), metas (CRUD + progresso), comissoes (listar + aprovar + pagar + estornar)
- Logica: calculo automatico de comissao ao registrar pagamento (fixa, escalonada, por meta)
- Estorno automatico ao cancelar venda
- Migration V10

**Frontend:**
- Configuracao de regras de comissao (fixa, escalonada com faixas, por meta)
- Metas: criar, acompanhar progresso
- Comissoes: listagem, aprovar, pagar
- Componentes: CommissionSummary

**Entregavel:** Comissionamento automatizado, gestao de metas

### Fase 6 — Dashboard e Relatorios (Semanas 13-14)
**Objetivo:** Visao gerencial e relatorios

**Backend:**
- Endpoints: dashboard (admin, vendedor, representante)
- Endpoints: 13 relatorios (vendas, estoque, comissoes, consignacoes, financeiro, ranking)
- Exportacao PDF e CSV
- Endpoints: empresa (CRUD + logo)

**Frontend:**
- Dashboard admin: StatCards, grafico de vendas (Recharts), ranking, vendas recentes
- Dashboard vendedor e representante
- Hub de relatorios com filtros, graficos e tabelas exportaveis
- Configuracoes: usuarios, dados da empresa
- Componentes: StatCard, charts (LineChart, BarChart, PieChart)

**Entregavel:** Sistema completo com dashboards e relatorios

### Fase 7 — Polish e QA (Semanas 15-16)
**Objetivo:** Refinamento, testes e preparacao para producao

- Testes unitarios e de integracao (JUnit 5, Testcontainers)
- Testes E2E dos fluxos criticos
- Otimizacao de performance (queries, cache, lazy loading)
- Revisao de responsividade mobile
- Revisao de acessibilidade
- Documentacao API (Swagger UI)
- Configuracao de ambiente de producao

**Entregavel:** Sistema pronto para producao

---

## 10. Arquivos de Referencia

| Arquivo | Conteudo |
|---|---|
| `pesquisa-setor.md` | Pesquisa completa sobre o setor de joias (fluxos, atributos, precificacao, ERPs, UX) |
| `arquitetura-backend.md` | Modelo de dados detalhado (22 entidades com atributos), 100+ endpoints REST com DTOs, regras de negocio, padroes arquiteturais |
| `arquitetura-frontend.md` | 42 telas mapeadas, 4 fluxos detalhados, 6 wireframes descritivos, 30+ componentes, design system, estrutura de rotas |
