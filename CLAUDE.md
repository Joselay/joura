# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

<!-- BEGIN:nextjs-agent-rules -->
## This is NOT the Next.js you know

This version has breaking changes — APIs, conventions, and file structure may all differ from your training data. Read the relevant guide in `node_modules/next/dist/docs/` before writing any code. Heed deprecation notices.
<!-- END:nextjs-agent-rules -->

## Commands

Package manager is **Bun** (`bun.lock`). Use `bun` / `bunx`, not `npm` / `npx`.

- `bun dev` — Next dev server (Turbopack)
- `bun run build` — production build
- `bun start` — run production build
- `bun run lint` — Biome check (no writes)
- `bun run format` — Biome format (writes)
- `bun run check` — Biome check with safe autofix (writes)
- `bun run typecheck` — `tsc --noEmit`

No test runner is configured.

## Stack

- **Next.js 16.2.4** (App Router, Turbopack dev)
- **React 19.2**
- **TypeScript** strict, path alias `@/*` → project root (no `src/` directory)
- **Tailwind CSS v4** via `@tailwindcss/postcss` (CSS-first config in `app/globals.css`, no `tailwind.config.*`)
- **shadcn/ui** — style `base-mira`, primitives from `@base-ui/react` (not Radix), icons from `@remixicon/react`
- **Biome 2.4** for lint + format + import sorting (replaced ESLint + Prettier)

## Biome configuration notes

- `semicolons: "asNeeded"` — omit semicolons where possible
- `components/ui/**` and `hooks/**` are **lint-disabled** (treated as vendored shadcn output); formatting still applies
- CSS parser has `tailwindDirectives: true` so `@apply` / `@theme` don't error

## shadcn/ui conventions

- `components.json` sets `style: "base-mira"`, `iconLibrary: "remixicon"`, `baseColor: "zinc"`, CSS variables on.
- Add components with `bunx shadcn@latest add <name>`; they land in `components/ui/`.
- `components/ui/` is already populated with ~40 shadcn components (accordion, dialog, chart, carousel, command, drawer, sidebar, etc.) — check `ls components/ui` before re-adding.
- New shadcn components may need a provider in `app/layout.tsx` (e.g. `TooltipProvider` is already wired inside `ThemeProvider`).
- Avoid hand-editing `components/ui/**` — Biome won't lint them, and they're expected to track upstream.

## Custom: ThemeProvider

`components/theme-provider.tsx` wraps `next-themes` and registers a global `d` hotkey that toggles dark/light (skipped when typing in inputs/textareas/contentEditable). If you add global keybindings or form UX, account for this.

## Project layout (current)

```
app/          Next.js App Router (layout.tsx, page.tsx, globals.css)
components/   theme-provider.tsx + ui/ (shadcn components)
lib/          utils.ts (cn helper)
hooks/        shadcn hooks (use-mobile, etc.)
public/       static assets
```

The project is early-stage — no `src/`, `features/`, or route groups exist yet.
