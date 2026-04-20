# Project Starter Comparison

This document compares five of the six project starters in this repo against the target stack:

- React 19 with the React Compiler
- React Router 7 (used for SSG)
- TypeScript
- shadcn/ui with Tailwind
- Playwright for testing
- Under consideration: Vite, Vitest, Bun

The `orange/` starter is intentionally excluded.

## Remix vs Router: are they the same?

**Yes — they are the same template.** A file-level diff of `remix/` and `router/` shows the *only* difference is the `name` field in `package.json` (`"remix"` vs `"router"`). All source, configs, and dependencies match exactly.

```bash
$ diff -rq remix/ router/
Files remix/package.json and router/package.json differ

$ diff remix/package.json router/package.json
2c2
<   "name": "remix",
---
>   "name": "router",
```

The commands used:

- `npx create-react-router@latest router --no-install --no-git-init --yes`
- `npx create-react-router@latest remix --template remix-run/react-router-templates/default --no-install --no-git-init --yes`

Both resolve to the same template: when no `--template` flag is passed, `create-react-router` defaults to `remix-run/react-router-templates/default`. So from here on, **remix and router are treated as one entry: `router` (default React Router template)**.

---

## Side-by-side feature matrix

| Feature | bun | vite | router (=remix) | shadcn | monorepo |
|---|---|---|---|---|---|
| React 19 | 19.x | 19.2.5 | 19.2.4 | 19.2.4 | 19.2.4 |
| React Compiler | no | no (README mentions how to add) | no | no | no |
| React Router 7 | **no** (custom Bun `serve()` router) | **no** | 7.14.0 | 7.13.1 | 7.12.0 (in `apps/web`) |
| SSR / SSG capable | no | SPA only | yes (`ssr: true`) | yes (`ssr: true`) | yes (`ssr: true`) |
| TypeScript | yes | yes | yes | yes | yes |
| Bundler / dev server | Bun bundler + `Bun.serve` (no Vite) | Vite 8 + `@vitejs/plugin-react` | Vite 8 + `@react-router/dev/vite` | Vite 7 + `@react-router/dev/vite` + `vite-tsconfig-paths` | Vite 7 + `@react-router/dev/vite` + `vite-tsconfig-paths` |
| Tailwind | v4 via `bun-plugin-tailwind` | **not installed** | v4 via `@tailwindcss/vite` | v4 via `@tailwindcss/vite` + `tw-animate-css` | v4 via `@tailwindcss/vite` + `tw-animate-css` |
| shadcn/ui | yes — `components.json` `style: new-york`, 6 pre-generated components (button, card, input, label, select, textarea) using raw `@radix-ui/react-*` packages | **no** | **no** | yes — `components.json` `style: radix-nova`, single seeded `button`, uses umbrella `radix-ui` pkg + `lucide-react` + `class-variance-authority` + `clsx` + `tailwind-merge` + `shadcn` runtime | yes — same shadcn config as `shadcn/` but placed in `packages/ui` and consumed via `@workspace/ui/*` exports |
| Path aliases | `@/*` → `./src/*` | none (default) | `~/*` → `./app/*` | `~/*` → `./app/*` | `@/*` → `./app/*` in web; `@workspace/ui/*` for shared UI |
| Package manager | bun (lockfile present) | none specified | none specified | bun (lockfile present) | bun (declared via `"packageManager": "bun@1.3.11"`) |
| Monorepo tooling | no | no | no | no | yes — Turbo 2.8 with workspaces `apps/*`, `packages/*` |
| Prettier | no | no | no | yes (`.prettierrc` + `prettier-plugin-tailwindcss`) | yes (root `.prettierrc` + plugin) |
| ESLint | no | yes (flat config, typescript-eslint + react-hooks + react-refresh) | no | no | no |
| Dockerfile | no | no | yes (node:20-alpine multistage) | yes | yes (in `apps/web`) |
| Playwright / Vitest | no | no | no | no | no |
| React Compiler | not configured | not configured (README only) | not configured | not configured | not configured |
| TS strict mode | strict + `noUncheckedIndexedAccess`, `noImplicitOverride`, `noFallthroughCasesInSwitch` | strict-ish (`noUnusedLocals`, `erasableSyntaxOnly`, `noFallthroughCasesInSwitch`) | strict | strict | strict |
| App entrypoint | `src/index.ts` (Bun.serve routes) + HTML imports + `src/frontend.tsx` | `src/main.tsx` (client-only `createRoot`) | `app/root.tsx` + `app/routes.ts` | `app/root.tsx` + `app/routes.ts` | `apps/web/app/root.tsx` + `apps/web/app/routes.ts` |
| Scripts | `dev` (bun --hot), `start`, `build` (custom `build.ts`) | `dev`, `build` (tsc -b && vite build), `lint`, `preview` | `dev`, `build`, `start`, `typecheck` | `dev`, `build`, `start`, `typecheck`, `format` | `dev`, `build`, `format`, `typecheck` (all via `turbo`) |
| Other niceties | `CLAUDE.md` with Bun guidance, `bunfig.toml`, `APITester.tsx` demo, bun-native API route example | Hero image, minimal demo | default welcome page | default welcome page (shadcn-styled, Geist variable font, dark-mode CSS variables) | default welcome page; shared UI package with Outfit font, zod, remixicon |

