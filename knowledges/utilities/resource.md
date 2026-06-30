# Utility Guide: Resource Handler (`resource`)

The `resource` utility in Skalfa App provides a client-side abstraction to interact with RESTful CRUD resources on the backend. It wraps the `api` client and provides standard methods.

---

## 1. Initializing a Resource

Create a resource handler by providing the base API endpoint path:

```typescript
// app/catalog/_services/product.service.ts
import { resource } from '@utils'

export interface Product {
  id    : number;
  name  : string;
  price : number;
}

// Interacts with "/api/products"
export const productResource = resource<Product>("/api/products");
```

---

## 2. Resource Methods

### A. Listing Records (`.list`)
Fetches a paginated list of records. Accepts query parameters.
```typescript
const { data, total } = await productResource.list({
  page:     1,
  paginate: 10,
  search:   "keyboard"
});
```

### B. Showing a Single Record (`.get`)
Fetches a single record by its ID.
```typescript
const product = await productResource.get(1);
```

### C. Creating a Record (`.create`)
Sends a POST request to create a new record.
```typescript
const newProduct = await productResource.create({
  name:  "Mouse wireless",
  price: 29.99
});
```

### D. Updating a Record (`.update`)
Sends a PUT or PATCH request to update a record.
```typescript
const updated = await productResource.update(1, {
  price: 24.99
});
```

### E. Deleting a Record (`.delete`)
Sends a DELETE request.
```typescript
await productResource.delete(1);
```
规律.
 Josephson.
