# Arquitetura Backend — Sistema de Joias

## Stack Tecnologica

| Componente | Tecnologia |
|---|---|
| Linguagem | Java 24+ (compativel com Spring Boot 4.x) |
| Framework | Spring Boot 4.0.3 |
| Seguranca | Spring Security + JWT (access + refresh token) |
| Persistencia | Spring Data JPA / Hibernate |
| Banco de Dados | PostgreSQL 18.3 |
| Migrations | Flyway |
| Validacao | Bean Validation (Jakarta Validation) |
| Documentacao API | SpringDoc OpenAPI 3 (Swagger UI) |
| Build | Maven |
| Testes | JUnit 5 + Mockito + Testcontainers |
| Cache | Spring Cache + Redis (cotacoes, catalogo) |
| Filas | Spring AMQP + RabbitMQ (eventos assincronos) |

## Padroes Arquiteturais

- **Camadas**: Controller → Service → Repository
- **DTOs** separados de entidades JPA (request/response DTOs distintos)
- **Exception Handler** centralizado via `@RestControllerAdvice`
- **Auditoria**: campos `criadoEm`, `atualizadoEm`, `criadoPor`, `atualizadoPor` em todas as entidades via `@MappedSuperclass`
- **Soft delete** em entidades criticas (Produto, Cliente, Venda)
- **Paginacao** padrao via `Pageable` do Spring Data
- **Filtros dinamicos** via Specification (Spring Data JPA Specifications)

---

## 1. Modelo de Dados Conceitual (PostgreSQL)

### 1.1 Diagrama de Entidades

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
                          ├── Certificado
                          └──< MovimentacaoEstoque

CotacaoMetal (historico)
RegraComissao
MetaVenda
```

### 1.2 Entidades e Atributos

#### Usuario
Representa administradores, gerentes, vendedores e representantes.

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | Identificador unico |
| nome | VARCHAR(200) | Nome completo |
| email | VARCHAR(255) UNIQUE | Email (usado como login) |
| senha_hash | VARCHAR(255) | Senha criptografada (BCrypt) |
| role | ENUM | ADMIN, GERENTE, VENDEDOR, REPRESENTANTE |
| telefone | VARCHAR(20) | Telefone de contato |
| cpf | VARCHAR(14) UNIQUE | CPF |
| ativo | BOOLEAN | Status ativo/inativo |
| percentual_comissao_padrao | DECIMAL(5,2) | Comissao padrao do usuario (%) |
| criado_em | TIMESTAMP | Data de criacao |
| atualizado_em | TIMESTAMP | Data de atualizacao |

#### Cliente

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | Identificador unico |
| tipo_pessoa | ENUM | FISICA, JURIDICA |
| nome | VARCHAR(200) | Nome completo / Razao social |
| cpf_cnpj | VARCHAR(18) UNIQUE | CPF ou CNPJ |
| email | VARCHAR(255) | Email |
| telefone | VARCHAR(20) | Telefone principal |
| telefone_secundario | VARCHAR(20) | Telefone secundario |
| data_nascimento | DATE | Data de nascimento |
| genero | ENUM | MASCULINO, FEMININO, OUTRO, NAO_INFORMADO |
| observacoes | TEXT | Notas sobre o cliente |
| ativo | BOOLEAN | Soft delete flag |
| criado_em | TIMESTAMP | |
| atualizado_em | TIMESTAMP | |

#### EnderecoCliente

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | |
| cliente_id | UUID (FK) | Referencia ao cliente |
| tipo | ENUM | RESIDENCIAL, COMERCIAL, ENTREGA |
| logradouro | VARCHAR(300) | Rua/Av |
| numero | VARCHAR(20) | Numero |
| complemento | VARCHAR(100) | Complemento |
| bairro | VARCHAR(100) | Bairro |
| cidade | VARCHAR(100) | Cidade |
| uf | CHAR(2) | Estado |
| cep | VARCHAR(10) | CEP |
| principal | BOOLEAN | Endereco principal? |

#### Categoria

| Coluna | Tipo | Descricao |
|---|---|---|
| id | SERIAL (PK) | |
| nome | VARCHAR(100) UNIQUE | Ex: Aneis, Aliancas, Brincos, Colares |
| descricao | TEXT | |
| ativo | BOOLEAN | |
| ordem_exibicao | INTEGER | Ordem no menu/catalogo |

#### Subcategoria

| Coluna | Tipo | Descricao |
|---|---|---|
| id | SERIAL (PK) | |
| categoria_id | INTEGER (FK) | |
| nome | VARCHAR(100) | Ex: Solitario, Meia-alianca |
| descricao | TEXT | |
| ativo | BOOLEAN | |
| ordem_exibicao | INTEGER | |
| **UNIQUE** | (categoria_id, nome) | |

#### Material

Tabela de referencia para metais.

| Coluna | Tipo | Descricao |
|---|---|---|
| id | SERIAL (PK) | |
| nome | VARCHAR(100) | Ex: Ouro 18K Amarelo |
| tipo_metal | ENUM | OURO, PRATA, PLATINA, ACO, TITANIO, OUTRO |
| quilatagem | VARCHAR(10) | Ex: 18K, 925, 950 |
| cor | VARCHAR(50) | Amarelo, Branco, Rose |
| pureza_percentual | DECIMAL(5,2) | Ex: 75.00 para 18K |
| ativo | BOOLEAN | |

#### Pedra

Tabela de referencia para tipos de pedra.

| Coluna | Tipo | Descricao |
|---|---|---|
| id | SERIAL (PK) | |
| nome | VARCHAR(100) | Ex: Diamante, Rubi, Zirconia |
| tipo | ENUM | PRECIOSA, SEMIPRECIOSA, SINTETICA |
| ativo | BOOLEAN | |

#### Produto

Entidade central. Representa uma peca individual (com numero de serie) ou um modelo de produto.

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | |
| codigo_interno | VARCHAR(50) UNIQUE | Codigo SKU/interno |
| numero_serie | VARCHAR(100) UNIQUE | Numero de serie unico da peca |
| nome | VARCHAR(300) | Nome do produto |
| descricao | TEXT | Descricao detalhada |
| categoria_id | INTEGER (FK) | |
| subcategoria_id | INTEGER (FK) | |
| material_id | INTEGER (FK) | Metal principal |
| peso_total_gramas | DECIMAL(10,3) | Peso total da peca |
| peso_metal_gramas | DECIMAL(10,3) | Peso do metal |
| tamanho | VARCHAR(20) | Aro (aneis), comprimento (colares/pulseiras) |
| acabamento | ENUM | POLIDO, FOSCO, ESCOVADO, DIAMANTADO, TEXTURIZADO, MARTELADO |
| tipo_cravacao | ENUM | GARRA, INGLESA, PAVE, TRILHO, INVISIVEL, NENHUMA |
| tipo_fecho | ENUM | TRAVA, MOSQUETAO, PRESSAO, ROSCA, GAVETA, NENHUM |
| tipo_produto | ENUM | JOIA, SEMIJOIA, BIJUTERIA |
| custo_mao_obra | DECIMAL(12,2) | Custo de fabricacao |
| custos_indiretos | DECIMAL(12,2) | Embalagem, certificacao, etc |
| markup | DECIMAL(5,2) | Multiplicador de margem (ex: 2.50) |
| preco_custo_calculado | DECIMAL(12,2) | Custo total calculado |
| preco_venda | DECIMAL(12,2) | Preco final de venda |
| preco_venda_sugerido | DECIMAL(12,2) | Para consignacao |
| genero | ENUM | MASCULINO, FEMININO, UNISSEX |
| personalizavel | BOOLEAN | Aceita gravacao/ajuste |
| destaque | BOOLEAN | Produto em destaque no catalogo |
| ativo | BOOLEAN | Soft delete |
| criado_em | TIMESTAMP | |
| atualizado_em | TIMESTAMP | |

#### ProdutoPedra

Relacionamento N:N entre Produto e Pedra, com atributos da pedra naquela peca especifica.

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | |
| produto_id | UUID (FK) | |
| pedra_id | INTEGER (FK) | |
| quantidade | INTEGER | Numero de pedras deste tipo |
| quilates_total | DECIMAL(8,3) | Peso total em quilates |
| lapidacao | VARCHAR(50) | Brilliant, Princess, Emerald, etc |
| cor_grau | VARCHAR(5) | Escala D-Z (diamantes) |
| pureza_grau | VARCHAR(10) | FL, IF, VVS1, VS1, SI1, etc |
| qualidade_corte | ENUM | EXCELLENT, VERY_GOOD, GOOD, FAIR, POOR |
| custo_unitario | DECIMAL(12,2) | Custo por quilate |
| custo_total | DECIMAL(12,2) | Custo total das pedras |
| observacoes | TEXT | |

#### ProdutoImagem

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | |
| produto_id | UUID (FK) | |
| url | VARCHAR(500) | URL da imagem (storage externo) |
| tipo | ENUM | PRINCIPAL, DETALHE, MODELO, CERTIFICADO |
| ordem | INTEGER | Ordem de exibicao |

#### Certificado

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | |
| produto_id | UUID (FK) | |
| entidade_certificadora | ENUM | GIA, IGI, HRD, IBGM, OUTRO |
| numero_certificado | VARCHAR(100) | Numero do certificado |
| data_emissao | DATE | |
| url_documento | VARCHAR(500) | Link para PDF/imagem do certificado |
| observacoes | TEXT | |

#### Estoque / MovimentacaoEstoque

O estoque e controlado por movimentacoes. O saldo atual e derivado da soma das movimentacoes.

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | |
| produto_id | UUID (FK) | |
| tipo_movimentacao | ENUM | ENTRADA, SAIDA, TRANSFERENCIA, AJUSTE, RESERVA, LIBERACAO_RESERVA |
| quantidade | INTEGER | Sempre positivo; tipo define direcao |
| localizacao_origem | ENUM | COFRE, VITRINE, REPRESENTANTE, TRANSITO, VENDIDO, CONSIGNADO |
| localizacao_destino | ENUM | (mesmos valores) |
| referencia_id | UUID | ID da venda, consignacao ou ajuste que originou |
| referencia_tipo | VARCHAR(50) | VENDA, CONSIGNACAO, INVENTARIO, MANUAL |
| usuario_id | UUID (FK) | Quem realizou |
| observacoes | TEXT | |
| criado_em | TIMESTAMP | |

**View materializada** `estoque_atual`:
```sql
CREATE MATERIALIZED VIEW estoque_atual AS
SELECT produto_id, localizacao_destino AS localizacao,
       SUM(CASE WHEN tipo_movimentacao IN ('ENTRADA','TRANSFERENCIA','LIBERACAO_RESERVA') THEN quantidade
                WHEN tipo_movimentacao IN ('SAIDA','RESERVA') THEN -quantidade
                ELSE 0 END) AS saldo
