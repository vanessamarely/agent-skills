---
name: react-typescript-code-practices
description: Ensures React TypeScript code follows best practices for Sample Manager Client, including TanStack React Query, React Final Form, multiple component libraries, and styled-components.
---

# React TypeScript Code Practices - Sample Manager Client

This skill helps maintain high-quality React TypeScript code in the Sample Manager Client project, focusing on TanStack React Query for server state management, React Final Form for complex forms, multiple component libraries (Waterworks, PrimeReact, Semantic UI), styled-components for styling, and FullCalendar integration.

## When to Use This Skill

- Reviewing new or modified React components using multiple UI libraries
- Implementing TanStack React Query for server state management
- Creating forms with React Final Form validation
- Working with styled-components for component styling
- Integrating FullCalendar for scheduling components
- Ensuring consistency across different component libraries

## Best Practices Guidelines

### 1. Component Structure
- Use functional components with hooks instead of class components
- Choose appropriate component library based on use case:
  - Waterworks for consistent, accessible UI components
  - PrimeReact for rich component sets
  - Semantic UI for semantic HTML components
- Use proper TypeScript interfaces for props across all libraries
- Implement default props using default parameters

### 2. TypeScript Integration
- Define strict types for all props, server state, and form data
- Use `interface` for component props and API response types
- Use `type` for form schemas and query result unions
- Avoid `any` types; use proper type definitions from all libraries
- Leverage utility types like `Partial`, `Pick`, `Omit` for flexible data handling

### 3. TanStack React Query State Management
- Use React Query for all server state management
- Implement proper query keys for caching and invalidation
- Use mutations for data modifications with optimistic updates
- Handle loading, error, and success states appropriately
- Use query invalidation for data consistency

### 4. Form Handling with React Final Form
- Use React Final Form for complex forms with validation
- Implement field-level and form-level validation
- Use form subscriptions for dynamic form behavior
- Handle form state with proper TypeScript typing
- Integrate with component libraries for consistent form UI

### 5. Component Library Integration
- **Waterworks**: Use for primary UI components (Button, Input, Modal) for consistency
- **PrimeReact**: Use for advanced components (DataTable, Calendar, etc.)
- **Semantic UI**: Use for semantic HTML components and layouts
- Ensure consistent styling and behavior across libraries
- Wrap third-party components with proper TypeScript interfaces

### 6. Styled Components
- Use styled-components for component-specific styling
- Create reusable styled components with TypeScript
- Use theme props for consistent design tokens
- Avoid inline styles; prefer styled-components
- Implement responsive design patterns

### 7. FullCalendar Integration
- Use FullCalendar for scheduling and calendar components
- Implement proper event handling with TypeScript
- Customize calendar appearance with styled-components
- Handle calendar state with React Query for data persistence

### 8. Hooks Usage
- Use built-in hooks (useState, useEffect, useCallback, useMemo) appropriately
- Leverage TanStack Query hooks: `useQuery`, `useMutation`, `useQueryClient`
- Use React Final Form hooks: `useForm`, `useField`
- Implement custom hooks for reusable logic
- Follow rules of hooks strictly

### 9. Performance Optimizations
- Use React.memo for components that re-render frequently
- Optimize React Query cache settings for performance
- Use lazy loading for code splitting with Vite
- Implement proper loading states to prevent layout shifts
- Use styled-components optimization techniques

### 10. Error Handling
- Implement error boundaries for component trees
- Handle React Query errors with proper user feedback
- Use form validation errors from React Final Form
- Provide user-friendly error messages across all libraries

### 11. Testing
- Write unit tests for components, hooks, and queries
- Use React Testing Library for component testing
- Mock React Query and API calls appropriately
- Test form validation and submission flows
- Test component library integrations

## Code Examples

### TanStack React Query with TypeScript
```tsx
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { api } from '../services/api';

interface Sample {
  id: string;
  name: string;
  status: 'pending' | 'completed' | 'failed';
  createdAt: string;
}

interface CreateSampleRequest {
  name: string;
  type: string;
}

export const useSamples = (status?: Sample['status']) => {
  return useQuery({
    queryKey: ['samples', status],
    queryFn: async () => {
      const response = await api.get<Sample[]>('/samples', {
        params: status ? { status } : undefined,
      });
      return response.data;
    },
    staleTime: 5 * 60 * 1000, // 5 minutes
  });
};

export const useCreateSample = () => {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: async (data: CreateSampleRequest) => {
      const response = await api.post<Sample>('/samples', data);
      return response.data;
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['samples'] });
    },
    onError: (error) => {
      console.error('Failed to create sample:', error);
    },
  });
};
```

