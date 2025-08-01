# 📦 Storybook Guidelines for MCP

Storybook is the primary tool to document, develop, and test components in isolation. These rules ensure clarity, usability, and consistency across your component library.

---

## 🗂 Where Stories Live

Each component must include a `.stories.tsx` file located next to the component:

```
/Button
  Button.tsx
  Button.stories.tsx
```

---

## 📖 Naming the Stories

```tsx
// Button.stories.tsx
import { Button } from './Button';
import type { Meta, StoryObj } from '@storybook/react';

const meta: Meta<typeof Button> = {
  title: 'Atoms/Button',
  component: Button,
  tags: ['autodocs'],
  parameters: {
    docs: {
      description: {
        component: 'Primary action button used across the UI.'
      }
    }
  }
};

export default meta;
```

### Best Practice
- Follow folder path in `title`: `Atoms/Button`, `Organisms/LoginForm`
- Add a short `description` under `docs.parameters`

---

## ✅ Required Stories

Each component must include at least the following:

| Story Type     | Example Name | Description                         |
|----------------|--------------|-------------------------------------|
| Default        | `Primary`    | The default or primary variant      |
| Variants       | `Secondary`, `Disabled`, etc. | Different UI props      |
| Playground     | `Playground` | With `args` to test props live      |

### Example
```tsx
export const Primary: StoryObj<typeof Button> = {
  args: {
    children: 'Click Me',
    variant: 'primary'
  }
};

export const Playground: StoryObj<typeof Button> = {
  args: {
    children: 'Playground!',
    variant: 'secondary',
    disabled: false
  }
};
```

---

## 🎨 Args and Controls

- Use `args` to define prop inputs.
- Storybook will auto-generate Controls.

```tsx
Button.args = {
  children: 'Save',
  disabled: false,
  variant: 'primary'
};
```

---

## 🛠 Addons & Plugins

Enable and use:
- `@storybook/addon-docs`
- `@storybook/addon-controls`
- `@storybook/addon-a11y`
- `storybook-dark-mode` (optional for theming)

---

## 🧪 Storybook for Testing

- Visual regression tests can be run using Chromatic
- Each story = snapshot-like visual unit

---

## ✅ Do's and Don'ts

### ✅ Do
- Add `docs.description.component`
- Include all variants
- Match title path to atomic structure
- Use named exports for clarity

### ❌ Don’t
- Put multiple components in one story file
- Leave out `args` for interactive controls
- Skip stories for important components

---

## ✅ Summary Checklist

- [x] Storybook file for every public component
- [x] Component title matches folder structure
- [x] Variants and Playground included
- [x] Description provided via `docs.description`
- [x] Uses `args` for controls
- [x] Only one component per file

