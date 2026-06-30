# Component Guide: Tabbar (`TabbarComponent`)

`TabbarComponent` is a tab navigation component in Skalfa App. It is used to switch between different views or sub-sections within a page without changing the URL.

---

## 1. Component Interface (`TabbarProps`)

```typescript
export interface TabbarProps {
  items: {
    key      : string;               // Unique identifier for the tab
    label    : ReactNode;             // Tab title text or JSX
    icon    ?: IconDefinition;        // Optional FontAwesome icon displayed before the title
    content  : ReactNode;             // The panel content displayed when active
  }[];
  activeKey ?: string;                // The key of the tab open by default
  onChange  ?: (key: string) => void; // Callback when the active tab changes
  className ?: string;                // Custom Tailwind CSS classes
}
```

---

## 2. Key Features

*   **Icon Integration**: Easily add FontAwesome icons alongside text titles.
*   **Controlled & Uncontrolled Modes**: Can be used as a controlled component (`activeKey` + `onChange`) or let it manage its own active tab state internally.
*   **Lazy Rendering**: Content of inactive tabs is kept hidden using CSS classes (`hidden`) to preserve state and avoid unnecessary re-renders of tab views.

---

## 3. Usage Example

```tsx
import { TabbarComponent } from "@components";
import { faUser, faShieldAlt, faBell } from "@fortawesome/free-solid-svg-icons";

export function SettingsTabs() {
  const tabs = [
    {
      key:     "profile",
      label:   "Profile Settings",
      icon:    faUser,
      content: <div className="p-4">Profile edit form goes here...</div>
    },
    {
      key:     "security",
      label:   "Security & Password",
      icon:    faShieldAlt,
      content: <div className="p-4">Change password form goes here...</div>
    },
    {
      key:     "notifications",
      label:   "Notification Preferences",
      icon:    faBell,
      content: <div className="p-4">Configure email/push alerts...</div>
    }
  ];

  return (
    <div className="bg-white border rounded-lg shadow-sm">
      <TabbarComponent items={tabs} activeKey="profile" />
    </div>
  );
}
```
 Josephson.
