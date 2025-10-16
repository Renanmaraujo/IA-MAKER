# Dev IA Maker — Starter Kit

Este projeto base implementa o esqueleto solicitado no teste técnico (React + TypeScript + Vite no frontend, fluxos n8n via webhooks consumindo Supabase e autenticação JWT).

> Ajuste as variáveis no `.env.example`, importe os fluxos do n8n e rode o frontend.

## Estrutura
```
frontend/
  src/
    api/
    components/
    pages/
    types/
    utils/
    main.tsx
    App.tsx
  index.html
  package.json
  tsconfig.json
  vite.config.ts

n8n/
  flows.json    # Export pronto para import

db/
  schema.sql    # Tabela messages e trigger updated_at

postman/
  collection.json
.env.example
```

## Setup Rápido

### 1) Supabase (ou Postgres puro)
- Crie um projeto no Supabase e rode o SQL em `db/schema.sql`.
- Gere uma service_role key (apenas para DEV).

### 2) n8n
- Suba o n8n (Docker ou local).
- Importe `n8n/flows.json`.
- Defina as credenciais/variáveis nas credenciais e env do n8n:
  - `SUPABASE_URL`
  - `SUPABASE_REST_URL` (ex.: https://<project>.supabase.co/rest/v1)
  - `SUPABASE_SERVICE_ROLE` (apenas DEV)
  - `JWT_SECRET` (ex.: "supersecret")
- Publique/obtenha as URLs públicas dos Webhooks (ou use o "Test URL").

### 3) Frontend
- Entre em `frontend/`
- Copie `.env.example` para `.env` e ajuste as URLs dos webhooks do n8n.
- Instale deps: `npm i`
- Rode: `npm run dev`

## Fluxos implementados
- `POST /api/auth/login` → Retorna JWT (usuário/mínimo fictício: `candidate` / `secret`).
- Mensagens:
  - `GET /api/messages`
  - `POST /api/messages`
  - `GET /api/messages/:id`
  - `PUT /api/messages/:id`
  - `PATCH /api/messages/:id`
  - `DELETE /api/messages/:id`

Erros retornam em `application/problem+json` e o frontend já trata esse formato.

## Testes
- Exemplos com Jest + React Testing Library (mínimos) incluídos.
- Scripts: `npm run test`

## Observações
- Em produção, **nunca** use `service_role` no frontend. Aqui ele é usado apenas no *backend do n8n* para simplificar o teste.
- O token JWT é salvo no `localStorage`.
