# Project Starter Comparison

This document compares all six project starters in this repo against the target stack.

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

| Feature | bun | vite | router (=remix) | shadcn | monorepo | orange |
|---|---|---|---|---|---|---|
| React 19 | 19.x | 19.2.5 | 19.2.4 | 19.2.4 | 19.2.4 | 19.1.0 |
| React Router 7 | no | no | 7.14.0 | 7.13.1 | 7.12.0 (apps/web) | 7.6.2 (SPA only) |
| SSR / SSG capable | no | no (SPA) | yes (`ssr: true`) | yes (`ssr: true`) | yes (`ssr: true`) | no (SPA via `BrowserRouter`/`HashRouter`, switched by `VITE_ROUTE`) |
| TypeScript | yes | yes | yes | yes | yes | yes |
| `tsconfig.json` | yes (strict + `noUncheckedIndexedAccess`, `noImplicitOverride`) | yes (split app/node refs, `erasableSyntaxOnly`) | yes (strict) | yes (strict) | yes (strict, per-package) | yes (split `tsconfig.json` / `tsconfig.app.json` / `tsconfig.node.json`, strict + `noUnusedLocals`, `noUnusedParameters`, `noFallthroughCasesInSwitch`, `noUncheckedSideEffectImports`) |
| Bundler / dev server | Bun bundler + `Bun.serve` (no Vite) | Vite 8 + `@vitejs/plugin-react` | Vite 8 + `@react-router/dev/vite` | Vite 7 + `@react-router/dev/vite` + `vite-tsconfig-paths` | Vite 7 + `@react-router/dev/vite` + `vite-tsconfig-paths` | Vite 6 + `@vitejs/plugin-react-swc` + `@tailwindcss/vite` + `vite-plugin-cesium-build` |
| Tailwind | v4 via `bun-plugin-tailwind` | no | v4 via `@tailwindcss/vite` | v4 via `@tailwindcss/vite` | v4 via `@tailwindcss/vite` | v4 via `@tailwindcss/vite` + `tailwindcss-animate` + `tailwind-scrollbar` plugins |
| shadcn/ui | yes — `style: new-york`, 6 preseeded components (button, card, input, label, select, textarea) using raw `@radix-ui/react-*` pkgs | no | no | yes — `style: radix-nova`, 1 preseeded component (button), uses the umbrella `radix-ui` pkg + `shadcn` runtime | yes — same as `shadcn/` but shared via `packages/ui` and consumed as `@workspace/ui/*` | yes — `style: base-vega` (unique), ~55 preseeded components, uses `@base-ui/react` primitives (not `@radix-ui/*` or `radix-ui`) |
| Prettier | no | no | no | yes (`.prettierrc` + `prettier-plugin-tailwindcss`, `tailwindFunctions: ["cn", "cva"]`) | yes (root `.prettierrc`, same plugin) | no |
| ESLint | no | **yes** (flat config, `typescript-eslint` + `react-hooks` + `react-refresh`) | no | no | no | **yes** (flat config, same plugin set as `vite/`) |
| Zod | no | no | no | no | yes (in `packages/ui`, `zod@^3.25.76`) | yes (`zod@^3.24.4`) |
| Path aliases | `@/*` → `./src/*` | none | `~/*` → `./app/*` | `~/*` → `./app/*` | `@/*` → `./app/*` + `@workspace/ui/*` | `@/*` → `./src/*` |
| Package manager | bun (lockfile) | none specified | none specified | bun (lockfile) | bun (`"packageManager": "bun@1.3.11"`, `"engines": "node>=20"`) | pnpm (`"packageManager": "pnpm@10.28.2"`, no `engines`) |
| Monorepo tooling | no | no | no | no | yes — Turbo 2.8 with `apps/*`, `packages/*` | no |
| Dockerfile | no | no | yes (node:20-alpine multistage) | yes | yes (in `apps/web`) | no (ships `vercel.json` + GitHub Actions workflow instead) |
| Notable extras | `CLAUDE.md` agent guidance, `bunfig.toml`, `APITester.tsx`, custom `build.ts`, Bun-native API route example | hero image demo | welcome page, Google Fonts Inter via preconnect | dark-mode CSS vars baked in, `tw-animate-css` preinstalled | Turbo tasks graph, shared UI package with own `components.json`, `@remixicon/react` + Outfit font | full admin template: login + auth + role/permission system, `react-intl` (en-US + zh-CN), MSW worker setup, Swagger UI route, 7-theme color switcher, Cesium/Three.js/Babylon.js/Deck.gl/Leaflet/OpenLayers/L7 demos, Recharts/ECharts/D3/AntV demos, `@dnd-kit`, `@uppy/*` file upload, `@faker-js/faker` mocks, pre-configured `axios` client |

