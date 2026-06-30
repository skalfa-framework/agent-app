# Skalfa Knowledge Registry (Frontend)

Before creating any implementation plan (`Stage 1 — Planning`), the agent **MUST read this registry** and only open the specific documentation files relevant to the UI components and utilities being used.

---

## 1. UI Components (`components/`)

### Base Components (`base.components`)
*   [Accordion](file:///d:/_skalfa/agent-app/knowledges/components/accordion.md): Collapsible panels with horizontal/vertical orientation.
*   [Breadcrumb](file:///d:/_skalfa/agent-app/knowledges/components/breadcrumb.md): Page navigation helper.
*   [Button](file:///d:/_skalfa/agent-app/knowledges/components/button.md): Standard button with various styles, sizes, and states (loading, disabled).
*   [Card](file:///d:/_skalfa/agent-app/knowledges/components/card.md): Structured container for grouping UI elements.
*   [Chip](file:///d:/_skalfa/agent-app/knowledges/components/chip.md): Small badge for tags, statuses, or categories.
*   [Input Components](file:///d:/_skalfa/agent-app/knowledges/components/input-components.md): Set of input fields (Text, Password, Textarea, Checkbox, Radio, Select, Date, Toggle, File).
*   [Modal Confirm](file:///d:/_skalfa/agent-app/knowledges/components/modal-confirm.md): Simple confirmation dialog (Yes/No).
*   [Modal Overlays](file:///d:/_skalfa/agent-app/knowledges/components/modal-overlays.md): Base modal, backdrop, bottom sheet, and slide-over.
*   [Tabbar](file:///d:/_skalfa/agent-app/knowledges/components/tabbar.md): Tabs for switching views.
*   [Table](file:///d:/_skalfa/agent-app/knowledges/components/table.md): Standard table with sorting, selection, and actions.
*   [Wizard](file:///d:/_skalfa/agent-app/knowledges/components/wizard.md): Multi-step form container.

### Magic Components
*   [Form Supervision](file:///d:/_skalfa/agent-app/knowledges/components/form-supervision.md): Schema-driven form generator with automatic validation.
*   [Table Supervision](file:///d:/_skalfa/agent-app/knowledges/components/table-supervision.md): Schema-driven table generator with automatic pagination, filtering, and search.

### UI Utilities
*   [Outside Click](file:///d:/_skalfa/agent-app/knowledges/components/outside-click.md): Helper hook/component to detect clicks outside an element.
*   [Typography Tips](file:///d:/_skalfa/agent-app/knowledges/components/typography-tips.md): Best practices for fonts and text styling in Skalfa App.

---

## 2. Core Utilities (`utilities/`)

*   [API Client](file:///d:/_skalfa/agent-app/knowledges/utilities/api.md): Dynamic REST API client (`api.get`, `api.post`, etc.).
*   [Auth Context](file:///d:/_skalfa/agent-app/knowledges/utilities/auth.md): Session state and permission guarding.
*   [Cavity State](file:///d:/_skalfa/agent-app/knowledges/utilities/cavity.md): Centralized global state manager.
*   [CN (Class Merging)](file:///d:/_skalfa/agent-app/knowledges/utilities/cn.md): Utility to merge Tailwind classes cleanly.
*   [Conversion Helpers](file:///d:/_skalfa/agent-app/knowledges/utilities/conversion.md): String casing, currency formatting, and date formatting.
*   [IndexedDB (IDB)](file:///d:/_skalfa/agent-app/knowledges/utilities/idb.md): Offline local database client.
*   [Resource Handler](file:///d:/_skalfa/agent-app/knowledges/utilities/resource.md): Client-side abstraction for CRUD resources.
*   [Shortcuts](file:///d:/_skalfa/agent-app/knowledges/utilities/shortcut.md): Keyboard shortcut manager.
*   [Validation Rules](file:///d:/_skalfa/agent-app/knowledges/utilities/validation.md): Frontend input validation rules matching the backend.
*   [Form & Table Hooks](file:///d:/_skalfa/agent-app/knowledges/utilities/form-table-hooks.md): Hooks used by the supervision components (`useSupervisionForm`, `useSupervisionTable`).
