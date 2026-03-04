```instructions
Repository Copilot Instructions

This repository contains many separate projects. Treat each top-level folder under the repository root as an independent project (client app or microservice). Do NOT treat the workspace as a single monorepo for code-style or type-checking purposes when performing edits, analysis, or lint suggestions.

Key scanning rules for Copilot and agents:
- Project detection: determine project type by inspecting the folder for typical files:
  - React client: presence of `index.html`, `src/main.tsx` or `src/index.tsx`, `package.json` scripts referencing `vite` or `react-scripts` or a `tsconfig` under the folder.
  - Node microservice: presence of `nest-cli.json`, `src/main.ts`, `package.json` scripts referencing `nestjs`, or `server`/`app` entry points.
- Scoping: only analyze, type-check, or propose changes inside the target project folder. Do not run workspace-wide linting or apply fixes across unrelated projects.
- File types and policies:
  - Client projects: only enforce React + TypeScript component conventions, accessibility rules, Storybook patterns, and front-end bundling rules. Ignore Node server lint rules (no backend-specific checks).
  - Microservice projects: only enforce Node/TypeScript server conventions (NestJS patterns, DTOs, controllers, services). Ignore React/Storybook/front-end checks.
  - Always exclude `node_modules`, build output folders (`dist`, `build`), and editor/IDE files.
- Type declarations: do not assume types or ambient declarations are installed across projects. If a consumer project is missing types (e.g., `@types/styled-components`), report it as a per-project issue and offer a per-project fix (install devDependency or add package-local declaration), do NOT modify other projects to satisfy one project's diagnostics.

When making changes that affect public APIs (exported types, filenames, barrels), only update projects that explicitly import the modified API. Do not perform global changes across unrelated projects.

If the user asks to run lint/typecheck, confirm which project folder to run it in and only run commands under that project.

## Agents and Skills (Repository-wide)
- Agents are defined in .github/agents/ and can be referenced for specialized work (e.g., accessibility).
- Skills are defined in .github/skills/**/SKILL.md and should be consulted by agents or Copilot when relevant.
- Do NOT create per-project copilot instruction files. Keep all instructions centralized in this root file.
- When an agent is used, it must report the exact files involved (with links) and apply fixes directly when safe and scoped.
- For accessibility work, prioritize WCAG 2.2 Level AAA and follow the Accessibility Agent guidance in .github/agents/accessibility.agent.md.

```

## Coding Guidelines
- Use TypeScript strictly with proper type annotations.
- Avoid `any` types and `as any` casts; prefer precise types, type guards, and small adapters.
- Follow React best practices with functional components and hooks.
- Implement state management with Redux Toolkit and Sagas for complex async flows.
- Use RTK Query for API calls where possible.
- Write clean, modular components with clear separation of concerns.
- Ensure accessibility and responsive design.
- Follow the project's ESLint configuration.

## Code Comments
- Avoid adding inline comments inside source files for implementation guidance. Instead, add guidance, usage patterns, or conventions to the project's Copilot instructions or the repository's SKILL agent files so they can be centrally maintained and applied by tooling.

## Workflow
- You MUST use the todo list tool to plan and track your progress. NEVER skip this step, and START with this step whenever the task is multi-step. Record clear, actionable steps and update statuses as you work so others can follow progress.

## Spec-Driven Development (SDD)
- Prefer Spec-Driven Development for new UI components and critical flows: create a concise spec before implementation that lists props, accessibility requirements, keyboard interactions, visual constraints, example usage, and acceptance tests.
- Place component specs under `.github/specs/` using the provided template (`.github/specs/component-spec-template.md`).
- Implement automated acceptance tests derived from the spec (RTL + axe checks) and add them to CI for PR validation.

## Key Practices
- Use Vite for fast development and building.
- Implement proper error boundaries and loading states.
- Optimize performance with React.memo, useMemo, and useCallback.
- Maintain consistent component structure and naming conventions.
- Prefer Waterworks components for UI consistency.
- Use Apollo Client for GraphQL queries and mutations.
- Implement comprehensive error handling for API calls.
- Write unit tests for components and business logic.

## API Integration Patterns
- Use RTK Query for REST API endpoints with automatic caching and invalidation.
- Use Apollo Client for GraphQL operations with proper error handling.
- Implement optimistic updates for better UX.
- Handle authentication tokens automatically in API clients.
- Use React Final Form for complex form handling and validation.

## State Management
- Use Redux Toolkit for state slices with createSlice.
- Implement async logic with Redux-Saga for complex workflows.
- Use selectors for computed state and memoization.
- Maintain immutable state updates.