You are a meticulous engineering assistant operating on this repository. Your single task is to analyze the entire monorepo and then create or update a top-quality `copilot-instructions.md` at the repository root.

## Objective

Produce a canonical, self-contained guide that captures how this monorepo is structured and how contributors (and AI tools) should write code here. The doc must be _authoritative_, _actionable_, and _derived from this repo’s actual source_—not generic advice.

## Ground truth about this repo (treat as constraints)

- Monorepo uses **pnpm workspaces**.
- Workspace package groups:

  - **libs**: shared packages including:
    - `core` — common utilities (React hooks, utils, remote fetcher).
    - `effector` — wrapper around npm `effector`; _all code must import this workspace package_, not the upstream package.
    - `eslint-config` — shared ESLint configuration for all packages/apps.
    - `styling` — extends `styled-components` functionality shared across apps/ui-kit.
    - `ui-kit` — shared UI components; many wrap **antd** with sensible defaults + some original components.
  - **features**: end-to-end features (e.g. `auth`, `logs`) consumed by apps.
  - **sites**: static Astro sites (currently **not** included in the pnpm workspace).
  - **apps**: full TypeScript-first React apps built with **Vite**. They use Prettier, ESLint (from `libs/eslint-config`), `styled-components` with theming, and **state management via the workspace `@ese/effector`** wrapper. Remote data handling is done with `@devexperts/remote-data-ts`.
    - For each feature that calls remote APIs, there is a `data-access/` folder with exactly three files:
      1. `*.data-access.ts` — performs the request(s).
      2. `*.api-model.ts` — API types + `io-ts` runtime validators/codecs.
      3. `*.api-mapper.ts` — pure mapping functions A→B from API shape to FE domain shape.

- Common code style is expected to be **generated/encoded by an AI agent** (this document is the source of truth).

## Deliverable

Create or update `/copilot-instructions.md`. If it exists, _merge and preserve_ any manual edits (do not clobber). The doc must include the sections below and fill in concrete, repo-specific details discovered from code and config.

## Required sections (fill with real data discovered in this repo)

1. **Overview & Purpose**
   - What this monorepo is, its technology stack, and the intent of this document.
2. **Workspace Layout**
   - Parse `pnpm-workspace.yaml` and root `package.json` to list workspace globs.
   - Output a _human-readable_ tree with package names, relative paths, and each package’s `name`, `type` (app/lib/feature/site), and key dependencies detected (e.g., React, Vite, antd, styled-components, effector).
   - Identify packages outside the workspace (e.g., `sites/*`) and note how they’re built/tested.
3. **Tooling & Runtimes**
   - Node version(s) (from `.nvmrc`, `.tool-versions`, `engines`).
   - pnpm version (if pinned).
   - Vite config conventions found.
   - TypeScript config resolution (`tsconfig.base.json`, per-package `tsconfig.json`, path aliases).
   - ESLint + Prettier setup, how to extend `libs/eslint-config`.
4. **Imports & Module Boundaries**
   - State unequivocally: _never import `effector` directly_; always import from the workspace package, e.g. `@ese/effector` (or the actual local scope/name).
   - How to import from `libs/*` and `features/*` (document path aliases).
   - ui-kit import policy (e.g., prefer `@libs/ui-kit` over `antd` directly; list any allowed exceptions).
5. **Styling & Theming**
   - How to use `styled-components` in apps and ui-kit.
   - Where the theme lives and how to access it (ThemeProvider location, theme typing).
   - Any helpers/utilities provided by `libs/styling` (list them with their signatures).
6. **State Management with `@ese/effector`**
   - The wrapper package name and entry points.
   - Any custom factories/effects/stores from the wrapper and local conventions (naming, file structure).
   - Example code showing creation of stores/effects using the wrapper.
7. **Remote Data Pattern (`@devexperts/remote-data-ts`)**
   - Show the lifecycle (initial → pending → success/failure) and how the UI should handle it.
   - Example component pattern consuming `RemoteData`.
8. **Feature Data-Access Pattern (the 3 files)**
   - Present the canonical structure and naming:
     - `*.api-model.ts` — define API types and `io-ts` codecs.
     - `*.api-mapper.ts` — map `Api` → domain types (pure functions).
     - `*.data-access.ts` — call remote endpoint using `libs/core` fetcher; decode with codec; return `RemoteData`.
   - Insert fully working **repo-adapted** examples using real utilities, real import paths, and a small real endpoint from this codebase if available. If not available, create minimal, accurate placeholders consistent with existing utils.
   - Include an example inspired by:
     - API: `type CountryApi = { country_name: string; country_id: string }`
     - FE: `type Country    = { countryName: string; countryId: string }`
     - with `io-ts` codec, mapper, and a data-access function returning `RemoteData<Error, Country[]>`.
9. **HTTP/Fetcher Conventions**
   - Where the fetcher lives (e.g., `libs/core`), its surface (timeout, baseURL, interceptors), and error model.
   - How to attach auth (token location, header injection).
10. **Error Handling & Logging**
    - How errors propagate through data-access → `RemoteData`.
    - Any logging utilities or `logs` feature usage.
11. **Code Style Rules (Authoritative)**
    - Language level (strict TS settings).
    - Naming, file suffixes (`*.data-access.ts`, `*.api-model.ts`, `*.api-mapper.ts`), folder structure.
    - Import order and restrictions (no deep imports unless allowed, no direct `effector`, prefer `ui-kit` wrappers).
    - React component conventions (FC typing, props naming, children handling).
    - Testing approach (if present): frameworks, where tests live, naming.
12. **How to Add a New Feature**
    - A concise, step-by-step recipe to scaffold a new feature (folder structure, the 3 data-access files, store/effects with `@ese/effector`, UI wiring with `RemoteData`).
    - Provide ready-to-paste code snippets aligned to _this_ repo’s imports and theme/types.
13. **Apps: Build, Run, Lint, Test**
    - Per-app commands inferred from `package.json` scripts (dev, build, preview, test, lint, typecheck).
    - Document workspace-wide commands (e.g., `pnpm -r build`) if applicable.
14. **Sites (Astro)**
    - Where they live; how to develop/build/deploy them; why they’re not in the workspace; any known caveats.
15. **Appendix**
    - Glossary of internal package names and what they provide.
    - Known pitfalls and “Do not do” list (e.g., “Do not import `effector` directly”).
    - Maintenance tips: how to evolve these rules safely.

## Mandatory examples (tailor to this repo’s names/paths)
