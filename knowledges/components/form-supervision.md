# Component Guide: Form Supervision (`FormSupervisionComponent`)

`FormSupervisionComponent` is a "magic" component in Skalfa App. It automatically generates and manages forms based on a JSON schema definition. It handles layout rendering, validation, and submission states.

---

## 1. Component Interface (`FormSupervisionProps`)

```typescript
export interface FormSupervisionProps {
  fields      : FormSupervisionField[];               // Array of field configurations
  onSubmit    : (values: Record<string, any>) => void; // Submit callback
  loading    ?: boolean;                              // Submit loading state
  submitLabel?: string;                               // Submit button text
  className  ?: string;                               // Custom Tailwind classes
}
```

---

## 2. Field Schema Configuration (`FormSupervisionField`)

Each field in the `fields` array is configured as follows:

```typescript
export interface FormSupervisionField {
  col          : number;                     // Grid column span (1-12)
  construction : {
    name         : string;                   // Field name/key in form values
    label        : string;                   // Form label
    type        ?: string;                   // Input type (e.g. text, select, file)
    placeholder ?: string;                   // Placeholder text
    options     ?: { value: any; label: string }[]; // Options for select/radio/checkbox
    required    ?: boolean;                  // Client-side required validation
    validation  ?: string[];                 // Custom validation rules
    [key: string]: any;                      // Additional field-specific properties
  };
}
```

---

## 3. Supported Input Types

The component automatically renders the appropriate input control based on `construction.type`:
*   `text` (Default): Single-line text input.
*   `input-password`: Password input with a visibility toggle.
*   `textarea`: Multi-line text area.
*   `select`: Dropdown list.
*   `checkbox`: Checkbox (single or multiple).
*   `radio`: Radio button group.
*   `date`: Date picker.
*   `file`: File uploader.
*   `toggle`: Switch/Toggle button.

---

## 4. Usage Example

```tsx
import { FormSupervisionComponent } from "@components";
import { useState } from "react";

export function UserRegistrationForm() {
  const [submitting, setSubmitting] = useState(false);

  const fields = [
    {
      col: 6,
      construction: {
        name:        "fullname",
        label:       "Full Name",
        type:        "text",
        placeholder: "Enter your full name",
        required:    true
      }
    },
    {
      col: 6,
      construction: {
        name:        "email",
        label:       "Email Address",
        type:        "text",
        placeholder: "username@example.com",
        required:    true,
        validation:  ["email"]
      }
    },
    {
      col: 12,
      construction: {
        name:        "role",
        label:       "User Role",
        type:        "select",
        options: [
          { value: "admin", label: "Administrator" },
          { value: "member", label: "Regular Member" }
        ],
        required:    true
      }
    }
  ];

  const handleSubmit = async (values: Record<string, any>) => {
    setSubmitting(true);
    console.log("Submitting values:", values);
    await new Promise((resolve) => setTimeout(resolve, 1500));
    setSubmitting(false);
  };

  return (
    <div className="max-w-xl p-6 bg-white rounded-lg border shadow-sm">
      <h2 className="text-lg font-bold mb-4">Register New User</h2>
      <FormSupervisionComponent
        fields={fields}
        onSubmit={handleSubmit}
        loading={submitting}
        submitLabel="Create Account"
      />
    </div>
  );
}
```
