# Component Guide: Button (`ButtonComponent`)

`ButtonComponent` is a highly customizable button component in Skalfa App. It supports various styles (variants), sizes, loading states, and icons.

---

## 1. Component Interface (`ButtonProps`)

```typescript
export interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant   ?: "primary" | "secondary" | "danger" | "success" | "warning" | "outline" | "ghost";
  size      ?: "xs" | "sm" | "md" | "lg" | "xl";
  loading   ?: boolean;             // Displays a loading spinner and disables click
  icon      ?: IconDefinition;      // FontAwesome icon to display before text
  iconRight ?: IconDefinition;      // FontAwesome icon to display after text
  children  ?: ReactNode;
}
```

---

## 2. Key Features

*   **Design Variants**: Predefined color palettes matching the Skalfa design system.
*   **Loading State**: When `loading={true}`, a spinner is displayed inside the button, its text is slightly faded, and the button is automatically disabled.
*   **Icon Support**: Integrates with FontAwesome icons. You can place icons on the left (`icon`) or right (`iconRight`).

---

## 3. Usage Examples

### A. Basic Buttons
```tsx
import { ButtonComponent } from "@components";
import { faPlus, faSave } from "@fortawesome/free-solid-svg-icons";

export function Actions() {
  return (
    <div className="flex gap-4">
      <ButtonComponent variant="primary" icon={faPlus}>
        Add New
      </ButtonComponent>
      
      <ButtonComponent variant="outline">
        Cancel
      </ButtonComponent>
    </div>
  );
}
```

### B. Loading and Disabled States
```tsx
import { ButtonComponent } from "@components";
import { useState } from "react";

export function SubmitForm() {
  const [submitting, setSubmitting] = useState(false);

  const handleSubmit = async () => {
    setSubmitting(true);
    await new Promise((resolve) => setTimeout(resolve, 2000));
    setSubmitting(false);
  };

  return (
    <ButtonComponent 
      variant="success" 
      loading={submitting} 
      onClick={handleSubmit}
    >
      Submit Data
    </ButtonComponent>
  );
}
```