---

## How each starter stacks up against the requirements

### 1. `bun/` — Bun-native, no React Router

- Fulfills: React 19, TypeScript, shadcn + Tailwind (6 components pre-seeded), Bun.
- Missing: React Router 7, SSR/SSG (uses `Bun.serve` routes instead — incompatible with the RR SSG story), Vite, Playwright, React Compiler, Vitest.
- Notes: The accompanying `CLAUDE.md` actively tells agents **not** to use Vite and **not** to use Vitest (prefers `bun test`). That conflicts directly with the stated stack (React Router 7 for SSG + Vite + Vitest considered).

### 2. `vite/` — Vanilla Vite + React 19 + TS

- Fulfills: React 19, TypeScript, Vite. ESLint configured.
- Missing: React Router 7, Tailwind, shadcn, Playwright, React Compiler, Vitest. Essentially a blank slate.
- Notes: Useful as a reference for a clean ESLint flat config, but everything else has to be added by hand.

### 3. `router/` (== `remix/`) — Default React Router 7 template

- Fulfills: React 19, React Router 7 (with `ssr: true`, supports SSG via RR's prerendering), TypeScript, Vite, Tailwind v4.
- Missing: shadcn (no `components.json`, no `cn` utility, no radix), Playwright, React Compiler, Vitest, Prettier, ESLint.
- Notes: Ships a Dockerfile. Closest "official React Router" baseline. Can be augmented with the shadcn CLI (`bunx --bun shadcn@latest init`).

### 4. `shadcn/` — React Router 7 default + shadcn preinstalled

- Fulfills: React 19, React Router 7 (SSR/SSG capable), TypeScript, Vite, Tailwind v4, shadcn/ui (radix-nova style), Prettier w/ Tailwind plugin.
- Missing: Playwright, React Compiler, Vitest, ESLint.
- Notes: This is exactly `router/` plus the shadcn init (`--template react-router --base radix`) plus Prettier. Uses `~/` alias and `app/` layout, matching the RR convention. Single-app (no monorepo overhead).

### 5. `monorepo/` — Turbo workspace with shadcn in a shared UI package

- Fulfills: Everything `shadcn/` does, structured as a monorepo (`apps/web` + `packages/ui`).
- Missing: Same as `shadcn/` — Playwright, React Compiler, Vitest, ESLint.
- Notes: Uses Turbo, bun as package manager, explicit `"engines": "node>=20"`. Overhead is not trivial: two `tsconfig.json`s in the UI package, `@workspace/ui` exports, cross-package CSS path. Valuable only if you expect multiple apps (web, docs, admin, mobile) sharing UI. For a single-app project it is pure extra complexity.

### What none of them have

- **React Compiler**: Must be added manually (`babel-plugin-react-compiler` + `@react-router/dev` babel config or Vite plugin).
- **Playwright**: Must be added manually (`bun add -D @playwright/test && bunx playwright install`).
- **Vitest**: Must be added manually if kept in the stack (note: with SSR/SSG React Router + Playwright E2E, Vitest is optional).
- **SSG configuration**: React Router's `ssr: true` enables SSR; true static pre-rendering requires setting `prerender` in `react-router.config.ts`. None of the starters configure this.

---

# Summary

**Pick `shadcn/`.**

Reasons it wins:

1. It is the only starter that ticks four of the five fixed requirements out of the box — **React 19**, **React Router 7**, **TypeScript**, **shadcn + Tailwind v4** — with zero additional scaffolding.
2. It is effectively `router/` + `shadcn init` + Prettier. You get the canonical React Router 7 SSR/SSG setup (`ssr: true`, `app/root.tsx`, `app/routes.ts`, Vite, Tailwind v4) without any monorepo bureaucracy.
3. Its shadcn config uses the modern `radix-nova` style via the `radix-ui` umbrella package, `lucide-react`, `tw-animate-css`, and the current `shadcn` runtime — matching what a fresh `shadcn@latest init` produces today.
4. `monorepo/` is strictly more machinery for the same app shell; Turbo + workspace-exported CSS paths are not worth the overhead unless you actually plan to ship more than one app. `bun/` breaks the React-Router-for-SSG requirement entirely. `vite/` is missing Tailwind, shadcn, and routing. `router/` is a strict subset of `shadcn/`.
5. Prettier is already wired up with `prettier-plugin-tailwindcss` and `tailwindFunctions: ["cn", "cva"]`, which is the fiddly part to get right.

## What to bring over from the other starters

From **`router/`** (and its twin `remix/`):
- Nothing material — `shadcn/` is a strict superset. Safe to ignore.

From **`vite/`**:
- The ESLint flat config (`eslint.config.js`) with `typescript-eslint`, `eslint-plugin-react-hooks`, `eslint-plugin-react-refresh`. `shadcn/` ships Prettier but no linter; drop this file in and add the deps.

From **`bun/`**:
- The `CLAUDE.md` pattern (project-level agent guidance), adapted to the React Router stack — useful if agents work on the repo.
- The extra pre-seeded shadcn components (`card`, `input`, `label`, `select`, `textarea`). In `shadcn/` you only get `button`; run `bunx --bun shadcn@latest add card input label select textarea` once to match.
- The stricter `tsconfig` flags: `noUncheckedIndexedAccess`, `noImplicitOverride`, `noFallthroughCasesInSwitch`. Worth copying into `shadcn/tsconfig.json`.
- Bun itself as the package manager (`shadcn/` already has `bun.lock`, so this is effectively done).

From **`monorepo/`**:
- Only adopt if/when a second app appears. If that day comes, the migration path is: move `shadcn/app/*` into `apps/web/`, lift `app/components/ui` + `app/lib` into `packages/ui`, copy `turbo.json` and the root `package.json` workspaces block.
- The `"engines": { "node": ">=20" }` and `"packageManager": "bun@1.3.11"` fields — worth pinning in `shadcn/package.json` regardless.

## What still needs to be added manually (none of the starters provide these)

- **React Compiler**: add `babel-plugin-react-compiler` and enable it via the React Router Vite plugin's Babel options.
- **Playwright**: `bun add -D @playwright/test`, `bunx playwright install`, add `e2e/` and a `playwright.config.ts`.
- **SSG prerendering**: set `prerender: true` (or an array of routes) in `react-router.config.ts`.
- **Vitest** (if kept in the stack): `bun add -D vitest @vitest/ui jsdom @testing-library/react`. Given Playwright is already chosen for E2E, Vitest is only needed for unit/component tests.
