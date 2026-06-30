# Utility Guide: Class Merging (`cn`)

The `cn` utility in Skalfa App is a helper function used to conditionally join CSS class names together. It is built on top of `clsx` and `tailwind-merge` to ensure Tailwind CSS classes are merged without conflicts.

---

## 1. Why Use `cn`?

When writing conditional Tailwind classes or passing custom classes to child components, simple string concatenation can result in duplicate or conflicting classes (e.g., `px-4 px-6`). 

`cn` resolves conflicts by overriding earlier classes with later ones:

```typescript
import { cn } from '@utils'

// Standard Tailwind conflict: px-4 and px-6
const classes = cn("px-4 py-2 bg-primary", "px-6");
// Returns: "py-2 bg-primary px-6" (px-4 is overridden by px-6)
```

---

## 2. Usage Examples

### A. Conditional Classes
Pass objects where keys are class names and values are boolean conditions:
```tsx
import { cn } from '@utils'

export function Alert({ type, message }: { type: "success" | "error"; message: string }) {
  return (
    <div
      className={cn(
        "p-4 rounded-md border",
        {
          "bg-green-50 border-green-200 text-green-800": type === "success",
          "bg-red-50 border-red-200 text-red-800":       type === "error"
        }
      )}
    >
      {message}
    </div>
  );
}
```

### B. Merging Custom Props Classes
Always use `cn` in your custom components to allow external `className` overrides:
```tsx
import { cn } from '@utils'

interface CardProps {
  className ?: string;
  children   : React.ReactNode;
}

export function Card({ className, children }: CardProps) {
  return (
    <div className={cn("p-6 bg-white rounded-lg border shadow-sm", className)}>
      {children}
    </div>
  );
}
```
*If a user renders `<Card className="p-8 shadow-md">`, the padding will be correctly updated to `p-8` and the shadow to `shadow-md`.*
规律.
 Josephson.
