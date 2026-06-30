# Utility Guide: API Client (`api`)

The `api` utility in Skalfa App is a dynamic REST API client built on top of `fetch`. It supports automatic JSON parsing, request interceptors (for adding authorization headers), and error handling.

---

## 1. Basic Requests

The `api` client provides shorthand methods for standard HTTP verbs:

```typescript
import { api } from '@utils'

// GET request
const products = await api.get("/api/products");

// POST request with body payload
const newProduct = await api.post("/api/products", {
  name:  "Keyboard Mech",
  price: 89.99
});

// PUT request
await api.put("/api/products/1", { price: 79.99 });

// DELETE request
await api.delete("/api/products/1");
```

---

## 2. Query Parameters

You can pass query parameters as an object in the second argument for `get` requests (or via options):

```typescript
import { api } from '@utils'

const activeUsers = await api.get("/api/users", {
  params: {
    status:   "active",
    paginate: 10,
    page:     1
  }
});
// Automatically requests: /api/users?status=active&paginate=10&page=1
```

---

## 3. Error Handling

The `api` client throws an error if the response status code is not in the 2xx range. The thrown error object contains the status and the parsed error response body:

```typescript
import { api } from '@utils'

try {
  await api.post("/api/users", { email: "invalid-email" });
} catch (error: any) {
  console.error("API Error Status:", error.status); // e.g., 422
  console.error("Error Message:", error.message);    // e.g., "Validation failed"
  console.error("Validation Details:", error.errors); // { email: ["Must be a valid email"] }
}
```

---

## 4. Hook Integration (`useApi`)

For React components, prefer using the `useApi` hook. It manages loading and error states automatically:

```tsx
import { useApi } from '@utils'
import { Spinner } from '@components'

export function ProductList() {
  // Automatically fetches data on mount
  const { data, loading, error, refresh } = useApi<any[]>("/api/products");

  if (loading) return <Spinner />;
  if (error) return <p className="text-danger">Failed to load: {error.message}</p>;

  return (
    <div>
      <button onClick={refresh}>Refresh Data</button>
      <ul>
        {data?.map(p => <li key={p.id}>{p.name} - ${p.price}</li>)}
      </ul>
    </div>
  );
}
```
