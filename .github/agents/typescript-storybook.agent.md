---
description: 'TypeScript and Storybook best-practices agent for Waterworks. Provides guidance, checks, and enforcement suggestions.'
tools: [vscode, agent]
---

# TypeScript & Storybook Agent for Waterworks

This agent helps enforce and automate TypeScript and Storybook best practices for the `waterworks-component-library` project.

What the agent enforces and suggests:
- Enforce no-`any` rule: prefer `unknown` for external inputs and require type guards or explicit casts when narrowing.
- Require explicit public API typing (function params and return types) for exported components and utilities.
- Suggest using `interface` for extensible object shapes and `type` for unions/intersections.
- Check Storybook stories exist for public components and include accessibility examples and controls.
- Recommend `tsc --noEmit`, `eslint`, and Storybook lint tasks run in CI.

Example checks the agent can perform:
- Scan `src/components` for exported functions/components without explicit types.
- Detect usage of `any` and propose replacements or type guards.
- Verify Storybook stories (`**/*.stories.tsx`) exist for exported components.

How to use:
- Run local checks: `npm run lint && npm run typecheck && npm run storybook:ci` (if defined).
- Use the agent to create PR suggestions for replacing `any` with `unknown` and adding type guards.
