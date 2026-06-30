# Typography Tips in Skalfa App

Skalfa App uses a modern typography system based on the Inter font and Tailwind CSS. Avoid using custom font sizes or custom inline styling. Always adhere to the established text utilities.

---

## 1. Heading Hierarchy

Always use a single `<h1>` per page. Follow the hierarchy for headings:

*   **Page Title**: `text-2xl font-bold text-foreground` (`<h1>`)
*   **Section Title**: `text-lg font-semibold text-foreground` (`<h2>`)
*   **Card Title**: `text-md font-medium text-foreground` (`<h3>`)
*   **Sub-section Title**: `text-sm font-medium text-foreground` (`<h4>`)

---

## 2. Text Colors

Use semantic text colors to represent different states and hierarchies:

*   **Primary Text**: `text-foreground` (Used for body text, headings, and important labels).
*   **Secondary Text**: `text-light-foreground` (Used for descriptions, placeholders, and secondary info).
*   **Muted Text**: `text-muted-foreground` (Used for timestamps, helper text, and disabled states).
*   **Interactive Link**: `text-primary hover:underline` (Used for links).

---

## 3. Best Practices

1.  **Don't hardcode hex colors**: Never write `<span style={{ color: '#333' }}>`. Use Tailwind utility classes like `text-foreground`.
2.  **Maintain contrast**: Ensure that text placed on dark backgrounds uses light text classes (e.g., `text-white` or `text-dark-foreground`).
3.  **Use line heights**: Always pair font sizes with corresponding line heights (e.g., `text-sm leading-relaxed`).