---

## Icons

| Starter | Icon library |
|---|---|
| bun | `lucide-react` (via shadcn init) |
| vite | none |
| router | none |
| shadcn | `lucide-react` (shadcn `iconLibrary: "lucide"`) |
| monorepo | `lucide-react` (shadcn config) **and** `@remixicon/react` (preinstalled in both `apps/web` and `packages/ui`) |
| orange | `lucide-react@0.488.0` (shadcn config) **and** `@tabler/icons-react@3.31.0` (second pack) |

## Fonts / Typography

| Starter | Font setup |
|---|---|
| bun | browser defaults (no font package) |
| vite | browser defaults |
| router | **Inter** from Google Fonts via `<link rel="preconnect">` + `<link rel="stylesheet">` in `app/root.tsx` |
| shadcn | **Geist Variable** via `@fontsource-variable/geist`, imported in `app/app.css`; `--font-sans: 'Geist Variable'` wired into the `@theme` block |
| monorepo | **Outfit Variable** via `@fontsource-variable/outfit`, imported in `packages/ui/src/styles/globals.css` |
| orange | browser defaults (no `@fontsource-variable/*` package); typography driven by 7 switchable theme CSS files in `src/themes/` |

## Animation

| Starter | Animation library |
|---|---|
| bun | `tw-animate-css` (devDep) |
| vite | none |
| router | none |
| shadcn | `tw-animate-css` (imported via `@import "tw-animate-css";` in `app/app.css`) |
| monorepo | `tw-animate-css` (imported in `packages/ui/src/styles/globals.css`) |
| orange | `tailwindcss-animate@1.0.7` via `@plugin "tailwindcss-animate"` in `src/index.css` (not `tw-animate-css`) |

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

### `orange/`

- Has: React 19, React Router 7.6.2 (SPA-only, `BrowserRouter`/`HashRouter` switch), TS with a split three-file `tsconfig` and stricter lint flags than any other starter (`noUnusedLocals`, `noUnusedParameters`, `noFallthroughCasesInSwitch`, `noUncheckedSideEffectImports`), Vite 6 + SWC, Tailwind v4, shadcn (`base-vega` style, ~55 preseeded components using `@base-ui/react`), ESLint (flat config), Zod, Zustand, Axios (pre-wired client with interceptors), two icon packs, `tailwindcss-animate`.
- Missing from the *fixed* stack: **SSR/SSG** (SPA-only, so it fails the "React Router for SSG" requirement), Prettier, TanStack Query, Lodash, Playwright, React Compiler, fonts, Dockerfile.
- Extras well beyond the stack: login + role/permission system, `react-intl` i18n (en-US + zh-CN), MSW mock worker, Swagger UI, 7-color runtime theme switcher, Cesium/Three.js/Babylon.js/Deck.gl/Leaflet/OpenLayers/L7 demos, Recharts/ECharts/D3/AntV charts, `@dnd-kit`, `@uppy/*` file upload, `@faker-js/faker`, `vercel.json`, GitHub Actions workflow, parallel Chinese README.
- Uniqueness: only starter using `base-vega` shadcn style, only starter using `@base-ui/react` primitives, only starter with `pnpm` as package manager, only starter with pre-seeded i18n and auth.
- Blocker: the bulk of what makes `orange/` large (auth flow, 3D/mapping libs, Swagger docs, i18n, 7 themes, 55 components) is extra weight to *delete* for a greenfield project, not extra weight to build on. And the SPA-only routing setup is backwards from the SSG target — reintroducing SSR means rewriting `App.tsx`, `layout.tsx`, the lazy `import.meta.glob` router, and the env-var-switched `BrowserRouter`/`HashRouter` logic.