FROM movimentacao_estoque
GROUP BY produto_id, localizacao_destino;
```

#### CotacaoMetal

Historico de cotacoes de metais preciosos.

| Coluna | Tipo | Descricao |
|---|---|---|
| id | SERIAL (PK) | |
| tipo_metal | ENUM | OURO, PRATA, PLATINA |
| quilatagem | VARCHAR(10) | 24K, 18K, etc |
| preco_grama | DECIMAL(12,2) | Preco por grama em BRL |
| data_cotacao | DATE | Data da cotacao |
| fonte | VARCHAR(100) | Fonte da cotacao |
| criado_em | TIMESTAMP | |
| **UNIQUE** | (tipo_metal, quilatagem, data_cotacao) | |

#### Venda

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | |
| numero_venda | VARCHAR(20) UNIQUE | Numero sequencial legivel |
| cliente_id | UUID (FK) | |
| vendedor_id | UUID (FK → Usuario) | |
| canal_venda | ENUM | LOJA_FISICA, ECOMMERCE, REPRESENTANTE, CONSIGNACAO |
| status | ENUM | RASCUNHO, CONFIRMADA, PAGA, CANCELADA, DEVOLVIDA |
| subtotal | DECIMAL(12,2) | Soma dos itens |
| desconto_percentual | DECIMAL(5,2) | |
| desconto_valor | DECIMAL(12,2) | |
| valor_total | DECIMAL(12,2) | Valor final |
| observacoes | TEXT | |
| criado_em | TIMESTAMP | |
| atualizado_em | TIMESTAMP | |

#### ItemVenda

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | |
| venda_id | UUID (FK) | |
| produto_id | UUID (FK) | |
| quantidade | INTEGER | |
| preco_unitario | DECIMAL(12,2) | Preco no momento da venda |
| desconto_item | DECIMAL(12,2) | Desconto no item |
| preco_final | DECIMAL(12,2) | Preco unitario - desconto |
| observacoes | TEXT | Ex: gravacao personalizada |

#### PagamentoVenda

Suporta pagamento misto (multiplas formas na mesma venda).

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | |
| venda_id | UUID (FK) | |
| forma_pagamento | ENUM | DINHEIRO, PIX, CARTAO_CREDITO, CARTAO_DEBITO, BOLETO, TRANSFERENCIA |
| valor | DECIMAL(12,2) | Valor pago nesta forma |
| parcelas | INTEGER | Numero de parcelas (1 = a vista) |

#### Consignacao

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | |
| numero_consignacao | VARCHAR(20) UNIQUE | Numero sequencial |
| representante_id | UUID (FK → Usuario) | Representante/revendedor |
| status | ENUM | CRIADA, ENVIADA, EM_ACERTO, ACERTADA, CANCELADA |
| data_envio | DATE | Data de envio das pecas |
| data_limite_acerto | DATE | Prazo para acerto |
| data_acerto | DATE | Data efetiva do acerto |
| valor_total_pecas | DECIMAL(12,2) | Valor total das pecas enviadas |
| valor_vendido | DECIMAL(12,2) | Valor total vendido pelo representante |
| valor_devolvido | DECIMAL(12,2) | Valor das pecas devolvidas |
| comissao_representante | DECIMAL(12,2) | Comissao calculada |
| valor_acerto | DECIMAL(12,2) | Valor a repassar (vendido - comissao) |
| observacoes | TEXT | |
| criado_em | TIMESTAMP | |
| atualizado_em | TIMESTAMP | |

#### ItemConsignacao

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | |
| consignacao_id | UUID (FK) | |
| produto_id | UUID (FK) | |
| preco_venda_sugerido | DECIMAL(12,2) | |
| status | ENUM | EM_POSSE, VENDIDO, DEVOLVIDO |
| data_venda | DATE | Se vendido |
| preco_venda_real | DECIMAL(12,2) | Preco pelo qual foi vendido |
| cliente_final_nome | VARCHAR(200) | Nome do comprador final |
| cliente_final_telefone | VARCHAR(20) | Contato do comprador |
| observacoes | TEXT | |

#### RegraComissao

Define regras de comissionamento configuráveis.

| Coluna | Tipo | Descricao |
|---|---|---|
| id | SERIAL (PK) | |
| nome | VARCHAR(200) | Nome da regra |
| tipo | ENUM | FIXA, ESCALONADA, POR_META |
| canal_venda | ENUM | LOJA_FISICA, ECOMMERCE, REPRESENTANTE, CONSIGNACAO, TODOS |
| categoria_id | INTEGER (FK, nullable) | Se aplica a categoria especifica |
| percentual_fixo | DECIMAL(5,2) | Para tipo FIXA |
| ativo | BOOLEAN | |
| criado_em | TIMESTAMP | |

#### FaixaComissao

Faixas para comissao escalonada.

| Coluna | Tipo | Descricao |
|---|---|---|
| id | SERIAL (PK) | |
| regra_comissao_id | INTEGER (FK) | |
| valor_minimo | DECIMAL(12,2) | Limite inferior da faixa |
| valor_maximo | DECIMAL(12,2) | Limite superior (NULL = sem limite) |
| percentual | DECIMAL(5,2) | Percentual da faixa |

#### MetaVenda

| Coluna | Tipo | Descricao |
|---|---|---|
| id | SERIAL (PK) | |
| usuario_id | UUID (FK) | Vendedor/representante |
| periodo_inicio | DATE | |
| periodo_fim | DATE | |
| valor_meta | DECIMAL(12,2) | Meta em valor de vendas |
| bonus_meta | DECIMAL(12,2) | Bonus ao atingir |
| atingida | BOOLEAN | |
| valor_realizado | DECIMAL(12,2) | Valor acumulado |

#### Comissao

Registro de cada comissao calculada.

| Coluna | Tipo | Descricao |
|---|---|---|
| id | UUID (PK) | |
| usuario_id | UUID (FK) | Vendedor/representante |
| venda_id | UUID (FK, nullable) | Se venda direta |
| consignacao_id | UUID (FK, nullable) | Se consignacao |
| regra_comissao_id | INTEGER (FK) | Regra aplicada |
| valor_base | DECIMAL(12,2) | Valor de referencia |
| percentual_aplicado | DECIMAL(5,2) | % efetivamente aplicado |
| valor_comissao | DECIMAL(12,2) | Valor da comissao |
| status | ENUM | PENDENTE, APROVADA, PAGA, ESTORNADA |
| periodo_apuracao | VARCHAR(7) | Ex: 2026-03 (YYYY-MM) |
| criado_em | TIMESTAMP | |

#### Empresa

Dados da empresa (registro unico, configuracao do sistema).

| Coluna | Tipo | Descricao |
|---|---|---|
| id | SERIAL (PK) | |
| razao_social | VARCHAR(300) | Razao social |
| nome_fantasia | VARCHAR(300) | Nome fantasia |
| cnpj | VARCHAR(18) UNIQUE | CNPJ |
| inscricao_estadual | VARCHAR(20) | IE |
| telefone | VARCHAR(20) | Telefone |
| email | VARCHAR(255) | Email da empresa |
| logradouro | VARCHAR(300) | Endereco |
| numero | VARCHAR(20) | |
| complemento | VARCHAR(100) | |
| bairro | VARCHAR(100) | |
| cidade | VARCHAR(100) | |
| uf | CHAR(2) | |
| cep | VARCHAR(10) | |
| logo_url | VARCHAR(500) | URL do logotipo |
| atualizado_em | TIMESTAMP | |

---

## 2. Endpoints REST

Base URL: `/api/v1`

### 2.1 Autenticacao (`/api/v1/auth`)

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| POST | `/auth/login` | Login com email/senha | `{ email, senha }` | `{ accessToken, refreshToken, usuario }` |
| POST | `/auth/registro` | Registro inicial (cria usuario pendente de aprovacao) | `{ nome, email, senha, telefone, cpf }` | `UsuarioDTO` (201) |
| POST | `/auth/refresh` | Renovar access token | `{ refreshToken }` | `{ accessToken, refreshToken }` |
| POST | `/auth/logout` | Invalidar refresh token | `{ refreshToken }` | `204 No Content` |
| POST | `/auth/recuperar-senha` | Solicitar reset de senha (envia email) | `{ email }` | `204 No Content` |
| POST | `/auth/reset-senha` | Redefinir senha via token | `{ token, novaSenha }` | `204 No Content` |
| GET | `/auth/me` | Dados do usuario logado | — | `UsuarioDTO` |
| PUT | `/auth/alterar-senha` | Alterar senha | `{ senhaAtual, novaSenha }` | `204 No Content` |

### 2.2 Usuarios (`/api/v1/usuarios`)

Requer role ADMIN ou GERENTE.

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/usuarios` | Listar (paginado, filtros) | `?role=&ativo=&page=&size=` | `Page<UsuarioResumoDTO>` |
| GET | `/usuarios/{id}` | Detalhe | — | `UsuarioDTO` |
| POST | `/usuarios` | Criar usuario | `UsuarioCreateDTO` | `UsuarioDTO` (201) |
| PUT | `/usuarios/{id}` | Atualizar | `UsuarioUpdateDTO` | `UsuarioDTO` |
| PATCH | `/usuarios/{id}/ativar` | Ativar | — | `204` |
| PATCH | `/usuarios/{id}/desativar` | Desativar | — | `204` |

