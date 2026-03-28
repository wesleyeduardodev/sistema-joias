# Sistema de Gestao e Venda de Joias

## Stack

- **Backend:** Spring Boot 4.0.3 + Java 25 + Maven
- **Frontend:** React 19.2.4 + TypeScript + Vite 8.0.3 + Shadcn/ui + Tailwind
- **Banco:** PostgreSQL 18.3
- **Roteamento:** React Router v7
- **Estado:** TanStack Query (servidor) + Zustand (cliente)
- **Formularios:** React Hook Form + Zod

## Ambiente de Desenvolvimento

- Banco de dados roda em container Docker via `docker-compose.yml`
- Para subir: `docker compose up -d`
- Conexao: `localhost:5432`, database `sistema_joias`, user `joias_user`, password `joias_dev_2026`
- Migrations gerenciadas pelo Flyway (executam automaticamente ao iniciar o backend)

## Documentacao

- `PRD.md` — Requisitos do produto, user stories, escopo MVP, plano de fases
- `pesquisa-setor.md` — Pesquisa sobre o setor de joias
- `arquitetura-backend.md` — Modelo de dados (22 entidades), 100+ endpoints REST, regras de negocio
- `arquitetura-frontend.md` — 42+ telas, wireframes, 30+ componentes, design system, rotas