---

## Summary

**Pick `shadcn/`.**

Why it still wins after the expanded requirements:

1. It covers the largest share of the **fixed** stack in a single-app layout: React 19, React Router 7, TS + tsconfig, Vite, Tailwind v4, shadcn/ui, Prettier + Tailwind plugin, icons (lucide), fonts (Geist), animation (tw-animate-css).
2. Versus `router/`: adds shadcn, Prettier, icons, font, animation — all required or nice-to-have on our list. The cost is an older React Router (7.13 vs 7.14) and a swapped-out welcome page, both trivially reversible.
3. Versus `monorepo/`: avoids Turbo + workspace overhead that pays off only with more than one app. The concrete monorepo-only wins — Zod, `@remixicon/react`, `engines`/`packageManager` pins — are one-line additions to `shadcn/`.
4. Versus `bun/`: `bun/` breaks the React-Router-for-SSG requirement and its `CLAUDE.md` pushes away from Vite/Vitest. Starting there means tearing out the Bun.serve HTTP layer.
5. Versus `vite/`: `vite/` is missing routing, Tailwind, shadcn, Prettier, icons, font, animation — starting there recreates `shadcn/` by hand.
6. Versus `orange/`: `orange/` is SPA-only (fails SSG), drags in a 55-component admin UI, auth system, i18n, 3D/mapping, charts, Swagger, and file upload — all of which would need to be deleted. Good reference for strict `tsconfig` flags and the ESLint flat config, but far too opinionated as a base.

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

From **`orange/`**:

- The extra `tsconfig` strict flags that neither `shadcn/` nor `bun/` enable: `noUnusedLocals`, `noUnusedParameters`, `noFallthroughCasesInSwitch`, `noUncheckedSideEffectImports`.
- The `eslint.config.js` as a secondary reference — same shape as `vite/`'s but confirmed to work alongside a shadcn/Tailwind stack.
- Pre-configured `axios` client pattern (request/response interceptors in `src/lib/axios.ts`) — a useful template even though we'll rebuild it against our own API.
- MSW setup (`public/mockServiceWorker.js` + the `VITE_MOCK_ENABLE` env-var toggle) if MSW is adopted from the "under consideration" list.
- Not recommended: the `base-vega` shadcn style, the `@base-ui/react` swap, the 55 preseeded components, the 7-theme switcher, the i18n layer, the 3D/mapping demos, Swagger UI, `@dnd-kit`, `@uppy/*`, `@faker-js/faker`, pnpm, `.trae/`, and the admin login scaffolding — none of that belongs in a lean starter.

## What must still be added manually (no starter provides them)

- **React Compiler** — `babel-plugin-react-compiler` + the React Router Vite plugin's Babel options.
- **Playwright** — `bun add -D @playwright/test`, `bunx playwright install`, add `e2e/` and `playwright.config.ts`.
- **SSG** — set `prerender: true` (or an explicit route array) in `react-router.config.ts`.
- **Zustand, TanStack Query, Lodash, Axios** — plain `bun add` of each; no configuration beyond a `QueryClientProvider` for TanStack Query.
- **Vitest** (if adopted) — `bun add -D vitest jsdom @testing-library/react @testing-library/jest-dom`, plus a `vitest.config.ts` (or extend `vite.config.ts`).
- **MSW, Testing Library, Storybook, tRPC** — add when the respective need arises.