### 2.3 Clientes (`/api/v1/clientes`)

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/clientes` | Listar (paginado, busca) | `?q=&page=&size=` | `Page<ClienteResumoDTO>` |
| GET | `/clientes/{id}` | Detalhe com enderecos | — | `ClienteDTO` |
| POST | `/clientes` | Criar | `ClienteCreateDTO` | `ClienteDTO` (201) |
| PUT | `/clientes/{id}` | Atualizar | `ClienteUpdateDTO` | `ClienteDTO` |
| DELETE | `/clientes/{id}` | Soft delete | — | `204` |
| GET | `/clientes/{id}/historico-compras` | Historico de vendas | `?page=&size=` | `Page<VendaResumoDTO>` |
| POST | `/clientes/{id}/enderecos` | Adicionar endereco | `EnderecoDTO` | `EnderecoDTO` (201) |
| PUT | `/clientes/{id}/enderecos/{endId}` | Atualizar endereco | `EnderecoDTO` | `EnderecoDTO` |
| DELETE | `/clientes/{id}/enderecos/{endId}` | Remover endereco | — | `204` |
| GET | `/clientes/busca` | Busca rapida para autocomplete (PDV) | `?q=` (nome, cpf, telefone) | `List<ClienteBuscaDTO>` (max 20, <200ms) |

### 2.4 Categorias e Subcategorias (`/api/v1/categorias`)

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/categorias` | Listar todas (com subcategorias) | — | `List<CategoriaDTO>` |
| POST | `/categorias` | Criar categoria | `CategoriaCreateDTO` | `CategoriaDTO` (201) |
| PUT | `/categorias/{id}` | Atualizar | `CategoriaUpdateDTO` | `CategoriaDTO` |
| POST | `/categorias/{id}/subcategorias` | Criar subcategoria | `SubcategoriaCreateDTO` | `SubcategoriaDTO` (201) |
| PUT | `/categorias/{catId}/subcategorias/{subId}` | Atualizar subcategoria | `SubcategoriaUpdateDTO` | `SubcategoriaDTO` |

