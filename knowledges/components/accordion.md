# Component Guide: Accordion (`AccordionComponent`)

`AccordionComponent` is a collapsible panel component in Skalfa App. It is used to dynamically show or hide content, such as FAQs, payment details, or sections in long forms.

---

## 1. Component Interface (`AccordionProps`)

```typescript
export interface AccordionProps {
  items       : { head: ReactNode; content: ReactNode }[]; // List of accordion items (title & content)
  setActive  ?: number | null;                             // Index of the panel open by default (0-indexed)
  horizontal ?: boolean;                                   // Enables horizontal collapse mode
  className  ?: string;                                    // Custom Tailwind CSS classes
}
```

---

## 2. Key Features

*   **Two-way Orientation**:
    *   *Vertical (Default)*: Panels expand and collapse vertically (top-to-bottom).
    *   *Horizontal (`horizontal={true}`)*: Panels expand and collapse horizontally (left-to-right), useful for dynamic column layouts.
*   **Smooth Animations**: The chevron icon (`faChevronDown` or `faChevronLeft`) automatically rotates 180 degrees with a smooth transition (`transition-transform`) when the panel state toggles.
*   **Self-Closing Toggle**: Clicking on the header of an already open panel will collapse it.

---

## 3. Usage Examples

### A. Vertical Accordion (Default)
```tsx
import { AccordionComponent } from "@components";

export function FaqSection() {
  const faqItems = [
    {
      head: <span>How do I reset my password?</span>,
      content: <p>Go to settings, click "Reset Password", and follow the instructions.</p>
    },
    {
      head: <span>Is my data secure?</span>,
      content: <p>Yes, we use industry-standard AES encryption to protect your data.</p>
    }
  ];

  return (
    <div className="max-w-2xl mx-auto p-4">
      <h2 className="text-xl font-bold mb-4">Frequently Asked Questions</h2>
      <AccordionComponent items={faqItems} setActive={0} />
    </div>
  );
}
```

### B. Horizontal Accordion
```tsx
import { AccordionComponent } from "@components";

export function ColumnSelector() {
  const columns = [
    {
      head: <div className="p-2 bg-primary text-white">Panel A</div>,
      content: <div className="p-4 border">Content of Panel A (Expanded horizontally)</div>
    },
    {
      head: <div className="p-2 bg-secondary text-white">Panel B</div>,
      content: <div className="p-4 border">Content of Panel B (Expanded horizontally)</div>
    }
  ];

  return (
    <div className="h-64 flex">
      <AccordionComponent items={columns} horizontal={true} />
    </div>
  );
}
```