### React Final Form with Component Libraries
```tsx
import React from 'react';
import { Form, Field } from 'react-final-form';
import { Button, Input, Card } from '@120wateraudit/waterworks';
import { Dropdown } from 'primereact/dropdown';
import styled from 'styled-components';

interface SampleFormData {
  name: string;
  type: string;
  priority: 'low' | 'medium' | 'high';
}

interface SampleFormProps {
  onSubmit: (data: SampleFormData) => void;
  initialValues?: Partial<SampleFormData>;
}

const StyledCard = styled(Card)`
  max-width: 500px;
  margin: 0 auto;
`;

const FormRow = styled.div`
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-bottom: 1rem;
`;

const SampleForm: React.FC<SampleFormProps> = ({ onSubmit, initialValues }) => {
  const validate = (values: SampleFormData) => {
    const errors: Partial<SampleFormData> = {};
    if (!values.name) errors.name = 'Name is required';
    if (!values.type) errors.type = 'Type is required';
    return errors;
  };

  return (
    <Form
      onSubmit={onSubmit}
      validate={validate}
      initialValues={initialValues}
      render={({ handleSubmit, submitting, pristine }) => (
        <StyledCard title="Create Sample">
          <form onSubmit={handleSubmit}>
            <FormRow>
              <Field name="name">
                {({ input, meta }) => (
                  <Input
                    {...input}
                    label="Sample Name"
                    error={meta.touched && meta.error}
                    helperText={meta.touched && meta.error}
                  />
                )}
              </Field>

              <Field name="type">
                {({ input, meta }) => (
                  <div>
                    <label htmlFor="type">Sample Type</label>
                    <Dropdown
                      {...input}
                      id="type"
                      options={[
                        { label: 'Water', value: 'water' },
                        { label: 'Soil', value: 'soil' },
                        { label: 'Air', value: 'air' },
                      ]}
                      placeholder="Select type"
                      className={meta.touched && meta.error ? 'p-invalid' : ''}
                    />
                    {meta.touched && meta.error && (
                      <small className="p-error">{meta.error}</small>
                    )}
                  </div>
                )}
              </Field>

              <Field name="priority" initialValue="medium">
                {({ input }) => (
                  <div>
                    <label>Priority</label>
                    <div>
                      {['low', 'medium', 'high'].map((priority) => (
                        <label key={priority}>
                          <input
                            {...input}
                            type="radio"
                            value={priority}
                            checked={input.value === priority}
                          />
                          {priority.charAt(0).toUpperCase() + priority.slice(1)}
                        </label>
                      ))}
                    </div>
                  </div>
                )}
              </Field>
            </FormRow>

            <Button
              type="submit"
              disabled={submitting || pristine}
              variant="primary"
            >
              {submitting ? 'Creating...' : 'Create Sample'}
            </Button>
          </form>
        </StyledCard>
      )}
    />
  );
};
```

### Styled Components with Theme
```tsx
import styled from 'styled-components';

interface Theme {
  colors: {
    primary: string;
    secondary: string;
    background: string;
    text: string;
  };
  spacing: {
    small: string;
    medium: string;
    large: string;
  };
}

const StyledButton = styled.button<{ variant?: 'primary' | 'secondary' }>`
  background-color: ${({ theme, variant = 'primary' }) =>
    variant === 'primary' ? theme.colors.primary : theme.colors.secondary};
  color: ${({ theme }) => theme.colors.text};
  border: none;
  padding: ${({ theme }) => theme.spacing.medium};
  border-radius: 4px;
  cursor: pointer;

  &:hover {
    opacity: 0.9;
  }

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
`;

const CardContainer = styled.div`
  background: ${({ theme }) => theme.colors.background};
  border-radius: 8px;
  padding: ${({ theme }) => theme.spacing.large};
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
`;

interface SampleCardProps {
  title: string;
  children: React.ReactNode;
}

const SampleCard: React.FC<SampleCardProps> = ({ title, children }) => (
  <CardContainer>
    <h3>{title}</h3>
    {children}
  </CardContainer>
);
```

## Validation Steps

1. Check TypeScript compilation errors
2. Run ESLint with project rules
3. Verify TanStack React Query cache behavior
4. Test form validation with React Final Form
5. Check styled-components rendering
6. Test component library integrations
7. Review performance with React DevTools

This skill ensures your Sample Manager Client code follows project-specific patterns for TanStack React Query, multiple component libraries, and styled-components usage.