### 2.5 Materiais (`/api/v1/materiais`)

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/materiais` | Listar todos | `?tipoMetal=` | `List<MaterialDTO>` |
| POST | `/materiais` | Criar | `MaterialCreateDTO` | `MaterialDTO` (201) |
| PUT | `/materiais/{id}` | Atualizar | `MaterialUpdateDTO` | `MaterialDTO` |

### 2.6 Pedras (`/api/v1/pedras`)

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/pedras` | Listar todas | `?tipo=` | `List<PedraDTO>` |
| POST | `/pedras` | Criar | `PedraCreateDTO` | `PedraDTO` (201) |
| PUT | `/pedras/{id}` | Atualizar | `PedraUpdateDTO` | `PedraDTO` |

### 2.7 Produtos (`/api/v1/produtos`)

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/produtos` | Listar (paginado, filtros avancados) | `?q=&categoriaId=&materialId=&pedraId=&tipoProtudo=&genero=&precoMin=&precoMax=&acabamento=&destaque=&ativo=&page=&size=&sort=` | `Page<ProdutoResumoDTO>` |
| GET | `/produtos/{id}` | Detalhe completo (pedras, imagens, certificados) | — | `ProdutoDetalheDTO` |
| POST | `/produtos` | Criar produto | `ProdutoCreateDTO` | `ProdutoDetalheDTO` (201) |
| PUT | `/produtos/{id}` | Atualizar | `ProdutoUpdateDTO` | `ProdutoDetalheDTO` |
| DELETE | `/produtos/{id}` | Soft delete | — | `204` |
| POST | `/produtos/{id}/imagens` | Upload de imagem | `multipart/form-data` | `ProdutoImagemDTO` (201) |
| DELETE | `/produtos/{id}/imagens/{imgId}` | Remover imagem | — | `204` |
| POST | `/produtos/{id}/certificados` | Adicionar certificado | `CertificadoCreateDTO` | `CertificadoDTO` (201) |
| POST | `/produtos/{id}/recalcular-preco` | Recalcular preco com base na cotacao atual | — | `ProdutoDetalheDTO` |
| POST | `/produtos/importar` | Importacao em lote (CSV/Excel) | `multipart/form-data` | `ImportacaoResultadoDTO` |
| POST | `/produtos/recalcular-precos` | Recalcular precos em lote | `{ categoriaId?, materialId? }` | `{ totalAtualizado }` |
| GET | `/produtos/busca` | Busca rapida para autocomplete (PDV) | `?q=` (nome, codigo, serie) | `List<ProdutoBuscaDTO>` (max 20, <200ms) |

### 2.8 Estoque (`/api/v1/estoque`)

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/estoque` | Posicao atual do estoque | `?localizacao=&categoriaId=&page=&size=` | `Page<EstoquePosicaoDTO>` |
| GET | `/estoque/produto/{produtoId}` | Historico de movimentacoes de um produto | `?page=&size=` | `Page<MovimentacaoDTO>` |
| POST | `/estoque/entrada` | Registrar entrada de pecas | `EntradaEstoqueDTO` | `MovimentacaoDTO` (201) |
| POST | `/estoque/saida` | Registrar saida manual | `SaidaEstoqueDTO` | `MovimentacaoDTO` (201) |
| POST | `/estoque/transferencia` | Transferir entre localizacoes | `TransferenciaDTO` | `MovimentacaoDTO` (201) |
| POST | `/estoque/ajuste` | Ajuste de inventario | `AjusteEstoqueDTO` | `MovimentacaoDTO` (201) |
| GET | `/estoque/inventario` | Relatorio de inventario completo | `?localizacao=&formato=pdf|csv` | `InventarioDTO` ou arquivo |
| POST | `/estoque/inventario/conferir` | Registrar conferencia fisica (compara esperado vs contado) | `ConferenciaEstoqueDTO` | `ConferenciaResultadoDTO` (201) |
| GET | `/estoque/resumo` | Resumo do estoque por categoria/metal | — | `EstoqueResumoDTO` |
| GET | `/estoque/alertas` | Alertas de estoque (baixo, consignacoes vencidas) | — | `List<AlertaEstoqueDTO>` |

