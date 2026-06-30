# Component Guide: Table Supervision (`TableSupervisionComponent`)

`TableSupervisionComponent` is a "magic" component in Skalfa App. It automatically generates a fully-featured data table from a JSON schema definition. It handles pagination, search, column sorting, and custom action buttons.

---

## 1. Component Interface (`TableSupervisionProps`)

```typescript
export interface TableSupervisionProps {
  columns     : TableSupervisionColumn[];             // Column definitions
  data        : any[];                                // Data rows
  total      ?: number;                               // Total count (for server-side pagination)
  loading    ?: boolean;                              // Table loading skeleton state
  onParams   ?: (params: Record<string, any>) => void;// Callback for pagination, sort, and search changes
  actions    ?: (row: any) => ReactNode;              // Custom row action buttons renderer
  className  ?: string;
}
```

---

## 2. Column Schema Configuration (`TableSupervisionColumn`)

```typescript
export interface TableSupervisionColumn {
  key       : string;           // Key name in the data row object
  label     : string;           // Header title of the column
  sortable ?: boolean;          // Enables column sorting
  type     ?: "text" | "number" | "date" | "currency" | "chip" | "image" | "custom";
  render   ?: (val: any, row: any) => ReactNode; // Custom cell renderer
}
```

---

## 3. Supported Column Types

*   `text` (Default): Displays value as plain text.
*   `number`: Formats value as a number.
*   `date`: Automatically formats ISO dates using the local date helper (`DD MMM YYYY`).
*   `currency`: Automatically formats numbers as Rupiah (`Rp`).
*   `chip`: Wraps the value in a `ChipComponent` (useful for status fields).
*   `image`: Renders a thumbnail image.
*   `custom`: Relies on the custom `render` function.

---

## 4. Usage Example

```tsx
import { TableSupervisionComponent, ButtonComponent } from "@components";
import { useSupervisionTable } from "@utils";

export function ProductListTable() {
  // Use hook to manage pagination, sorting, and API fetching state
  const { data, total, loading, handleParams, refresh } = useSupervisionTable({
    url: "/api/products"
  });

  const columns = [
    { key: "sku", label: "SKU", sortable: true },
    { key: "name", label: "Product Name", sortable: true },
    { key: "price", label: "Price", type: "currency", sortable: true },
    { key: "status", label: "Status", type: "chip" },
    { key: "created_at", label: "Added Date", type: "date" }
  ];

  const renderActions = (row: any) => (
    <div className="flex gap-2">
      <ButtonComponent size="xs" variant="outline" onClick={() => console.log("Edit:", row.id)}>
        Edit
      </ButtonComponent>
      <ButtonComponent size="xs" variant="danger" onClick={() => console.log("Delete:", row.id)}>
        Delete
      </ButtonComponent>
    </div>
  );

  return (
    <div className="p-6 bg-white rounded-lg border shadow-sm">
      <h2 className="text-lg font-bold mb-4">Inventory Products</h2>
      <TableSupervisionComponent
        columns={columns}
        data={data}
        total={total}
        loading={loading}
        onParams={handleParams}
        actions={renderActions}
      />
    </div>
  );
}
```
