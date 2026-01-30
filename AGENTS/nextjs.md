---
name: nextjs-agent
description: Next.js agent guide for Take Flight and client implementations. Covers structure, standards, commands, accessibility, and best practices. Use when working on Next.js projects in this repo.
---

# Next.js Agent Guide (Take Flight and Client Implementations)

This guide is for AI agents first, and experienced developers second. It focuses on the Take Flight v3 template project and client implementations like KCare (Next.js App Router, ecommerce, and integrations).

## Absolute Rule: No Assumptions

Do not assume any paths, commands, or conventions. If anything is unclear or missing, ask the user for clarification or confirmation before proceeding.

## Scope and Focus

- Next.js App Router structure and route conventions.
- Client implementation patterns (Take Flight template and KCare-style projects).
- Ecommerce providers, server actions, and project configuration.
- Linting, formatting, and TypeScript conventions.
- Accessibility and frontend best practices.

## Project Structure (Discover, Do Not Guess)

### Take Flight v3 (Nx Monorepo)

- `modules/` contains core, reusable React components and provider logic.
- `template-project/` contains the Next.js implementation.
- `template-project/src/app/` uses App Router with locale and route groups.
- `template-project/src/components/` contains project-specific UI.

Typical App Router layout:

- `src/app/[locale]/(shop)/[[...ecommerce-path]]/`
- `src/app/[locale]/(shop)/product/[urlKey]/`
- `src/app/[locale]/(shop)/category/[[...urlKey]]/`
- `src/app/[locale]/(checkout)/checkout/`
- `src/app/_actions/` for server actions (cart, checkout, auth)
- `src/app/api/` for API routes

### Client Implementations (KCare-style)

- `src/app/` App Router with locale segment and route groups.
- `src/components/` for UI, including `makeswift/` blocks and registrars.
- `src/e-comm-providers/` for Adobe Commerce integration and transformers.
- `src/i18n/` and `messages/` for next-intl and translations.
- `src/proxies/` and `src/projectConfig.ts` for route handling and config.
- `src/generatedGql/` for GraphQL codegen output.

If a folder does not exist, do not create it without user approval.

## Coding Standards (Enforce All)

These projects use `@aligent/ts-code-standards` as the baseline. Follow repository configs exactly.

- ESLint: React config from `@aligent/ts-code-standards`
- Rule: `@typescript-eslint/consistent-type-assertions` set to `never`
- Prettier: `@aligent/ts-code-standards` with Tailwind support
  - `tailwindAttributes: ['classNames']`
  - `tailwindFunctions: ['clsx', 'classNames']`
- EditorConfig: 4-space indent, LF, max line length 100
  - 2-space indent for `package.json`, `*.yml`, `*.md`
- TypeScript: extends `@aligent/ts-code-standards/tsconfigs/react.json`
  - path alias `@/*` -> `src/*`
  - `moduleResolution: bundler`
  - Next.js TS plugin enabled

If any config is missing, ask the user to provide it before changing behavior.

## Build and Dev Commands (Use the Correct Context)

You must choose the correct command set based on the repo. If you are not sure, ask.

### Take Flight v3 (Root Nx Workspace)

From `take-flight-v3/package.json`:

- `yarn dev` (generate types, extract classnames, run template project)
- `yarn lint`
- `yarn check-types`
- `yarn test`
- `yarn test-storybook`
- `yarn storybook`
- `yarn generate-types`

### Take Flight Template Project (App Implementation)

From `take-flight-v3/template-project/package.json`:

- `yarn dev` (Next.js dev with `--webpack`)
- `yarn build`, `yarn start`
- `yarn build-adobe`
- `yarn lint:check`, `yarn lint:fix`
- `yarn check-types`
- `yarn storybook`, `yarn test-storybook`
- `yarn test`
- `yarn generate-types`
- `yarn verify-static-build`

### Client Repo (KCare-style)

From `kcare-frontend/package.json`:

- `yarn dev`, `yarn build`, `yarn start`, `yarn build-adobe`
- `yarn lint:check`, `yarn lint:fix`
- `yarn check-types`, `yarn test`
- `yarn storybook`, `yarn test-storybook`
- `yarn generate-types`, `yarn verify-static-build`

Do not run any command unless the user explicitly asks you to.

## Accessibility (Good Practices)

Keep changes accessible and WCAG-aligned. Do not introduce regressions.

- Use semantic HTML (`nav`, `main`, `section`, headings).
- Provide skip links (e.g., `skipToMainContent`).
- Use `aria-label`, `aria-live`, `aria-expanded`, and `aria-roledescription` as appropriate.
- Ensure keyboard navigation works for menus and carousels.
- Use `sr-only` patterns for screen reader-only text.
- Keep Storybook a11y addon checks green where configured.

## Conventions and Best Practices (Frontend)

- Server Components by default. Add `'use client'` only when needed.
- Place mutations in server actions under `src/app/_actions/`.
- Use `next-intl` with `src/i18n/` and `messages/` translations.
- Keep ecommerce logic in `src/e-comm-providers/` and export methods from `src/ecommMethods.ts`.
- Use `projectConfig.ts` for proxies, routing, and provider wiring.
- Makeswift integration lives under `src/components/makeswift/` and `app/.../makeswift/` routes.
- Tailwind v4 with `clsx`/`classNames`. Prefer `classNames` prop when components support it.
- Avoid `setTimeout`/`setInterval` unless a deterministic alternative is impossible and approved.
- Use early returns and `handle*` event naming for handlers.

## Critical Examples

### Server Action Wrapper (Cart)

```tsx
'use server';

import { addToCart as addToCartMethod } from '@/ecommMethods';
import { ActionResult } from '@aligent/take-flight/e-comm-providers/types/EcommerceProvider';
import { Cart } from '@aligent/take-flight/e-comm-providers/types/cart';

export async function addToCart(
  quantity: number,
  productId: string,
  selectedOptions: Record<string, string>,
  locale?: string
): Promise<ActionResult<Cart>> {
  return await addToCartMethod(quantity, productId, selectedOptions, locale);
}
```

### E-comm Method Composition

```ts
import { projectConfig } from '@/projectConfig';
import { defaultTransformProduct } from '@aligent/take-flight/e-comm-providers';
import { makeGetProduct } from '@aligent/take-flight/e-comm-providers/AdobeProvider';

export const getProduct = makeGetProduct(
  projectConfig.apolloClient,
  defaultTransformProduct
);
```

### Makeswift Component Registration

```tsx
import { ReactRuntime } from '@aligent/makeswift/library/runtime/react';
import registerCarousel from './makeswift/Carousel/Carousel.makeswift';

export default function registerComponents(runtime: ReactRuntime) {
  registerCarousel(runtime);
}
```

### Locale-Aware Client Component

```tsx
'use client';

import { useTranslations } from 'next-intl';

const SkipToMainLink = () => {
  const t = useTranslations('Header');

  return (
    <a href="#main-content" className="sr-only focus:not-sr-only" aria-label={t('skipToMainContent')}>
      {t('skipToMainContent')}
    </a>
  );
};

export default SkipToMainLink;
```

## Agent Workflow (Recommended)

1. Read repository structure before proposing changes.
2. Create a plan before implementing changes and ask the user questions for clarification (do not guess).
2. Ask for missing commands or paths.
4. Apply small, reversible changes.
5. Re-run linters only if commands are provided.

## Subagent Output Requirements

- When delegating to any subagent, include the full subagent output in the final response.
- Do not omit sections (e.g., Mermaid diagrams, sequences, data flow charts).
- If output contains diagrams, render them verbatim in the response.