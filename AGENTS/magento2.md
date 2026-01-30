---
name: magento2-agent
description: Magento 2 custom theme agent guide. Covers theme/module structure, React/Vite integration, standards, commands, accessibility, and best practices. Use for Magento 2 frontend work in this repo.
---

# Magento 2 Custom Theme Agent Guide (CCG Standard)

This guide is for AI agents first, and experienced developers second. It uses a Magento 2 project structure standard for custom themes, modules, and React integrations.

## Absolute Rule: No Assumptions

Do not assume any paths, commands, or conventions. If anything is unclear or missing, ask the user for clarification or confirmation before proceeding.

## Scope and Focus

- Theme inheritance based on Luma, with project-specific overrides.
- Frontend assets (layout XML, templates, LESS, JS modules).
- React components built with Vite and Tailwind, when present.
- Frontend and PHP linting, formatting, and standards.
- Accessibility and frontend best practices.

## Project Structure (Discover, Do Not Guess)

Use these placeholder paths as the baseline. Confirm exact locations before editing.

### Theme (Luma Extension)

- `app/design/frontend/<Vendor>/<Theme>/`
- `app/design/frontend/<Vendor>/<Theme>/theme.xml`
- `app/design/frontend/<Vendor>/<Theme>/registration.php`
- `app/design/frontend/<Vendor>/<Theme>/i18n/<locale>.csv`
- `app/design/frontend/<Vendor>/<Theme>/web/css/source/`
- `app/design/frontend/<Vendor>/<Theme>/web/js/`
- `app/design/frontend/<Vendor>/<Theme>/web/images/`
- `app/design/frontend/<Vendor>/<Theme>/web/fontawesome/`
- Module overrides:
  - `app/design/frontend/<Vendor>/<Theme>/Magento_Theme/`
  - `app/design/frontend/<Vendor>/<Theme>/Magento_Catalog/`
  - `app/design/frontend/<Vendor>/<Theme>/Magento_Checkout/`
  - `app/design/frontend/<Vendor>/<Theme>/Magento_Customer/`

### Custom Modules

- `app/code/<Vendor>/`
- Modules follow standard Magento structure (`etc/`, `Block/`, `Model/`, `view/`, `registration.php`)
- Theme helpers and feature modules live here (catalog, quick order, store location, access control)

### React + Vite Integration (Module-Based)

- `app/code/<Vendor>/React/view/frontend/src/app/`
  - `components/`
  - `widgets/` (Vite entry points)
- Build output:
- `app/code/<Vendor>/React/view/frontend/web/`
- Tooling config at repo root:
  - `vite.config.js`
  - `tailwind.config.js`
  - `postcss.config.mjs`

If any folder is missing, do not create it without user approval.

## Coding Standards (Enforce All)

Follow all defined linters and formatting tools in the repository. Do not relax rules.

- JavaScript/TypeScript:
  - ESLint via `@aligent/ts-code-standards`
  - `@typescript-eslint/consistent-type-assertions: never`
  - `eslint-config-prettier`
- Prettier:
  - `@aligent/ts-code-standards` baseline
  - Tailwind support with `classNames`, `clsx`
- EditorConfig:
  - 4-space indent, LF, max line length 100
  - 2-space indent for `package.json`, `*.yml`, `*.md`
- PHP:
  - PHPCS with Magento2 standard
  - PHPCBF for auto-fix
  - PHPStan for analysis

If a config is missing, ask the user to provide it before changing behavior.

## Build and Dev Commands (Confirm Before Running)

Use the correct command set for the repo. Do not run commands unless the user asks.

### React Module (Vite)

From `package.json`:

- `yarn dev`
- `yarn build`
- `yarn lint:check`
- `yarn lint:fix`
- `yarn check-types`

These target the React module under `app/code/CountryCareGroup/React/view/frontend/src/app`.

### PHP Code Quality

From `composer.json` scripts:

- `composer run check-style`
- `composer run fix-style`
- `composer run analyse`

If your environment wraps commands through `manta`, ask for the exact wrapper commands.

## Accessibility (Good Practices)

Keep changes accessible and WCAG-aligned. Do not introduce regressions.

- Use semantic HTML elements and correct heading order.
- Ensure interactive elements are keyboard-accessible.
- Provide visible focus styles.
- Provide accessible names via `aria-label`, `aria-labelledby`, or visible text.
- Use `aria-live` for dynamic content (cart, filters, notifications).
- Ensure color contrast meets accessibility guidelines.

## Conventions and Best Practices (Frontend)

- Keep layout XML minimal and predictable; prefer override blocks over copy-paste.
- Keep JS modular and scoped. Use `requirejs-config.js` where needed.
- Use LESS in `web/css/source/` and keep overrides in theme scope.
- React widgets must live under the React module and be built with Vite.
- Widgets in `widgets/` are the Vite entry points; output goes to `view/frontend/web/`.
- Use Tailwind utility classes consistently; prefer `classNames` props when provided.
- Avoid `setTimeout`/`setInterval` unless a deterministic alternative is impossible and approved.
- Use early returns and `handle*` naming for event handlers.
- Ensure changes work across local, staging, and production builds.

## Critical Examples

### Theme Registration

```php
<?php
use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::THEME,
    'frontend/CCG/ccg',
    __DIR__
);
```

### Layout Override (Structure Example)

```xml
<!-- app/design/frontend/<Vendor>/<Theme>/Magento_Catalog/layout/catalog_product_view.xml -->
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <body>
        <referenceBlock name="product.info">
            <block class="Magento\Framework\View\Element\Template"
                   name="vendor.product.info"
                   template="Magento_Catalog::product/view/custom-info.phtml"/>
        </referenceBlock>
    </body>
</page>
```

### React Widget Entry (Vite)

```tsx
import React from 'react';
import { createRoot } from 'react-dom/client';

const mountQuickOrder = () => {
  const rootElement = document.getElementById('quick-order-root');

  if (!rootElement) {
    return;
  }

  const root = createRoot(rootElement);
  root.render(
    <div role="status" aria-live="polite">
      Quick order loaded
    </div>
  );
};

mountQuickOrder();
```

### Accessible Trigger (Template Example)

```html
<button
  type="button"
  class="action primary"
  aria-label="Open quick order"
  tabindex="0"
  onkeydown="if (event.key === 'Enter') { this.click(); }"
>
  Quick Order
</button>
```

## Agent Workflow (Recommended)

1. Read repository structure before proposing changes.
2. Create a plan before implementing changes and ask the user questions for clarification (do not guess).
2. Ask for missing `manta` commands or paths.
4. Apply small, reversible changes.
5. Re-run linters only if commands are provided.

## Subagent Output Requirements

- When delegating to any subagent, include the full subagent output in the final response.
- Do not omit sections (e.g., Mermaid diagrams, sequences, data flow charts).
- If output contains diagrams, render them verbatim in the response.