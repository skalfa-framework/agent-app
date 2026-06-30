# Component Guide: Data Table (`TableComponent`)

`TableComponent` is a primitive table component in Skalfa App designed to render tabular data cleanly. It supports custom column headers, row selection, sorting, and pagination.

---

## 1. Component Interface (`TableProps`)

```typescript
export interface TableProps {
  columns     : { key: string; label: string; sortable?: boolean }[];
  data        : any[];
  loading    ?: boolean;
  onSort     ?: (key: string, direction: "asc" | "desc") => void;
  onSelect   ?: (selectedIds: any[]) => void; // Enables checkbox selection
  actions    ?: (row: any) => ReactNode;      // Action buttons in the last column
  className  ?: string;
}
```

---

## 2. Key Features

*   **Row Selection**: Providing the `onSelect` callback automatically adds a checkbox column on the left side of the table, including a "Select All" checkbox in the header.
*   **Sorting States**: Sortable columns display interactive caret icons indicating `asc` (ascending), `desc` (descending), or `none` (unsorted) states.
*   **Loading Skeleton**: When `loading={true}`, the table displays a beautiful animated placeholder skeleton instead of empty rows.

---

## 3. Usage Example

```tsx
import { TableComponent } from "@components";
import { useState } from "react";

export function BasicUserTable() {
  const [sort, setSort] = useState({ key: "name", dir: "asc" });

  const columns = [
    { key: "name", label: "Name", sortable: true },
    { key: "email", label: "Email" },
    { key: "role", label: "Role" }
  ];

  const data = [
    { id: 1, name: "Alice", email: "alice@example.com", role: "Admin" },
    { id: 2, name: "Bob", email: "bob@example.com", role: "Member" }
  ];

  const handleSort = (key: string, direction: "asc" | "desc") => {
    setSort({ key, dir: direction });
    console.log(`Sorting by ${key} ${direction}`);
  };

  return (
    <TableComponent
      columns={columns}
      data={data}
      onSort={handleSort}
      actions={(row) => (
        <button className="text-blue-500 hover:underline" onClick={() => alert(`View ${row.name}`)}>
          View
        </button>
      )}
    />
  );
}
```
