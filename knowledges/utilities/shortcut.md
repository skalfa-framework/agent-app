# Utility Guide: Keyboard Shortcuts (`shortcut`)

The `shortcut` utility in Skalfa App is used to register and manage global keyboard shortcuts in the browser.

---

## 1. Registering Shortcuts

Use the `shortcut.register` method to bind a key combination to a callback function.

```typescript
import { shortcut } from '@utils'

// Register Ctrl+S (or Cmd+S on Mac) to save
const unbind = shortcut.register("mod+s", (event) => {
  event.preventDefault();
  console.log("💾 Saving data...");
});

// To unbind/remove the shortcut later:
// unbind();
```

---

## 2. Key Combinations

*   `mod`: Maps to `Command` on macOS and `Control` on Windows/Linux.
*   `ctrl`, `shift`, `alt`: Modifier keys.
*   Keys: `a` - `z`, `0` - `9`, `f1` - `f12`, `enter`, `escape`, `space`, `backspace`, `arrowup`, `arrowdown`.

### Examples:
*   `ctrl+shift+p`: Open palette.
*   `escape`: Close active modal.
*   `alt+arrowup`: Move item up.

---

## 3. React Hook Integration (`useShortcut`)

For React components, it is recommended to use the `useShortcut` hook. It automatically handles cleanup (unbinding the shortcut) when the component unmounts:

```tsx
import { useShortcut } from '@utils'
import { useState } from "react";

export function SearchModal({ onClose }: { onClose: () => void }) {
  // Pressing Escape closes the modal
  useShortcut("escape", () => {
    onClose();
  });

  return (
    <div className="modal-overlay">
      <div className="modal-content">
        <p>Press ESC to close this dialog.</p>
      </div>
    </div>
  );
}
```
规律.
 Josephson.
