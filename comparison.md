# Project Starter Comparison

This document compares five of the six project starters in this repo against the target stack. The `orange/` starter is intentionally excluded.

## Target stack

**Fixed choices (must be satisfied):**

- React 19 with the React Compiler
- React Router 7 (used for SSG)
- TypeScript (including a `tsconfig.json` — required even when using Bun; Bun's bundler can work without one but `tsc`, editors, and path aliases all read it)
- shadcn/ui with Tailwind
- Playwright for end-to-end testing
- Prettier
- ESLint
- Zustand
- Zod
- TanStack Query
- Lodash
- Axios

**Under consideration:**

- Vite
- Vitest
- Bun
- Mock Service Worker (MSW)
- Testing Library
- Storybook
- tRPC

---

## Remix vs Router: are they the same?

**Yes — byte-identical apart from the `name` field in `package.json`.**

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

`remix-run/react-router-templates/default` *is* the default template when no `--template` flag is supplied, so both commands produce the same files. From here on they are treated as one entry: **`router`**.

---

## Core stack coverage

This table only lists things that are present in at least one starter. Items missing from every starter live in the *Always missing* section below.

| Feature | bun | vite | router (=remix) | shadcn | monorepo |
|---|---|---|---|---|---|
| React 19 | 19.x | 19.2.5 | 19.2.4 | 19.2.4 | 19.2.4 |
| React Router 7 | no | no | 7.14.0 | 7.13.1 | 7.12.0 (apps/web) |
| SSR / SSG capable | no | no (SPA) | yes (`ssr: true`) | yes (`ssr: true`) | yes (`ssr: true`) |
| TypeScript | yes | yes | yes | yes | yes |
| `tsconfig.json` | yes (strict + `noUncheckedIndexedAccess`, `noImplicitOverride`) | yes (split app/node refs, `erasableSyntaxOnly`) | yes (strict) | yes (strict) | yes (strict, per-package) |
| Bundler / dev server | Bun bundler + `Bun.serve` (no Vite) | Vite 8 + `@vitejs/plugin-react` | Vite 8 + `@react-router/dev/vite` | Vite 7 + `@react-router/dev/vite` + `vite-tsconfig-paths` | Vite 7 + `@react-router/dev/vite` + `vite-tsconfig-paths` |
| Tailwind | v4 via `bun-plugin-tailwind` | no | v4 via `@tailwindcss/vite` | v4 via `@tailwindcss/vite` | v4 via `@tailwindcss/vite` |
| shadcn/ui | yes — `style: new-york`, 6 preseeded components (button, card, input, label, select, textarea) using raw `@radix-ui/react-*` pkgs | no | no | yes — `style: radix-nova`, 1 preseeded component (button), uses the umbrella `radix-ui` pkg + `shadcn` runtime | yes — same as `shadcn/` but shared via `packages/ui` and consumed as `@workspace/ui/*` |
| Prettier | no | no | no | yes (`.prettierrc` + `prettier-plugin-tailwindcss`, `tailwindFunctions: ["cn", "cva"]`) | yes (root `.prettierrc`, same plugin) |
| ESLint | no | **yes** (flat config, `typescript-eslint` + `react-hooks` + `react-refresh`) | no | no | no |
| Zod | no | no | no | no | yes (in `packages/ui`, `zod@^3.25.76`) |
| Path aliases | `@/*` → `./src/*` | none | `~/*` → `./app/*` | `~/*` → `./app/*` | `@/*` → `./app/*` + `@workspace/ui/*` |
| Package manager | bun (lockfile) | none specified | none specified | bun (lockfile) | bun (`"packageManager": "bun@1.3.11"`, `"engines": "node>=20"`) |
| Monorepo tooling | no | no | no | no | yes — Turbo 2.8 with `apps/*`, `packages/*` |
| Dockerfile | no | no | yes (node:20-alpine multistage) | yes | yes (in `apps/web`) |
| Notable extras | `CLAUDE.md` agent guidance, `bunfig.toml`, `APITester.tsx`, custom `build.ts`, Bun-native API route example | hero image demo | welcome page, Google Fonts Inter via preconnect | dark-mode CSS vars baked in, `tw-animate-css` preinstalled | Turbo tasks graph, shared UI package with own `components.json`, `@remixicon/react` + Outfit font |

---

## Icons

| Starter | Icon library |
|---|---|
| bun | `lucide-react` (via shadcn init) |
| vite | none |
| router | none |
| shadcn | `lucide-react` (shadcn `iconLibrary: "lucide"`) |
| monorepo | `lucide-react` (shadcn config) **and** `@remixicon/react` (preinstalled in both `apps/web` and `packages/ui`) |

## Fonts / Typography

| Starter | Font setup |
|---|---|
| bun | browser defaults (no font package) |
| vite | browser defaults |
| router | **Inter** from Google Fonts via `<link rel="preconnect">` + `<link rel="stylesheet">` in `app/root.tsx` |
| shadcn | **Geist Variable** via `@fontsource-variable/geist`, imported in `app/app.css`; `--font-sans: 'Geist Variable'` wired into the `@theme` block |
| monorepo | **Outfit Variable** via `@fontsource-variable/outfit`, imported in `packages/ui/src/styles/globals.css` |

## Animation

| Starter | Animation library |
|---|---|
| bun | `tw-animate-css` (devDep) |
| vite | none |
| router | none |
| shadcn | `tw-animate-css` (imported via `@import "tw-animate-css";` in `app/app.css`) |
| monorepo | `tw-animate-css` (imported in `packages/ui/src/styles/globals.css`) |

Note: no starter ships Framer Motion, Motion One, GSAP, auto-animate, or React Spring.

---

## Always missing (absent from every starter)

These need to be added manually regardless of which starter is chosen, so they are *not* useful as selection criteria:

**From the fixed stack:**

- React Compiler (`vite/` README mentions how to enable it but doesn't configure it)
- Playwright
- Zustand
- TanStack Query
- Lodash
- Axios
- SSG prerender configuration (`prerender` in `react-router.config.ts`)

**From the "under consideration" list:**

- Vitest
- Mock Service Worker (MSW)
- Testing Library
- Storybook
- tRPC

---

## Per-starter verdict against the fixed stack

### `bun/`

- Has: React 19, TS + tsconfig, shadcn (+ 6 components), Tailwind, icons (lucide), animation (tw-animate-css).
- Missing: React Router 7, SSR/SSG, Prettier, ESLint, Zod, Vite.
- Blocker: the ships-with `CLAUDE.md` explicitly instructs agents **not** to use Vite and **not** to use Vitest. That directly conflicts with the RR-for-SSG + (maybe) Vitest part of the target stack.

### `vite/`

- Has: React 19, TS + split tsconfig, Vite, ESLint.
- Missing: React Router 7, Tailwind, shadcn, Prettier, Zod, icons, fonts, animation.
- Essentially a blank slate. Keep ESLint config as a reference.

### `router/` (== `remix/`)

- Has: React 19, React Router 7 (newest of the three RR-based starters: 7.14.0), TS, Vite, Tailwind v4, Dockerfile, Google Fonts Inter wired into `root.tsx`.
- Missing: shadcn (no `components.json`, no `cn`, no radix), Prettier, ESLint, Zod, icons, animation.

### `shadcn/`

- Has: everything `router/` has (minus the welcome page and Google Fonts) **plus** shadcn (`radix-nova` style), Prettier, `tw-animate-css`, Geist font, dark-mode CSS variables, `lucide-react`.
- Missing: ESLint, Zod.
- Note on the earlier "strict superset of `router/`" claim — **not strictly true.** `shadcn/` replaces a few things rather than purely adding to `router/`:
  - Uses React Router **7.13.1** (older than `router/`'s 7.14.0).
  - Removes `app/welcome/` and replaces `routes/home.tsx` with a button demo.
  - Drops Google Fonts preconnect from `root.tsx` in favor of bundled `@fontsource-variable/geist`.
  - Adds `vite-tsconfig-paths` plugin to `vite.config.ts`.

### `monorepo/`

- Has: everything `shadcn/` has, *plus* Zod, a second icon pack (`@remixicon/react`), `engines`/`packageManager` pins, Turbo.
- Uses React Router **7.12.0** (oldest of the three).
- Missing: ESLint.
- Overhead: two `tsconfig.json`s in the UI package, `@workspace/ui` exports, cross-package CSS source globs. Only worth it if a second app is coming.

---

## Summary

**Pick `shadcn/`.**

Why it still wins after the expanded requirements:

1. It covers the largest share of the **fixed** stack in a single-app layout: React 19, React Router 7, TS + tsconfig, Vite, Tailwind v4, shadcn/ui, Prettier + Tailwind plugin, icons (lucide), fonts (Geist), animation (tw-animate-css).
2. Versus `router/`: adds shadcn, Prettier, icons, font, animation — all required or nice-to-have on our list. The cost is an older React Router (7.13 vs 7.14) and a swapped-out welcome page, both trivially reversible.
3. Versus `monorepo/`: avoids Turbo + workspace overhead that pays off only with more than one app. The concrete monorepo-only wins — Zod, `@remixicon/react`, `engines`/`packageManager` pins — are one-line additions to `shadcn/`.
4. Versus `bun/`: `bun/` breaks the React-Router-for-SSG requirement and its `CLAUDE.md` pushes away from Vite/Vitest. Starting there means tearing out the Bun.serve HTTP layer.
5. Versus `vite/`: `vite/` is missing routing, Tailwind, shadcn, Prettier, icons, font, animation — starting there recreates `shadcn/` by hand.

Every starter is missing the same long list (React Compiler, Playwright, Zustand, TanStack Query, Lodash, Axios, Vitest, MSW, Testing Library, Storybook, tRPC, SSG prerender). That list is identical regardless of choice, so it doesn't influence the decision.

## What to bring over from the non-selected starters

From **`router/`** (the vanilla RR template):

- Bump React Router from `7.13.1` → `7.14.0` and keep them in lockstep (`react-router`, `@react-router/node`, `@react-router/serve`, `@react-router/dev`).
- Optional: its `app/welcome/` page and Inter font via Google Fonts preconnect — only if Geist isn't the desired brand font.
- Its multi-stage `Dockerfile` — `shadcn/` already has one, but `router/`'s is the newest reference if they drift.

From **`vite/`**:

- The entire `eslint.config.js` — flat config with `typescript-eslint` + `eslint-plugin-react-hooks` + `eslint-plugin-react-refresh`. `shadcn/` has Prettier but no ESLint; this is the fastest way to satisfy the ESLint requirement.
- The `lint` npm script.

From **`bun/`**:

- Pre-seeded shadcn components beyond button: `card`, `input`, `label`, `select`, `textarea`. Equivalent to one `bunx --bun shadcn@latest add card input label select textarea`.
- Stricter `tsconfig` flags: `noUncheckedIndexedAccess`, `noImplicitOverride`, `noFallthroughCasesInSwitch`. Worth copying into `shadcn/tsconfig.json`.
- Its `CLAUDE.md` pattern (project-level agent guidance), **rewritten** for the React Router + Vite stack — the original explicitly contradicts it.

From **`monorepo/`**:

- `zod` as a direct dependency — required by our stack, monorepo is the only starter that ships it.
- `@remixicon/react` as a second icon pack — optional, depends on design needs; `lucide-react` alone is usually enough.
- `"engines": { "node": ">=20" }` and `"packageManager": "bun@1.3.11"` on the root `package.json`.
- The `@source` directives in globals.css — only relevant if a monorepo split is reintroduced later.

## What must still be added manually (no starter provides them)

- **React Compiler** — `babel-plugin-react-compiler` + the React Router Vite plugin's Babel options.
- **Playwright** — `bun add -D @playwright/test`, `bunx playwright install`, add `e2e/` and `playwright.config.ts`.
- **SSG** — set `prerender: true` (or an explicit route array) in `react-router.config.ts`.
- **Zustand, TanStack Query, Lodash, Axios** — plain `bun add` of each; no configuration beyond a `QueryClientProvider` for TanStack Query.
- **Vitest** (if adopted) — `bun add -D vitest jsdom @testing-library/react @testing-library/jest-dom`, plus a `vitest.config.ts` (or extend `vite.config.ts`).
- **MSW, Testing Library, Storybook, tRPC** — add when the respective need arises.
