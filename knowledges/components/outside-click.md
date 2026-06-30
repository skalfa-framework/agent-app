# Component Guide: Outside Click Detector (`OutsideClickComponent`)

`OutsideClickComponent` (and its corresponding hook `useOutsideClick`) is used in Skalfa App to detect when a user clicks outside a specific element. This is commonly used to close dropdown menus, tooltips, or popovers.

---

## 1. Hook Interface (`useOutsideClick`)

```typescript
export function useOutsideClick(
  ref: RefObject<HTMLElement>,
  callback: () => void
): void;
```

---

## 2. Component Wrapper (`OutsideClickComponent`)

A wrapper component that accepts a children element and triggers `onOutsideClick` when a click occurs outside of it.

```typescript
export interface OutsideClickProps {
  onOutsideClick : () => void;
  children       : ReactElement;
}
```

---

## 3. Usage Examples

### A. Using the Hook (Preferred for Custom Components)
```tsx
import { useOutsideClick } from "@components";
import { useRef, useState } from "react";

export function DropdownMenu() {
  const [isOpen, setIsOpen] = useState(false);
  const menuRef = useRef<HTMLDivElement>(null);

  // Close menu when clicking outside
  useOutsideClick(menuRef, () => {
    if (isOpen) setIsOpen(false);
  });

  return (
    <div ref={menuRef} className="relative inline-block">
      <button onClick={() => setIsOpen(!isOpen)}>Toggle Menu</button>
      {isOpen && (
        <div className="absolute right-0 mt-2 w-48 bg-white border shadow-lg rounded-md p-2">
          <p className="p-2 hover:bg-light cursor-pointer">Option A</p>
          <p className="p-2 hover:bg-light cursor-pointer">Option B</p>
        </div>
      )}
    </div>
  );
}
```

### B. Using the Component Wrapper
```tsx
import { OutsideClickComponent } from "@components";
import { useState } from "react";

export function PopoverInfo() {
  const [showPopover, setShowPopover] = useState(false);

  return (
    <div className="relative">
      <button onClick={() => setShowPopover(true)}>Info</button>

      {showPopover && (
        <OutsideClickComponent onOutsideClick={() => setShowPopover(false)}>
          <div className="absolute top-8 left-0 p-4 bg-white border rounded shadow-md z-10">
            <p>This is a popover. Click outside to close it.</p>
          </div>
        </OutsideClickComponent>
      )}
    </div>
  );
}
```
