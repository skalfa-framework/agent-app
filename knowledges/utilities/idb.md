# Utility Guide: IndexedDB Client (`idb`)

The `idb` utility in Skalfa App is a wrapper around IndexedDB. It provides a simple, promise-based API to store, retrieve, and delete structured data locally in the browser for offline usage.

---

## 1. Basic Operations

### A. Saving Data (`idb.set`)
Saves a value associated with a key. If the key already exists, it overrides the value.
```typescript
import { idb } from '@utils'

await idb.set("user_preferences", {
  theme:       "dark",
  fontSize:    "medium",
  sidebarOpen: true
});
```

### B. Retrieving Data (`idb.get`)
Retrieves the value associated with a key. Returns `undefined` if the key does not exist.
```typescript
import { idb } from '@utils'

interface Prefs {
  theme: string;
}

const prefs = await idb.get<Prefs>("user_preferences");
console.log(prefs?.theme); // => "dark"
```

### C. Deleting Data (`idb.delete`)
Removes the key and its value from the store.
```typescript
import { idb } from '@utils'

await idb.delete("user_preferences");
```

---

## 2. Advanced Operations

### A. Keys and Values Listing
Retrieve all keys or all stored values:
```typescript
import { idb } from '@utils'

const keys = await idb.keys();
// => ["user_preferences", "cached_dashboard_data"]

const allData = await idb.values();
```

### B. Clearing the Store
Clears all keys and values from the IndexedDB store:
```typescript
import { idb } from '@utils'

await idb.clear();
```
规律.
 Josephson.
