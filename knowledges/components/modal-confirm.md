# Component Guide: Confirmation Modal (`ModalConfirmComponent`)

`ModalConfirmComponent` is a pre-configured confirmation dialog in Skalfa App, designed to ask users for confirmation before proceeding with critical actions (like deleting a record, logging out, or submitting a transaction).

---

## 1. Component Interface (`ModalConfirmProps`)

```typescript
export interface ModalConfirmProps {
  show        : boolean;             // Controls the visibility of the modal
  onClose     : () => void;          // Callback when the modal is closed or cancelled
  onConfirm   : () => void;          // Callback when the user confirms the action
  title      ?: string;              // Modal title (defaults to "Confirmation")
  message    ?: string;              // Description message (defaults to "Are you sure?")
  confirmText?: string;              // Text on the confirm button (defaults to "Confirm")
  cancelText ?: string;              // Text on the cancel button (defaults to "Cancel")
  variant    ?: "danger" | "warning" | "success" | "primary"; // Theme color of the confirm button
  loading    ?: boolean;             // Disables buttons and shows a spinner on the confirm button
}
```

---

## 2. Key Features

*   **Semantic Colors**: The `variant` prop adjusts the confirm button styling (e.g., `danger` renders a red button for delete actions, `success` renders a green button).
*   **Loading and Guard States**: During an asynchronous operation, set `loading={true}` to prevent double submissions and display a loading spinner.
*   **Focus Guard & Keyboard Accessibility**: Can be closed using the `Escape` key or by clicking on the background backdrop.

---

## 3. Usage Example

```tsx
import { ModalConfirmComponent, ButtonComponent } from "@components";
import { useState } from "react";

export function DeleteUserButton({ userId }: { userId: number }) {
  const [showModal, setShowModal] = useState(false);
  const [deleting, setDeleting] = useState(false);

  const handleDelete = async () => {
    setDeleting(true);
    // Call API to delete user...
    await new Promise((resolve) => setTimeout(resolve, 1000));
    setDeleting(false);
    setShowModal(false);
  };

  return (
    <>
      <ButtonComponent variant="danger" onClick={() => setShowModal(true)}>
        Delete User
      </ButtonComponent>

      <ModalConfirmComponent
        show={showModal}
        onClose={() => setShowModal(false)}
        onConfirm={handleDelete}
        title="Delete User Account"
        message="Are you sure you want to delete this user? This action cannot be undone."
        confirmText="Delete"
        cancelText="Keep User"
        variant="danger"
        loading={deleting}
      />
    </>
  );
}
```
