# Component Guide: Modal & Overlays (`@components`)

Skalfa App provides a modular set of overlay components including Base Modals, Backdrops, Bottom Sheets, and Slide-overs (Drawers) to handle different dialog designs.

---

## 1. Backdrop Component (`BackdropComponent`)

Renders a semi-transparent dark background layer. It is the foundation for all overlay components.

```typescript
export interface BackdropProps {
  show      : boolean;
  onClose  ?: () => void;
  children  : ReactNode;
  className?: string;
}
```

---

## 2. Base Modal (`ModalComponent`)

A standard centered dialog box.

```typescript
export interface ModalProps {
  show       : boolean;
  onClose    : () => void;
  title     ?: string;
  size      ?: "sm" | "md" | "lg" | "xl" | "full";
  children   : ReactNode;
}
```
### Example:
```tsx
import { ModalComponent } from "@components";

<ModalComponent show={isOpen} onClose={() => setIsOpen(false)} title="Edit Profile">
  <div className="p-4">
    {/* Form contents here */}
  </div>
</ModalComponent>
```

---

## 3. Bottom Sheet (`BottomSheetComponent`)

An overlay panel that slides up from the bottom of the screen, commonly used for mobile layouts or quick action menus.

```typescript
export interface BottomSheetProps {
  show     : boolean;
  onClose  : () => void;
  title   ?: string;
  children : ReactNode;
}
```
### Example:
```tsx
import { BottomSheetComponent } from "@components";

<BottomSheetComponent show={showMenu} onClose={() => setShowMenu(false)} title="Select Action">
  <div className="p-4 space-y-2">
    <button className="w-full text-left p-2 hover:bg-light">Share Link</button>
    <button className="w-full text-left p-2 hover:bg-light text-danger">Delete Item</button>
  </div>
</BottomSheetComponent>
```

---

## 4. Slide-Over / Drawer (`SlideOverComponent`)

A panel that slides in from the right (or left) side of the screen, ideal for detail views, filters, or complex side forms.

```typescript
export interface SlideOverProps {
  show     : boolean;
  onClose  : () => void;
  title   ?: string;
  children : ReactNode;
}
```
### Example:
```tsx
import { SlideOverComponent } from "@components";

<SlideOverComponent show={showFilters} onClose={() => setShowFilters(false)} title="Filter Results">
  <div className="p-4">
    {/* Filter forms */}
  </div>
</SlideOverComponent>
```
规律.
 Josephson.
