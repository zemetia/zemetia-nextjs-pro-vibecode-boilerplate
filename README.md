# Zemetia Next.js Frontend Edition

> Production-grade Next.js template engineered for AI-assisted development — where senior engineering decisions are already made, and Claude knows every rule.

---

## What is this?

**Zemetia Next.js Frontend Edition** is an opinionated, production-ready frontend template built on the latest cutting-edge stack. It is not just a starter kit — it is a fully architected system where every convention, constraint, and pattern has been decided upfront so your team can ship features from day one without debating folder structures, state management strategies, or API client patterns.

### Full Stack Architecture

| Layer | Technology | Version |
|---|---|---|
| Framework | Next.js (App Router) | 16 |
| Language | TypeScript (strict) | 6 |
| Styling | Tailwind CSS v4 + design tokens | v4 |
| Internationalization | next-intl | v4 |
| Server state | TanStack Query | v5 |
| Global state | Zustand | v5 |
| Validation | Zod | v4 |
| Toast notifications | Sonner | v2 |
| Error monitoring | Sentry | v10 |
| Product analytics | PostHog | latest |
| UI primitives | shadcn/ui + CVA | — |
| Icons | Lucide React | v1 |
| Testing | Vitest + React Testing Library | v4 |
| Component explorer | Storybook | v10 |
| Linting | ESLint flat config (zero warnings) | v9 |
| Formatting | Prettier + Tailwind plugin | v3 |
| Request proxy | proxy.ts + next-limitr | Next.js 16 |

### What "production-ready" actually means here

- **Proxy layer** (`proxy.ts`) — replaces the deprecated `middleware.ts` from Next.js 16. Handles sliding-window rate limiting (60 req/min per IP on `/api/*`), security headers (`X-Frame-Options`, `HSTS`, CSP family, `Permissions-Policy`), and locale routing — all composed in one place.
- **i18n from day one** — English and Indonesian locales out of the box, with `localePrefix: 'as-needed'` so `/about` stays clean in the default locale.
- **Cookie-persisted state** — Zustand stores persist to cookies (SSR-readable via `next/headers`), not `localStorage`, so server components can read theme without a flash.
- **ApiClient pipeline** — typed fetch wrapper with request/response interceptors, `ApiError` classification, and Next.js cache integration. Components never call `fetch` directly.
- **Design token system** — all colors defined as `@theme {}` CSS custom properties. Raw hex values and Tailwind color utilities are banned — your design system cannot drift.
- **Four-file component rule** — every UI component ships with implementation, Storybook story, Vitest test, and barrel export. Consistency is enforced by convention, not discipline.

---

## AI Vibe Coding — Professional Friendly

This template is designed to make **Claude (and any AI coding assistant) a genuine senior engineer on your team**, not just an autocomplete tool.

### The problem it solves

A senior developer joining a project needs weeks to learn the conventions: which router to use, where state lives, how API calls are structured, what's banned and why. An AI assistant cold-dropped into a codebase faces the same problem — and without guardrails, it will invent patterns, mix conventions, and introduce technical debt at speed.

**The solution is the Blueprint.**

### The Blueprint

The `docs/blueprint/` directory is a structured, cross-linked reference system written specifically for AI agents. Every file covers exactly one concern and links to related files. When Claude (or any agent) touches any part of this codebase, it reads the relevant blueprint section first — so it always applies the correct pattern.

```
docs/blueprint/
├── INDEX.md           — master index, hard constraints, file creation checklists
├── STRUCTURE.md       — every file and folder, what goes where
├── ARCHITECTURE.md    — request lifecycle, provider tree, proxy, integrations
├── COMPONENTS.md      — CVA pattern, four-file rule, shadcn, Storybook, tests
├── DESIGN_SYSTEM.md   — @theme tokens, oklch palette, typography, radius
├── SERVICES.md        — ApiClient, interceptors, domain services, Zod schemas
├── STATE.md           — Zustand stores, cookie persistence, storage decision tree
├── I18N.md            — next-intl routing, translations, navigation, static gen
└── BEST_PRACTICE.md   — TypeScript constraints, ESLint rules, naming, anti-patterns
```

Each blueprint file is structured for fast AI consumption: tables over prose, explicit constraints, canonical import paths, and worked examples. The AI does not need to guess — every decision is documented.

### CLAUDE.md — The AI Engineering Contract

`CLAUDE.md` (at the project root) is the entry point that Claude Code reads at the start of every session. It is not documentation for humans — it is an **engineering contract for AI agents**.

It does three things:

