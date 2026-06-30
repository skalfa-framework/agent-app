# Component Guide: Breadcrumb (`BreadcrumbComponent`)

`BreadcrumbComponent` is a navigation helper in Skalfa App. It displays the user's current location within the application's hierarchical structure, allowing them to navigate back up easily.

---

## 1. Component Interface (`BreadcrumbProps`)

```typescript
export interface BreadcrumbProps {
  items: {
    label : string;          // Text displayed for the breadcrumb item
    path ?: string;          // Link path (navigates to this path when clicked)
  }[];
  className ?: string;       // Custom Tailwind CSS classes
}
```

---

## 2. Key Features

*   **Hierarchical Paths**: Dynamically renders links separated by a chevron right icon (`faChevronRight`).
*   **Active State Styling**: The last item in the breadcrumb list (the current page) is automatically styled as active (typically bolder or highlighted in primary color) and is not clickable.
*   **Next.js Link Integration**: Clickable items use Next.js `Link` under the hood to ensure single-page application (SPA) navigation without page reloads.

---

## 3. Usage Example

```tsx
import { BreadcrumbComponent } from "@components";

export function BookingDetailPage() {
  const breadcrumbItems = [
    { label: "Home", path: "/dashboard" },
    { label: "Bookings", path: "/bookings" },
    { label: "Detail Booking #1024" } // Active item (no path)
  ];

  return (
    <div className="p-4">
      <BreadcrumbComponent items={breadcrumbItems} />
      <h1 className="text-2xl font-bold mt-2">Booking #1024</h1>
    </div>
  );
}
```
