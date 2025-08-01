# 📁 MCP Folder Structure Rules (with Examples)

## 🧱 Top-Level Monorepo Structure

```
/apps                     → Consumer applications
/packages                 → Reusable libraries (UI, logic, config)
```

### ✅ Example:

```
mcp/
  apps/
    main-app/             → Your actual frontend app
    storybook/            → Storybook instance to preview components
  packages/
    components/           → Atomic design components (Atoms → Molecules → Organisms)
    pages/                → Composed reusable pages (LoginPage, DashboardPage)
    hooks/                → Shared logic (useDebounce, useAuth)
    utils/                → Pure functions (formatDate, calculateAge)
    tokens/               → Design tokens (colors, spacing, fontSizes)
    config/               → Linting, TS config, etc.
    types/                → Shared TypeScript types/interfaces
```

---

## 🧬 `/packages/components` Structure

Organized using **Atomic Design Pattern**:

```
/components
  /atoms/
    /Button/
      Button.tsx
      Button.stories.tsx
      Button.test.tsx
      index.ts
  /molecules/
    /InputWithLabel/
      InputWithLabel.tsx
      InputWithLabel.test.tsx
      InputWithLabel.stories.tsx
      index.ts
  /organisms/
    /LoginForm/
      LoginForm.tsx
      LoginForm.test.tsx
      LoginForm.stories.tsx
      index.ts
```

### ✅ Example:

```tsx
// packages/components/atoms/Button/Button.tsx
import styled from 'styled-components';

const StyledButton = styled.button`
  padding: ${({ theme }) => theme.spacing.md};
`;

export const Button = ({ children }) => {
  return <StyledButton>{children}</StyledButton>;
};
```

---

## 🧩 `/packages/pages` Structure

Each folder in `pages/` should be a reusable, standalone **composed view** using organisms:

```
/pages
  /LoginPage/
    LoginPage.tsx
    LoginPage.test.tsx
    LoginPage.stories.tsx
    index.ts
  /DashboardPage/
```

> 💡 These are **not route files**. They are **composable, themed page-level components** used inside actual routes in `apps/main-app/pages`.

### ✅ Example:

```tsx
// packages/pages/LoginPage/LoginPage.tsx
import { LoginForm } from '@/components/organisms';

export const LoginPage = () => (
  <div>
    <LoginForm />
  </div>
);
```

---

## ⚓ `/packages/hooks`, `/packages/utils`

**Shared logic and utilities**, with unit tests.

```
/hooks
  useDebounce.ts
  useClickOutside.ts
/hooks/__tests__/
  useDebounce.test.ts

/utils
  formatDate.ts
  validateEmail.ts
```

### ✅ Example:

```ts
// packages/utils/formatDate.ts
export const formatDate = (date: Date) => date.toLocaleDateString();
```

---

## 🎨 `/packages/tokens`

Design tokens to support **theming and design consistency**.

```
/tokens
  colors.ts
  spacing.ts
  fontSizes.ts
  borderRadius.ts
  breakpoints.ts
```

### ✅ Example:

```ts
// tokens/colors.ts
export const colors = {
  primary: '#0070f3',
  gray100: '#f7fafc',
};
```

---

## 🧾 `/packages/config`

Shared configuration packages:

```
/config
  eslint-config-custom/
    index.js
  prettier-config-custom/
    index.js
  tsconfig/
    base.json
```

> Used in `tsconfig.json`, `.eslintrc.js`, etc. across all packages.

---

## 🧠 `/apps/main-app`

This is your actual app (e.g., user-facing frontend). It **consumes components, pages, tokens**, etc.

```
/main-app
  /pages
    index.tsx             → Renders <HomePage />
    login.tsx             → Renders <LoginPage />
  /styles
  /public
  package.json
```

### ✅ Example:

```tsx
// apps/main-app/pages/login.tsx
import { LoginPage } from '@/pages';

export default LoginPage;
```

---

## 🔄 Import Rules Example

✅ Good:

```ts
import { Button } from '@/components/atoms';
import { LoginPage } from '@/pages';
import { useAuth } from '@/hooks';
```

❌ Bad:

```ts
import Button from '../../../packages/components/atoms/Button/Button';
```

---

## ✅ Summary Rules

| Layer     | Folder                  | Examples                     |
| --------- | ----------------------- | ---------------------------- |
| Atoms     | `/components/atoms`     | `Button`, `Text`, `Input`    |
| Molecules | `/components/molecules` | `InputWithLabel`, `CardList` |
| Organisms | `/components/organisms` | `LoginForm`, `Header`        |
| Pages     | `/pages`                | `LoginPage`, `DashboardPage` |
| Hooks     | `/hooks`                | `useDebounce`, `useAuth`     |
| Utils     | `/utils`                | `formatDate`, `getInitials`  |
| Tokens    | `/tokens`               | `colors.ts`, `spacing.ts`    |
| App Pages | `/apps/main-app/pages`  | `index.tsx`, `login.tsx`     |