1. **Points to the Blueprint** — before planning any fix or feature, the AI must read the relevant blueprint sections. This prevents pattern invention from scratch.

2. **States the non-negotiables** — a short, scannable list of hard rules that can never be violated, regardless of context:
   - Navigation always via `@/i18n/navigation`, never `next/navigation`
   - Server data via TanStack Query only — no `useState` for API responses
   - Colors via design tokens only — no raw hex, no Tailwind color utilities
   - `npm run lint` must exit 0 with zero warnings
   - `'use client'` only when a hook, event handler, or browser API actually requires it
   - Services are plain objects — components never call `fetch` or `apiClient` directly
   - `middleware.ts` is deprecated — all request logic goes in `proxy.ts`

3. **Gives the stack snapshot** — versions in one place so the AI picks the right API surface without hallucinating deprecated patterns.

The result: Claude behaves like a developer who has already read every ADR, knows every convention, and asks clarifying questions when something is genuinely ambiguous — instead of making it up.

---

## Getting Started

### Prerequisites

- Node.js 20+
- npm 10+

### Installation

```bash
# 1. Clone the repository
git clone <repository-url>
cd template-nextjs

# 2. Install dependencies
npm install

# 3. Set up environment variables
cp .env.example .env.local
```

Edit `.env.local` with your values:

```env
# Required
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_APP_NAME=MyApp
NEXT_PUBLIC_API_URL=http://localhost:3001/api

# Optional — both are no-ops without these
NEXT_PUBLIC_SENTRY_DSN=
NEXT_PUBLIC_POSTHOG_KEY=
```

### Development

```bash
npm run dev          # Start dev server with Turbopack (http://localhost:3000)
npm run storybook    # Component explorer (http://localhost:6006)
```

### Quality checks

```bash
npm run lint         # ESLint — must exit 0 (zero warnings policy)
npm run lint:fix     # Auto-fix lint issues
npm run type-check   # TypeScript — no emit
npm run format       # Prettier format
npm run format:check # Prettier check (CI)
```

### Testing

```bash
npm run test         # Vitest in watch mode
npm run test:run     # Single run (CI)
npm run test:ui      # Vitest UI
npm run test:coverage # Coverage report
```

### Build

```bash
npm run build        # Production build
npm run start        # Start production server
npm run build-storybook # Build static Storybook
```

---

## Project Structure (quick reference)

```
src/
├── app/
│   ├── [locale]/         # All pages — Server Components by default
│   │   ├── layout.tsx    # Provider tree: NextIntl → PostHog → Toaster
│   │   └── page.tsx      # Home page
│   ├── api/health/       # Health check endpoint (Edge runtime)
│   └── globals.css       # Tailwind v4 @theme tokens
├── components/
│   ├── ui/               # Primitives (Button, Input, Badge, Card, Typography…)
│   ├── layout/           # Header, Footer, PageWrapper
│   └── shared/           # Cross-feature (LanguageSwitcher…)
├── hooks/                # Client-side hooks (useToast, useBreakpoint…)
├── i18n/                 # next-intl config, routing, navigation
├── lib/                  # cn, utils, sentry, cookies, validations
├── providers/            # PostHogProvider
├── proxy/                # rate-limit.ts, security-headers.ts
├── services/             # ApiClient + domain services
├── stores/               # Zustand stores
└── types/                # Shared TypeScript types

messages/
├── en/                   # English translations
└── id/                   # Indonesian translations

proxy.ts                  # Request intercept entry (Next.js 16)
docs/blueprint/           # AI agent reference system
```

---

## For AI Agents (Claude Code)

If you are an AI agent working on this codebase:

1. Read `CLAUDE.md` — it is your contract.
2. Before touching any file, identify which blueprint section covers it.
3. Read that section. Then plan. Never guess at patterns.
4. The hard constraints in `INDEX.md` are non-negotiable — they exist because of real incidents and deliberate architectural decisions, not style preferences.

---

## Attribution & Collaboration

Built and maintained by **[Zemetia](https://zemetia.id)**.

This template represents accumulated decisions from production deployments — every constraint exists for a reason. We welcome collaboration from teams who want to:

- Extend the blueprint for new integration patterns (auth, CMS, payments)
- Port the AI-first documentation approach to other frameworks
- Contribute new UI components following the four-file rule
- Share real-world patterns discovered while building on this foundation

To collaborate, reach out or open a discussion. We are particularly interested in teams using this template in production who have found gaps or improvements in the Blueprint — those learnings benefit everyone.

---

*Zemetia Next.js Frontend Edition — engineering decisions made once, enforced everywhere.*