### 2.9 Cotacoes (`/api/v1/cotacoes`)

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/cotacoes/atual` | Cotacao mais recente de cada metal | — | `List<CotacaoDTO>` |
| GET | `/cotacoes/historico` | Historico de cotacoes | `?tipoMetal=&de=&ate=` | `List<CotacaoDTO>` |
| POST | `/cotacoes` | Registrar nova cotacao | `CotacaoCreateDTO` | `CotacaoDTO` (201) |

### 2.10 Vendas (`/api/v1/vendas`)

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/vendas` | Listar vendas (paginado, filtros) | `?status=&canalVenda=&vendedorId=&clienteId=&de=&ate=&page=&size=` | `Page<VendaResumoDTO>` |
| GET | `/vendas/{id}` | Detalhe da venda com itens | — | `VendaDetalheDTO` |
| POST | `/vendas` | Criar venda | `VendaCreateDTO` | `VendaDetalheDTO` (201) |
| PUT | `/vendas/{id}` | Atualizar rascunho | `VendaUpdateDTO` | `VendaDetalheDTO` |
| PATCH | `/vendas/{id}/confirmar` | Confirmar venda (gera movimentacao de estoque) | — | `VendaDetalheDTO` |
| PATCH | `/vendas/{id}/registrar-pagamento` | Marcar como paga (dispara calculo de comissao) | `{ formaPagamento, parcelas }` | `VendaDetalheDTO` |
| PATCH | `/vendas/{id}/cancelar` | Cancelar (estorna estoque e comissao) | `{ motivo }` | `VendaDetalheDTO` |
| PATCH | `/vendas/{id}/devolver` | Registrar devolucao | `DevolucaoDTO` | `VendaDetalheDTO` |
| GET | `/vendas/formas-pagamento` | Listar formas de pagamento disponiveis | — | `List<FormaPagamentoDTO>` |

### 2.11 Consignacao (`/api/v1/consignacoes`)

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/consignacoes` | Listar (paginado, filtros) | `?status=&representanteId=&de=&ate=&page=&size=` | `Page<ConsignacaoResumoDTO>` |
| GET | `/consignacoes/{id}` | Detalhe com itens | — | `ConsignacaoDetalheDTO` |
| POST | `/consignacoes` | Criar consignacao (montar kit) | `ConsignacaoCreateDTO` | `ConsignacaoDetalheDTO` (201) |
| PATCH | `/consignacoes/{id}/enviar` | Marcar como enviada (move estoque para CONSIGNADO) | — | `ConsignacaoDetalheDTO` |
| PATCH | `/consignacoes/{id}/itens/{itemId}/vender` | Registrar venda de item pelo representante | `{ precoVendaReal, clienteFinalNome?, clienteFinalTelefone? }` | `ItemConsignacaoDTO` |
| PATCH | `/consignacoes/{id}/itens/{itemId}/devolver` | Registrar devolucao de item | — | `ItemConsignacaoDTO` |
| PATCH | `/consignacoes/{id}/acertar` | Realizar acerto financeiro (calcula comissao) | — | `ConsignacaoDetalheDTO` |
| PATCH | `/consignacoes/{id}/cancelar` | Cancelar consignacao (devolver tudo) | `{ motivo }` | `ConsignacaoDetalheDTO` |
| PATCH | `/consignacoes/{id}/estender-prazo` | Estender prazo de acerto | `{ novaDataLimite }` | `ConsignacaoDetalheDTO` |
| POST | `/consignacoes/{id}/adicionar-pecas` | Adicionar pecas a consignacao ativa | `{ itens: [{ produtoId, precoVendaSugerido }] }` | `ConsignacaoDetalheDTO` |
| GET | `/consignacoes/{id}/resumo-financeiro` | Resumo financeiro do kit | — | `ConsignacaoResumoFinanceiroDTO` |
| GET | `/consignacoes/vencidas` | Listar consignacoes com prazo expirado | — | `List<ConsignacaoResumoDTO>` |

### 2.12 Comissoes (`/api/v1/comissoes`)

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/comissoes` | Listar comissoes (paginado) | `?usuarioId=&status=&periodo=&page=&size=` | `Page<ComissaoDTO>` |
| GET | `/comissoes/resumo` | Resumo de comissoes por periodo | `?periodo=&usuarioId=` | `ComissaoResumoDTO` |
| PATCH | `/comissoes/{id}/aprovar` | Aprovar comissao | — | `ComissaoDTO` |
| PATCH | `/comissoes/{id}/pagar` | Marcar como paga | — | `ComissaoDTO` |
| PATCH | `/comissoes/{id}/estornar` | Estornar comissao | `{ motivo }` | `ComissaoDTO` |

### 2.13 Regras de Comissao (`/api/v1/regras-comissao`)

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/regras-comissao` | Listar todas as regras | — | `List<RegraComissaoDTO>` |
| GET | `/regras-comissao/{id}` | Detalhe com faixas | — | `RegraComissaoDetalheDTO` |
| POST | `/regras-comissao` | Criar regra | `RegraComissaoCreateDTO` | `RegraComissaoDetalheDTO` (201) |
| PUT | `/regras-comissao/{id}` | Atualizar regra | `RegraComissaoUpdateDTO` | `RegraComissaoDetalheDTO` |
| DELETE | `/regras-comissao/{id}` | Desativar regra | — | `204` |

### 2.14 Metas de Venda (`/api/v1/metas`)

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/metas` | Listar metas | `?usuarioId=&periodo=` | `List<MetaVendaDTO>` |
| POST | `/metas` | Criar meta | `MetaVendaCreateDTO` | `MetaVendaDTO` (201) |
| PUT | `/metas/{id}` | Atualizar meta | `MetaVendaUpdateDTO` | `MetaVendaDTO` |
| GET | `/metas/{id}/progresso` | Progresso da meta | — | `MetaProgressoDTO` |

### 2.15 Relatorios (`/api/v1/relatorios`)

Todos os relatorios suportam `formato=json|pdf|csv` via query param.

| Metodo | Path | Descricao | Parametros |
|---|---|---|---|
| GET | `/relatorios/vendas` | Vendas por periodo | `?de=&ate=&canalVenda=&vendedorId=&agrupamento=dia|semana|mes` |
| GET | `/relatorios/vendas/por-categoria` | Vendas agrupadas por categoria | `?de=&ate=` |
| GET | `/relatorios/vendas/por-vendedor` | Ranking de vendedores | `?de=&ate=` |
| GET | `/relatorios/estoque/posicao` | Posicao atual do estoque | `?localizacao=&categoriaId=` |
| GET | `/relatorios/estoque/giro` | Giro de estoque | `?de=&ate=&categoriaId=` |
| GET | `/relatorios/estoque/valor` | Valor do estoque por localizacao | — |
| GET | `/relatorios/comissoes` | Comissoes por periodo | `?de=&ate=&usuarioId=` |
| GET | `/relatorios/comissoes/ranking` | Ranking de comissoes | `?de=&ate=` |
| GET | `/relatorios/consignacoes` | Status de consignacoes | `?de=&ate=&representanteId=` |
| GET | `/relatorios/consignacoes/vencimentos` | Consignacoes proximas do vencimento | `?diasAteVencimento=7` |
| GET | `/relatorios/financeiro/faturamento` | Faturamento geral | `?de=&ate=&agrupamento=dia|semana|mes` |
| GET | `/relatorios/produtos/mais-vendidos` | Produtos mais vendidos | `?de=&ate=&limit=20` |
| GET | `/relatorios/clientes/mais-ativos` | Clientes com maior volume | `?de=&ate=&limit=20` |

