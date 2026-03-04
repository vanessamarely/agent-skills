---
name: Component Spec Template
description: Template for frontend component specs (props, accessibility, interactions, tests)
---

# Component Spec: <ComponentName>

> Centralized specs: keep all component specs under `.github/specs/` only.
> Do not create per-project spec files. Use `.github/project-context.md` for project context.

## Purpose
- Short description of the component and where it is used.

## Props / API
- `propName: type` — description (required/optional, default)

## Accessibility Requirements
- Visible label: (yes/no) — describe label text or `aria-label` usage.
- Roles and states: list required ARIA attributes (e.g., `role`, `aria-expanded`).
- Announcements: any `aria-live` needs.

## Keyboard Interaction Matrix
- Tab focus: yes/no
- Enter/Space: activate
- Arrow keys: (describe behavior when applicable)
- ESC: close/dismiss behavior

## Visual Constraints
- Size / spacing constraints
- Contrast requirements (minimum ratios)
- Iconography and alignment

## Example Usage
```tsx
// minimal usage example
<ExampleComponent propA={"value"} onAction={() => {}} />
```

## Expected DOM Structure (short snippet)
- Describe expected elements and semantic structure that tests can assert.

## Acceptance Tests (required)
1. Unit tests for core logic (Jest)
2. Component rendering & keyboard interactions (React Testing Library)
3. Accessibility checks (axe) — include axe assertions in tests or run as part of CI

## CI / Automation
- Which scripts must run in CI for this spec (e.g. `npm run test`, `npm run lint:a11y`, `npm run test:rtl`)

## Notes / Edge cases
- Any special considerations or fallback behavior

## Owner / Reviewer
- Name or team responsible for the component and tests
