# 🧪 Testing Guidelines for MCP

Establish consistent and useful tests for all MCP components using Jest and Testing Library.

---

## ✅ Test Types to Include

| Test Type         | Applies To             | Required? |
|-------------------|-------------------------|------------|
| Render Test       | All components          | ✅          |
| Interaction Test  | Inputs, Buttons, Forms  | ✅          |
| Snapshot Test     | Static presentational   | ⚠️ Only if necessary |
| Unit Test         | Hooks, Utils            | ✅          |

---

## 🧬 Test File Structure

Each component must have a test file next to its implementation:

```
/Button
  Button.tsx
  Button.test.tsx
```

---

## 🧪 Test File Naming

```
ComponentName.test.tsx
```

Hooks:
```
useMyHook.test.ts
```

Utils:
```
formatDate.test.ts
```

---

## 🧰 Testing Tools

| Tool               | Usage                            |
|--------------------|-----------------------------------|
| Jest               | Unit + Component testing         |
| React Testing Library | User-focused DOM testing     |
| jest-dom           | Extra matchers for assertions    |

---

## 🧼 Testing Conventions

### Render Tests
```tsx
test('renders button with text', () => {
  render(<Button>Click Me</Button>);
  expect(screen.getByText('Click Me')).toBeInTheDocument();
});
```

### Interaction Tests
```tsx
test('calls onClick when clicked', () => {
  const handleClick = jest.fn();
  render(<Button onClick={handleClick}>Click</Button>);
  fireEvent.click(screen.getByText('Click'));
  expect(handleClick).toHaveBeenCalled();
});
```

---

## 🚫 What NOT to Do

- ❌ Don't test implementation details (e.g., `className`, internal state)
- ❌ Don’t snapshot dynamic components
- ❌ Don’t skip tests in PRs (`it.skip`, `test.skip`)

---

## ✅ Good Practices

- Write tests as documentation — name them clearly
- One `describe` block per component
- Reuse `render` functions for DRY tests
- Test edge cases (null props, long text, etc.)

---

## 📚 Example

```tsx
// Button.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './Button';

describe('Button', () => {
  it('renders correctly', () => {
    render(<Button>Save</Button>);
    expect(screen.getByText('Save')).toBeInTheDocument();
  });

  it('handles onClick', () => {
    const onClick = jest.fn();
    render(<Button onClick={onClick}>Save</Button>);
    fireEvent.click(screen.getByText('Save'));
    expect(onClick).toHaveBeenCalled();
  });
});
```

---

## ✅ Summary Checklist

- [x] Render test for every component
- [x] Interaction test for interactivity
- [x] Co-located `.test.tsx` files
- [x] Avoid snapshot spam
- [x] Don’t skip tests or commit failing ones
- [x] Coverage must meet team threshold (e.g. 80%+)