### 2.16 Dashboard (`/api/v1/dashboard`)

Endpoints dedicados para alimentar os dashboards por perfil.

| Metodo | Path | Descricao | Response |
|---|---|---|---|
| GET | `/dashboard/admin` | KPIs gerais: vendas hoje/mes, estoque total, meta equipe, alertas | `DashboardAdminDTO` |
| GET | `/dashboard/admin/grafico-vendas` | Dados para grafico de vendas | `?periodo=7d|30d|90d` → `List<GraficoVendaDTO>` |
| GET | `/dashboard/admin/vendas-recentes` | Ultimas vendas (limit 10) | `List<VendaResumoDTO>` |
| GET | `/dashboard/admin/ranking-vendedores` | Top vendedores do mes | `List<RankingVendedorDTO>` |
| GET | `/dashboard/vendedor` | KPIs pessoais do vendedor logado: metas, comissoes, vendas recentes | `DashboardVendedorDTO` |
| GET | `/dashboard/representante` | KPIs do representante logado: metas, comissoes, pecas em posse, proximos acertos | `DashboardRepresentanteDTO` |

**DTOs de dashboard:**

```json
// DashboardAdminDTO
{
  "vendasHoje": { "valor": 12500.00, "quantidade": 5, "variacaoPercentual": 12.0 },
  "vendasMes": { "valor": 287000.00, "quantidade": 125, "variacaoPercentual": 8.0 },
  "estoqueTotal": { "quantidade": 1247, "alertas": 3 },
  "metaEquipe": { "valor": 350000.00, "realizado": 287000.00, "percentual": 82.0 }
}

// DashboardVendedorDTO
{
  "metaPessoal": { "valor": 50000.00, "realizado": 38000.00, "percentual": 76.0 },
  "comissoesMes": { "valor": 1140.00, "status": "PENDENTE" },
  "vendasRecentes": [ ... ]
}

// DashboardRepresentanteDTO
{
  "metaPessoal": { "valor": 30000.00, "realizado": 18500.00, "percentual": 61.7 },
  "comissoesMes": { "valor": 6475.00, "status": "PENDENTE" },
  "pecasEmPosse": 10,
  "proximoAcerto": { "consignacaoId": "uuid", "dataLimite": "2026-03-31", "diasRestantes": 4 }
}
```

### 2.17 Representantes (`/api/v1/representantes`)

Endpoints dedicados para gestao de representantes (usuarios com role REPRESENTANTE).

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/representantes` | Listar representantes (paginado) | `?ativo=&q=&page=&size=` | `Page<RepresentanteResumoDTO>` |
| GET | `/representantes/{id}` | Perfil completo | — | `RepresentantePerfilDTO` |
| POST | `/representantes` | Criar representante | `RepresentanteCreateDTO` | `RepresentantePerfilDTO` (201) |
| PUT | `/representantes/{id}` | Atualizar | `RepresentanteUpdateDTO` | `RepresentantePerfilDTO` |
| GET | `/representantes/{id}/metas` | Metas do representante | — | `List<MetaVendaDTO>` |
| GET | `/representantes/{id}/comissoes` | Comissoes do representante | `?periodo=` | `List<ComissaoDTO>` |
| GET | `/representantes/{id}/pecas` | Pecas em posse do representante | — | `List<ItemConsignacaoDTO>` |
| GET | `/representantes/{id}/consignacoes` | Consignacoes do representante | `?status=&page=&size=` | `Page<ConsignacaoResumoDTO>` |
| GET | `/representantes/ativos` | Lista simplificada de representantes ativos (para selects) | — | `List<RepresentanteSelectDTO>` |

### 2.18 Representante Self-Service (`/api/v1/minha-conta`)

Endpoints para o representante autenticado gerenciar suas proprias informacoes.

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/minha-conta/pecas` | Pecas em posse do representante logado | — | `List<ItemConsignacaoDTO>` |
| POST | `/minha-conta/vendas` | Registrar venda de peca em posse | `{ itemConsignacaoId, precoVendaReal, clienteFinalNome?, clienteFinalTelefone? }` | `ItemConsignacaoDTO` |
| GET | `/minha-conta/vendas` | Historico de vendas registradas | `?de=&ate=&page=&size=` | `Page<ItemConsignacaoDTO>` |
| GET | `/minha-conta/acertos` | Listar acertos (consignacoes acertadas) | `?page=&size=` | `Page<ConsignacaoResumoDTO>` |
| GET | `/minha-conta/acertos/{consignacaoId}` | Detalhe de um acerto | — | `ConsignacaoDetalheDTO` |

### 2.19 Empresa (`/api/v1/empresa`)

Requer role ADMIN.

| Metodo | Path | Descricao | Request | Response |
|---|---|---|---|---|
| GET | `/empresa` | Dados da empresa | — | `EmpresaDTO` |
| PUT | `/empresa` | Atualizar dados da empresa | `EmpresaUpdateDTO` | `EmpresaDTO` |
| POST | `/empresa/logo` | Upload do logotipo | `multipart/form-data` | `{ logoUrl }` |

---

## 3. Regras de Negocio

### 3.1 Calculo de Preco

```
precoCusto = (cotacaoMetalPorGrama × pesoMetalGramas)
           + somaCustoPedras
           + custoMaoDeObra
           + custosIndiretos

precoVenda = precoCusto × markup
```

- Ao atualizar a cotacao do metal, o sistema permite recalcular precos em lote (por categoria ou material).
- O preco de venda pode ser sobrescrito manualmente (override).
- Para consignacao, o `precoVendaSugerido` e definido na criacao da consignacao.

### 3.2 Calculo de Comissao

O calculo segue a seguinte logica:

1. **Comissao Fixa**: `valorVenda × percentualFixo / 100`
2. **Comissao Escalonada**: O total vendido no periodo e aplicado nas faixas progressivas. Cada faixa aplica seu percentual ao valor dentro dela.
3. **Comissao por Meta**: Comissao fixa normal + bonus se meta do periodo for atingida.

**Regras gerais**:
- Comissao so e calculada apos confirmacao do pagamento da venda.
- Em caso de cancelamento/devolucao, a comissao e estornada.
- Periodo de apuracao: mensal (configuravel).
- A regra de comissao aplicada depende do canal de venda e da categoria do produto. Se nenhuma regra especifica existir, usa a regra com `canal=TODOS` e `categoria=null`.

**Prioridade de resolucao de regra**:
1. Regra especifica para canal + categoria
2. Regra especifica para canal (categoria null)
3. Regra generica (canal TODOS, categoria null)
4. Percentual padrao do usuario (`percentual_comissao_padrao`)

### 3.3 Fluxo de Consignacao

