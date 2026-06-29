# Portfolio Website

A full-stack personal portfolio website with glassmorphism design, dark/light theme, AI assistant, admin panel, and email contact.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080)
- `pnpm --filter @workspace/portfolio run dev` — run the portfolio frontend (port 21113)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string
- Required env: `OPENAI_API_KEY` — OpenAI API key for AI assistant
- Optional env: `SESSION_SECRET` — Express session secret (default fallback provided)
- Optional env: `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASS` — for email notifications
- Optional env: `CONTACT_EMAIL` — email address to receive contact form submissions

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite, Tailwind CSS, Framer Motion, next-themes, wouter
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- AI: OpenAI GPT-4o-mini (multilingual portfolio assistant)
- Email: Nodemailer (optional SMTP)
- Auth: express-session + bcryptjs (admin panel)
- Build: esbuild (CJS bundle)

## Where things live

- `lib/api-spec/openapi.yaml` — source of truth for all API contracts
- `lib/db/src/schema/portfolio.ts` — all DB table definitions
- `artifacts/api-server/src/routes/` — backend route handlers (portfolio, contact, assistant, admin)
- `artifacts/api-server/src/lib/` — adminAuth, emailService, logger
- `artifacts/portfolio/src/pages/` — frontend pages (Home, admin/*)
- `artifacts/portfolio/src/components/` — shared components (Navbar, Footer, sections, ChatAssistant)
- `artifacts/portfolio/src/index.css` — theme tokens and global styles

## Architecture decisions

- Portfolio content stored as JSONB columns in a single `portfolio_content` row for easy bulk updates via admin panel
- Admin auth uses express-session (cookie-based) — same-origin requests automatically include cookies
- AI assistant streams via SSE (not WebSockets) using raw fetch + ReadableStream on client
- AI assistant auto-detects the user's language and responds in the same language
- All portfolio data dynamically fetched from API — zero hardcoded content in frontend

## Product

- **Portfolio (`/`)** — Hero, About, Projects (filterable), Skills (categorized bars), Experience & Education (timeline), Contact form, AI assistant floating chat
- **Admin (`/admin`)** — Full CRUD for all sections, contact messages inbox, settings (admin credentials, contact email)
- **Theme** — Dark (default) and light mode, persisted in localStorage
- **AI Assistant** — Multilingual, responds in user's language, floating bottom-right chat bubble

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- Default admin credentials: `admin` / `admin123` — change immediately via `/admin/settings` after first login
- Email notifications require SMTP env vars; contact messages are always saved to DB regardless
- Run codegen after any OpenAPI spec change: `pnpm --filter @workspace/api-spec run codegen`

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
