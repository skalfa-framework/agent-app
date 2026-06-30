# Utility Guide: Authentication Context (`auth`)

The `auth` context in Skalfa App manages the user's session state, login/logout flows, and permission guarding on the client-side.

---

## 1. Accessing Auth State (`useAuth`)

Use the `useAuth` hook to access the authenticated user object and session actions:

```tsx
import { useAuth } from '@utils'

export function DashboardHeader() {
  const { user, logout } = useAuth();

  return (
    <div className="flex justify-between items-center p-4 border-b">
      <span>Welcome, <strong>{user?.name}</strong>!</span>
      <button onClick={logout} className="text-sm text-red-500">Log Out</button>
    </div>
  );
}
```

---

## 2. Guarding UI Elements by Permission

You can conditionally render UI elements based on whether the logged-in user possesses specific permission keys:

```tsx
import { useAuth } from '@utils'

export function ProductActions({ product }: { product: any }) {
  const { hasPermission } = useAuth();

  return (
    <div className="flex gap-2">
      {/* Always visible */}
      <button>View Detail</button>

      {/* Only visible to users with "300.03" (Update Product) permission */}
      {hasPermission("300.03") && (
        <button className="text-yellow-500">Edit Product</button>
      )}

      {/* Only visible to users with "300.04" (Delete Product) permission */}
      {hasPermission("300.04") && (
        <button className="text-red-500">Delete</button>
      )}
    </div>
  );
}
```

---

## 3. Route Guarding

Page routes are guarded by wrapping them in the `Guard` component or using layout level checks:

```tsx
import { Guard } from '@components'
import { BookingListStructure } from './_structures/booking-list.structure'

export default function BookingsPage() {
  return (
    // Only allows users with permission "400.01" (View Bookings) to access this page
    <Guard permission="400.01">
      <BookingListStructure />
    </Guard>
  );
}
```
