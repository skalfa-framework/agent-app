# Component Guide: Card (`CardComponent`)

`CardComponent` is a structural container in Skalfa App. It is used to group related content, forms, or data displays into a clean, bordered box with consistent padding and styling.

---

## 1. Component Interface (`CardProps`)

```typescript
export interface CardProps {
  title       ?: ReactNode;          // Header title (can be string or JSX)
  extra       ?: ReactNode;          // Extra actions displayed in the header right corner
  footer      ?: ReactNode;          // Footer content
  className   ?: string;             // Custom Tailwind classes for the wrapper
  bodyClass   ?: string;             // Custom Tailwind classes for the body container
  children     : ReactNode;          // Card body content
}
```

---

## 2. Key Features

*   **Header and Extra Actions**: Includes a header section with a title and an optional `extra` slot on the right (ideal for action buttons, links, or dropdowns).
*   **Consistent Spacing**: Pre-configured paddings and borders that align with the application's overall design language.
*   **Footer Slot**: An optional footer section separated by a subtle border, commonly used for form submit buttons or pagination controls.

---

## 3. Usage Example

```tsx
import { CardComponent, ButtonComponent } from "@components";
import { faEdit } from "@fortawesome/free-solid-svg-icons";

export function UserProfileCard() {
  return (
    <CardComponent
      title="User Profile"
      extra={
        <ButtonComponent variant="ghost" size="sm" icon={faEdit}>
          Edit
        </ButtonComponent>
      }
      footer={
        <div className="flex justify-end gap-2">
          <span className="text-xs text-light-foreground">Last updated: 2 hours ago</span>
        </div>
      }
    >
      <div className="space-y-2">
        <p><strong>Name:</strong> John Doe</p>
        <p><strong>Email:</strong> john.doe@example.com</p>
        <p><strong>Role:</strong> Administrator</p>
      </div>
    </CardComponent>
  );
}
```
