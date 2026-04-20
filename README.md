# Project Starters

Commands executed on Monday 20th April 2026

Commands executed to set up each subdirectory:

## shadcn

```bash
bunx --bun shadcn@latest init --template react-router --name shadcn --base radix --no-monorepo --no-rtl -y
```

## monorepo

```bash
bunx --bun shadcn@latest init --template react-router --name monorepo --base radix --monorepo --no-rtl -y
```

## router

```bash
npx create-react-router@latest router --no-install --no-git-init --yes
```

## remix

```bash
npx create-react-router@latest remix --template remix-run/react-router-templates/default --no-install --no-git-init --yes
```

## vite

```bash
bun create vite@latest vite --template react-ts --no-interactive
```

## bun

```bash
bun init --react=shadcn bun -y
```

Note: `bun init` created the project in a subdirectory named `init`; it was renamed to `bun`.

## orange

```bash
git clone https://github.com/yluiop123/orange.git /tmp/orange-repo
mv /tmp/orange-repo/vite-shadcn orange
```
