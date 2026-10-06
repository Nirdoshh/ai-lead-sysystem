# AGENTS.md

## Quick architecture

- **Stack:** React + TypeScript + Vite, with Cloudflare Workers via `@cloudflare/vite-plugin`.
- **Frontend entry:** `index.html` → `src/main.tsx` → `src/App.tsx`.
- **Worker entry:** `worker/index.ts` exports `fetch` and handles `/api/*`.
- **API calls:** Frontend uses relative `fetch('/api/...')`; during development the Cloudflare Vite plugin routes API requests to the local Worker.
- **SPA handling:** Cloudflare serves the application as a single-page app for deployed builds.

## Commands

- `npm run dev` — run frontend and Worker locally.
- `npm run build` — type-check and create the production build.
- `npm run lint` — run Oxlint.
- `npm run preview` — build and preview the production application.
- `npm run deploy` — build and deploy with Wrangler.
- `npm run cf-typegen` — regenerate Cloudflare Worker types.

`package.json` is the source of truth for available project commands.

## Key configuration

- `vite.config.ts` — connects React, Vite, and Cloudflare Workers.
- `wrangler.jsonc` — Cloudflare Worker, asset, compatibility, and runtime configuration.
- `tsconfig.*` — TypeScript project configuration.
- `worker-configuration.d.ts` — generated Cloudflare Worker types.

## Working guidelines

- Keep frontend/browser responsibilities in `src/`.
- Keep server-side logic and private integrations in `worker/`.
- Do not change the Worker/API contract without updating its callers.
- Treat TypeScript errors from `tsc -b` as blocking.
- Follow existing project patterns before introducing new abstractions.
- Keep changes focused and avoid unrelated modifications.

## Safety and verification

- Never place secrets, API keys, or private credentials in `src/` or other browser-bundled code.
- `worker-configuration.d.ts` is generated; do not edit it manually.
- After changing Worker bindings or vars, run `npm run cf-typegen`.
- After meaningful code changes, run `npm run lint` and `npm run build`.
- When behavior changes, verify it with the application running through `npm run dev`.
- Report any verification step that was skipped or failed.

## High-value first reads

1. `package.json`
2. `vite.config.ts`
3. `wrangler.jsonc`
4. `worker/index.ts`
5. `src/App.tsx`