```
CRIADA → ENVIADA → EM_ACERTO → ACERTADA
  │         │
  │         └─→ CANCELADA
  └─→ CANCELADA
```

1. **Criar** (`CRIADA`): Selecionar representante e pecas. Cada item recebe `precoVendaSugerido`.
2. **Enviar** (`ENVIADA`): Confirma envio fisico. Estoque movimenta de `COFRE/VITRINE` para `CONSIGNADO`. Define `dataLimiteAcerto`.
3. **Registrar vendas/devolucoes**: Representante informa quais pecas vendeu (com preco real) e quais devolveu. Itens vendidos geram movimentacao `CONSIGNADO → VENDIDO`. Itens devolvidos geram `CONSIGNADO → COFRE`.
4. **Acertar** (`ACERTADA`): Calcula comissao do representante sobre valor vendido. Gera registro de comissao. Registra valor de acerto (valor vendido - comissao).
5. **Cancelar**: Devolve todas as pecas ao estoque. So permitido se nenhum item estiver vendido.

**Alertas**:
- Consignacoes com prazo expirado geram alerta no dashboard.
- Endpoint dedicado lista consignacoes vencidas.

### 3.4 Controle de Estoque

- **Rastreamento individual**: Cada peca com numero de serie e rastreada individualmente.
- **Localizacoes**: COFRE, VITRINE, REPRESENTANTE, TRANSITO, CONSIGNADO, VENDIDO.
- **Movimentacoes automaticas**:
  - Venda confirmada: `VITRINE/COFRE → VENDIDO`
  - Cancelamento de venda: `VENDIDO → VITRINE`
  - Envio consignacao: `COFRE/VITRINE → CONSIGNADO`
  - Devolucao consignacao: `CONSIGNADO → COFRE`
  - Venda via consignacao: `CONSIGNADO → VENDIDO`
- **Reserva**: Ao criar um rascunho de venda, o produto pode ser reservado (impede venda duplicada).
- **Inventario**: Ajustes manuais com justificativa obrigatoria.
- **Saldo**: Derivado da soma das movimentacoes (nao e campo armazenado; view materializada com refresh periodico).

### 3.5 Autenticacao e Autorizacao

**JWT com dois tokens**:
- Access Token: validade curta (15 minutos), contem `userId`, `role`, `nome`.
- Refresh Token: validade longa (7 dias), armazenado no banco, revogavel.

**Roles e permissoes**:

| Recurso | ADMIN | GERENTE | VENDEDOR | REPRESENTANTE |
|---|---|---|---|---|
| Usuarios (CRUD) | Total | Leitura + criar vendedor | — | — |
| Produtos (CRUD) | Total | Total | Leitura | Leitura |
| Estoque | Total | Total | Leitura | Apenas suas consignacoes |
| Vendas | Total | Total | Suas vendas | — |
| Consignacoes | Total | Total | Leitura | Suas consignacoes |
| Clientes | Total | Total | Total | Leitura dos seus |
| Comissoes | Total | Total | Suas comissoes | Suas comissoes |
| Regras comissao | Total | Leitura | — | — |
| Cotacoes | Total | Total | Leitura | — |
| Relatorios | Total | Total | Limitado | Limitado |

### 3.6 Validacoes Importantes

- **Produto**: `numero_serie` unico; `peso_total >= peso_metal`; `markup > 1.0`; pelo menos uma imagem para produtos ativos.
- **Venda**: Produto deve estar disponivel em estoque; cliente obrigatorio; vendedor obrigatorio; nao permitir vender produto com status CONSIGNADO.
- **Consignacao**: Representante deve ter role REPRESENTANTE; pecas devem estar disponiveis; prazo minimo de acerto: 7 dias.
- **Comissao**: Nao recalcular comissao ja PAGA; estorno so de comissao PENDENTE ou APROVADA.
- **Cotacao**: Nao permitir cotacao com data futura; nao permitir duplicata (metal + quilatagem + data).

---

## 4. Estrutura de Pacotes (Java)

```
com.joias.sistema
├── config/                  # Configuracoes Spring, Security, CORS, cache
│   ├── SecurityConfig.java
│   ├── JwtConfig.java
│   ├── CorsConfig.java
│   └── CacheConfig.java
├── security/                # JWT filter, provider, user details
│   ├── JwtAuthenticationFilter.java
│   ├── JwtTokenProvider.java
│   └── CustomUserDetailsService.java
├── exception/               # Excecoes e handler global
│   ├── GlobalExceptionHandler.java
│   ├── ResourceNotFoundException.java
│   ├── BusinessException.java
│   └── EstoqueInsuficienteException.java
├── entity/                  # Entidades JPA
│   ├── BaseEntity.java      # @MappedSuperclass com auditoria
│   ├── Usuario.java
│   ├── Cliente.java
│   ├── Produto.java
│   ├── ProdutoPedra.java
│   └── ...
├── enums/                   # Enums compartilhados
│   ├── Role.java
│   ├── CanalVenda.java
│   ├── StatusVenda.java
│   ├── TipoMetal.java
│   └── ...
├── repository/              # Spring Data JPA repositories
│   ├── UsuarioRepository.java
│   ├── ProdutoRepository.java
│   └── ...
├── dto/                     # DTOs de request e response
│   ├── request/
│   │   ├── ProdutoCreateDTO.java
│   │   ├── VendaCreateDTO.java
│   │   └── ...
│   └── response/
│       ├── ProdutoResumoDTO.java
│       ├── ProdutoDetalheDTO.java
│       └── ...
├── mapper/                  # MapStruct mappers (Entity ↔ DTO)
│   ├── ProdutoMapper.java
│   └── ...
├── service/                 # Logica de negocio
│   ├── AuthService.java
│   ├── ProdutoService.java
│   ├── EstoqueService.java
│   ├── VendaService.java
│   ├── ConsignacaoService.java
│   ├── ComissaoService.java
│   ├── CotacaoService.java
│   ├── PrecoService.java
│   └── RelatorioService.java
├── controller/              # REST controllers
│   ├── AuthController.java
│   ├── ProdutoController.java
│   ├── EstoqueController.java
│   ├── VendaController.java
│   ├── ConsignacaoController.java
│   ├── ComissaoController.java
│   ├── CotacaoController.java
│   └── RelatorioController.java
└── util/                    # Utilidades
    └── PrecoCalculator.java
```

---

## 5. Principais DTOs

