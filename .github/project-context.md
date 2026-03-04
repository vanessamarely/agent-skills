# 120Water Project Context (Centralized)

This document centralizes project context to avoid per-project instructions and to reduce prompt overhead. It is the single source of truth for agents and skills.

## How to Use This Document
- Reference this file for project setup, goals, frameworks, and lint/test expectations.
- Do not create per-project copilot instruction files.
- Agents should cite this document in reviews and remediation notes.

## Project Overview by Folder

> **Note:** “Goal” is inferred from folder name and dependencies when README is not present. Validate with each project’s README if needed.

| Folder | Type / Setup | Goal (inferred) | Frameworks & Practices | Lint / Test Scripts |
| --- | --- | --- | --- | --- |



## Accessibility & Type Safety Principles (All Projects)
- Buttons and interactive controls must have discernible text or accessible names (WCAG 2.2 AAA).
- Avoid `any` and prefer `unknown` with type guards.
- Use semantic HTML and ensure keyboard support for interactive elements.

## React Example (Good Practice)
```tsx
import React, { useMemo } from 'react'
import { Button } from '@120wateraudit/waterworks'

interface ActionBarProps {
  isSaving: boolean
  onSave: () => void
}

const ActionBar = ({ isSaving, onSave }: ActionBarProps): JSX.Element => {
  const label = useMemo(() => (isSaving ? 'Saving…' : 'Save'), [isSaving])

  return (
    <Button
      type="button"
      variant="primary"
      disabled={isSaving}
      aria-label={label}
      onClick={onSave}>
      {label}
    </Button>
  )
}

export default ActionBar
```

## Node / NestJS Example (Good Practice)
```ts
import { Body, Controller, Post } from '@nestjs/common'
import { IsEmail, IsString } from 'class-validator'

class CreateUserDto {
  @IsEmail()
  email!: string

  @IsString()
  name!: string
}

@Controller('users')
export class UsersController {
  @Post()
  create(@Body() dto: CreateUserDto): { id: string } {
    return { id: 'new-user-id' }
  }
}
```

## Related Skills & Agents
- Accessibility agent: .github/agents/accessibility.agent.md
- TypeScript & Storybook agent: .github/agents/typescript-storybook.agent.md
- React TypeScript practices: .github/skills/react-typescript-code-practices/SKILL.md
