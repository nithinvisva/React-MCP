# 🔧 Component Rules for MCP

This document defines best practices for building and maintaining components in your Monorepo Component Platform (MCP) using Windsurf, React, TypeScript, and styled-components.

---

## 📐 Component Structure Philosophy

All components must follow the **Atomic Design Pattern**:

| Layer     | Description                             | Example Components        |
| --------- | --------------------------------------- | ------------------------- |
| Atoms     | Small, reusable visual elements         | `Button`, `Input`, `Text` |
| Molecules | Group of atoms forming functional units | `InputWithLabel`, `Card`  |
| Organisms | Complex, self-contained UI blocks       | `LoginForm`, `Navbar`     |

📁 Components must be stored under their respective folders:

```
/packages/components/atoms/Button/
/packages/components/molecules/InputWithLabel/
/packages/components/organisms/LoginForm/
```

---

## 📦 Component Folder Layout

Each component lives in its own folder with the following structure:

```
<Button>
  Button.tsx            // Main component
  Button.test.tsx       // Unit test
  Button.stories.tsx    // Storybook stories
  index.ts              // Barrel export
```

Every file serves a purpose:

- `Component.tsx`: React + styled-components definition
- `Component.test.tsx`: Behavior and rendering tests using RTL
- `Component.stories.tsx`: Storybook integration
- `index.ts`: Named exports only

---

## ✍️ Props API Standards

- All props must be typed using `type`, not `interface`
- Use common naming patterns: `variant`, `size`, `disabled`, `onClick`, `children`
- Always destructure props in the function signature
- Use union types for enums (e.g., `variant: 'primary' | 'secondary'`)
- Optional props should use `?`, not `= undefined`

```ts
export type ButtonProps = {
  children: React.ReactNode;
  variant?: 'primary' | 'secondary';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  onClick?: () => void;
};
```

```tsx
export const Button = ({ children, variant = 'primary' }: ButtonProps) => {
  return <StyledButton variant={variant}>{children}</StyledButton>;
};
```

---

## 🎨 Styling Rules

- All styling must be done using `styled-components`
- Use only values from the theme (no magic values)
- Responsive styles must use `theme.breakpoints`

```tsx
const StyledButton = styled.button`
  padding: ${({ theme }) => theme.spacing.md};
  background-color: ${({ theme }) => theme.colors.primary};
  @media (min-width: ${({ theme }) => theme.breakpoints.md}) {
    font-size: ${({ theme }) => theme.fontSizes.lg};
  }
`;
```

---

## 🧼 Logic Placement Guidelines

- No business logic inside atoms/molecules
- Hooks must be declared in `/packages/hooks`
- Complex conditional rendering should live in organisms or page components

---

## 🧠 Reusability Best Practices

| Guideline                        | ✅ Required |
| -------------------------------- | ---------- |
| Accepts generic `children` prop  | Yes        |
| Theme-based styling only         | Yes        |
| No hardcoded strings or labels   | Yes        |
| Flexible via props, not context  | Yes        |
| Self-contained (no global state) | Yes        |

---

## 🧪 Testing Rules

- Test file must be co-located
- Use `@testing-library/react` and `jest`
- All props, states, and interactions must be testable

```tsx
test('renders button with text', () => {
  render(<Button>Click Me</Button>);
  expect(screen.getByText('Click Me')).toBeInTheDocument();
});
```

---

## 📖 Storybook Requirements

- Every public component must include a `.stories.tsx` file
- At minimum, provide:
  - `Primary` story
  - `Variants` story (if applicable)
  - `Playground` story with `args`

```tsx
export const Primary = {
  args: {
    children: 'Save',
    variant: 'primary',
  }
};
```

---

## 🚫 Do's and Don'ts

### ✅ Do

- Use named exports
- Keep components under 150 lines
- Prefer composition over config
- Document all prop types

### ❌ Don’t

- Use default exports
- Use inline styles
- Mix visual logic with business logic
- Depend on global styles

---

## ✅ Summary Checklist

-