### ProdutoCreateDTO (Request)
```json
{
  "codigoInterno": "AN-OUR-18K-001",
  "numeroSerie": "SN-2026-00001",
  "nome": "Anel Solitario Ouro 18K com Diamante 0.5ct",
  "descricao": "...",
  "categoriaId": 1,
  "subcategoriaId": 3,
  "materialId": 5,
  "pesoTotalGramas": 4.5,
  "pesoMetalGramas": 4.2,
  "tamanho": "16",
  "acabamento": "POLIDO",
  "tipoCravacao": "GARRA",
  "tipoFecho": "NENHUM",
  "tipoProduto": "JOIA",
  "custoMaoDeObra": 350.00,
  "custosIndiretos": 80.00,
  "markup": 2.50,
  "genero": "FEMININO",
  "personalizavel": true,
  "pedras": [
    {
      "pedraId": 1,
      "quantidade": 1,
      "quilatesTotal": 0.50,
      "lapidacao": "Brilliant",
      "corGrau": "G",
      "purezaGrau": "VS1",
      "qualidadeCorte": "EXCELLENT",
      "custoUnitario": 15000.00
    }
  ]
}
```

### VendaCreateDTO (Request)
```json
{
  "clienteId": "uuid",
  "canalVenda": "LOJA_FISICA",
  "descontoPercentual": 5.0,
  "observacoes": "Presente de aniversario",
  "itens": [
    {
      "produtoId": "uuid",
      "quantidade": 1,
      "descontoItem": 0
    }
  ],
  "pagamentos": [
    {
      "formaPagamento": "CARTAO_CREDITO",
      "valor": 3000.00,
      "parcelas": 6
    },
    {
      "formaPagamento": "PIX",
      "valor": 2415.00,
      "parcelas": 1
    }
  ]
}
```
> **Nota**: O campo `pagamentos` e um array para suportar pagamento misto (ex: parte cartao + parte PIX). Para pagamento unico, enviar array com um elemento.

### ConsignacaoCreateDTO (Request)
```json
{
  "representanteId": "uuid",
  "dataLimiteAcerto": "2026-04-27",
  "observacoes": "Kit abril - colecao outono",
  "itens": [
    {
      "produtoId": "uuid",
      "precoVendaSugerido": 5800.00
    }
  ]
}
```

### ProdutoDetalheDTO (Response)
```json
{
  "id": "uuid",
  "codigoInterno": "AN-OUR-18K-001",
  "numeroSerie": "SN-2026-00001",
  "nome": "Anel Solitario Ouro 18K com Diamante 0.5ct",
  "descricao": "...",
  "categoria": { "id": 1, "nome": "Aneis" },
  "subcategoria": { "id": 3, "nome": "Solitario" },
  "material": { "id": 5, "nome": "Ouro 18K Amarelo", "tipoMetal": "OURO", "quilatagem": "18K", "cor": "Amarelo" },
  "pesoTotalGramas": 4.5,
  "pesoMetalGramas": 4.2,
  "tamanho": "16",
  "acabamento": "POLIDO",
  "tipoCravacao": "GARRA",
  "tipoProduto": "JOIA",
  "precoCustoCalculado": 9530.00,
  "precoVenda": 23825.00,
  "genero": "FEMININO",
  "personalizavel": true,
  "destaque": false,
  "pedras": [
    {
      "pedra": { "id": 1, "nome": "Diamante", "tipo": "PRECIOSA" },
      "quantidade": 1,
      "quilatesTotal": 0.50,
      "lapidacao": "Brilliant",
      "corGrau": "G",
      "purezaGrau": "VS1",
      "qualidadeCorte": "EXCELLENT",
      "custoTotal": 7500.00
    }
  ],
  "imagens": [
    { "id": "uuid", "url": "https://storage.../img1.jpg", "tipo": "PRINCIPAL", "ordem": 1 }
  ],
  "certificados": [
    { "id": "uuid", "entidadeCertificadora": "GIA", "numeroCertificado": "GIA-12345", "dataEmissao": "2026-01-15" }
  ],
  "estoqueDisponivel": 1,
  "localizacao": "VITRINE"
}
```

---

## 6. Tratamento de Erros

Formato padrao de erro:

```json
{
  "timestamp": "2026-03-27T14:30:00Z",
  "status": 400,
  "error": "Bad Request",
  "message": "Produto com numero de serie SN-001 ja existe",
  "path": "/api/v1/produtos",
  "fieldErrors": [
    { "field": "numeroSerie", "message": "Numero de serie ja cadastrado" }
  ]
}
```

| HTTP Status | Uso |
|---|---|
| 200 | Sucesso (GET, PUT, PATCH) |
| 201 | Criado (POST) |
| 204 | Sem conteudo (DELETE, acoes sem retorno) |
| 400 | Validacao / regra de negocio |
| 401 | Nao autenticado |
| 403 | Sem permissao |
| 404 | Recurso nao encontrado |
| 409 | Conflito (ex: numero de serie duplicado) |
| 422 | Erro de processamento (ex: estoque insuficiente) |
| 500 | Erro interno |

---

## 7. Migrations (Flyway)

Estrutura de migrations:

```
src/main/resources/db/migration/
├── V1__criar_tabelas_referencia.sql       # material, pedra, categoria, subcategoria
├── V2__criar_tabela_usuario.sql           # usuario
├── V3__criar_tabela_cliente.sql           # cliente, endereco_cliente
├── V4__criar_tabela_produto.sql           # produto, produto_pedra, produto_imagem, certificado
├── V5__criar_tabelas_estoque.sql          # movimentacao_estoque, view estoque_atual
├── V6__criar_tabela_cotacao.sql           # cotacao_metal
├── V7__criar_tabelas_venda.sql            # venda, item_venda
├── V8__criar_tabelas_consignacao.sql      # consignacao, item_consignacao
├── V9__criar_tabelas_comissao.sql         # regra_comissao, faixa_comissao, meta_venda, comissao
├── V10__criar_tabela_empresa.sql          # empresa (registro unico)
├── V11__dados_iniciais.sql                # categorias, materiais, pedras padrao, empresa default
```

---

## 8. Configuracoes de Seguranca (CORS / Rate Limiting)

- CORS configurado para permitir origens do frontend (configuravel via `application.yml`).
- Rate limiting nos endpoints de autenticacao (5 tentativas/minuto por IP).
- Endpoints publicos: `POST /auth/login`, `POST /auth/refresh`, `GET /produtos` (catalogo publico), `GET /categorias`.
- Todos os demais endpoints requerem autenticacao JWT.

---

## 9. Eventos Assincronos (RabbitMQ)

| Evento | Produtor | Consumidor | Acao |
|---|---|---|---|
| `venda.paga` | VendaService | ComissaoService | Calcular comissao |
| `venda.cancelada` | VendaService | ComissaoService, EstoqueService | Estornar comissao e estoque |
| `consignacao.acertada` | ConsignacaoService | ComissaoService | Calcular comissao do representante |
| `cotacao.atualizada` | CotacaoService | (Notificacao) | Alertar sobre nova cotacao |
| `consignacao.vencida` | Scheduler | (Notificacao) | Alertar sobre consignacoes vencidas |
