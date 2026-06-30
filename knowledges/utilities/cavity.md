# Utility Guide: Cavity State Manager (`cavity`)

`cavity` is the central global state manager in Skalfa App. It is a lightweight reactive store designed to share state across non-parent-child components without the boilerplate of Redux or Context.

---

## 1. Registering State Keys

All global state keys and their initial values must be defined in the cavity store:

```typescript
// store/index.ts
import { cavity } from '@utils'

// Initialize state
cavity.init({
  theme:       "light",
  sidebarOpen: true,
  cartCount:   0
});
```

---

## 2. Accessing State in React Components (`useCavity`)

Use the `useCavity` hook to read values. The component will automatically re-render when the selected state key changes.

```tsx
import { useCavity } from '@utils'

export function Sidebar() {
  // Reads sidebarOpen and re-renders when it changes
  const [isOpen, setIsOpen] = useCavity<boolean>("sidebarOpen");

  return (
    <div className={`fixed top-0 left-0 h-full bg-white transition-all ${isOpen ? "w-64" : "w-0"}`}>
      <button onClick={() => setIsOpen(false)}>Close Sidebar</button>
      {/* Sidebar content */}
    </div>
  );
}
```

---

## 3. Updating State Outside React Components

You can read or update state values directly from plain JavaScript/TypeScript files (such as services, helpers, or API interceptors) without using hooks:

```typescript
// app/auth/_services/auth.service.ts
import { cavity } from '@utils'

export class AuthService {
  static async handleLoginSuccess(user: any) {
    // Set global user state
    cavity.set("user", user);
    
    // Read theme state
    const currentTheme = cavity.get("theme");
    console.log("Current theme is:", currentTheme);
  }
}
```
*Note: `cavity.set()` triggers reactive updates to all components subscribing to that key via `useCavity()`.*
