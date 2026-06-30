# Utility Guide: Form & Table Supervision Hooks

Skalfa App provides specialized React hooks to manage states for form submission and paginated data tables. These hooks are used internally by the supervision components but can also be used for custom implementations.

---

## 1. Form Hook (`useSupervisionForm`)

Manages form values, validation states, and API submission.

```typescript
export function useSupervisionForm(options: {
  initialValues ?: Record<string, any>;
  validation    ?: Record<string, string[]>; // Validation rules
  onSubmit       : (values: Record<string, any>) => Promise<void> | void;
})
```

### Example:
```tsx
import { useSupervisionForm } from '@utils'

export function CustomForm() {
  const { values, errors, loading, setValue, submit } = useSupervisionForm({
    initialValues: { username: "" },
    validation: {
      username: ["required", "min:3"]
    },
    onSubmit: async (data) => {
      // API call
    }
  });

  return (
    <form onSubmit={(e) => { e.preventDefault(); submit(); }}>
      <input 
        value={values.username} 
        onChange={(e) => setValue("username", e.target.value)} 
      />
      {errors.username && <p className="text-red-500">{errors.username[0]}</p>}
      <button type="submit" disabled={loading}>Submit</button>
    </form>
  );
}
```

---

## 2. Table Hook (`useSupervisionTable`)

Manages server-side pagination, sorting, search queries, and data fetching for tables.

```typescript
export function useSupervisionTable(options: {
  url      : string;                   // API endpoint to fetch data from
  params  ?: Record<string, any>;      // Initial query parameters
})
```

### Returned Properties:
*   `data`: Array of records fetched from the API.
*   `total`: Total number of records (for pagination).
*   `loading`: Boolean loading state.
*   `handleParams`: Callback to update query parameters (e.g. page, search, sort).
*   `refresh`: Function to re-trigger the API fetch.

### Example:
See the usage example in [Table Supervision Guide](file:///d:/_skalfa/agent-app/knowledges/components/table-supervision.md#L4-L44).
