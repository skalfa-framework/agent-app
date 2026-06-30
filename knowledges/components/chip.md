# Component Guide: Chip (`ChipComponent`)

`ChipComponent` is a compact badge element in Skalfa App. It is commonly used to display statuses, tags, categories, or filters.

---

## 1. Component Interface (`ChipProps`)

```typescript
export interface ChipProps {
  label      : string;                                                   // Text displayed inside the chip
  variant   ?: "primary" | "secondary" | "danger" | "success" | "warning" | "info" | "light";
  outline   ?: boolean;                                                 // Enables outline style
  onRemove  ?: () => void;                                              // If provided, renders a close/remove button
  className ?: string;                                                  // Custom Tailwind CSS classes
}
```

---

## 2. Key Features

*   **Status Indicators**: Color variants represent different semantic meanings (e.g., `success` for completed, `danger` for failed, `warning` for pending).
*   **Removable Chips**: Providing an `onRemove` callback automatically renders a small "x" button on the right side of the chip, enabling tags or active filters.
*   **Design Styles**: Supports both solid background (`default`) and thin bordered (`outline`) styles.

---

## 3. Usage Examples

### A. Status Badges
```tsx
import { ChipComponent } from "@components";

export function OrderStatus({ status }: { status: string }) {
  if (status === "completed") {
    return <ChipComponent label="Completed" variant="success" />;
  }
  if (status === "pending") {
    return <ChipComponent label="Pending" variant="warning" />;
  }
  return <ChipComponent label="Cancelled" variant="danger" />;
}
```

### B. Removable Tags
```tsx
import { ChipComponent } from "@components";
import { useState } from "react";

export function TagFilter() {
  const [tags, setTags] = useState(["React", "TypeScript", "Tailwind"]);

  const handleRemove = (tagToRemove: string) => {
    setTags(tags.filter((t) => t !== tagToRemove));
  };

  return (
    <div className="flex gap-2">
      {tags.map((tag) => (
        <ChipComponent
          key={tag}
          label={tag}
          variant="primary"
          outline
          onRemove={() => handleRemove(tag)}
        />
      ))}
    </div>
  );
}
```
