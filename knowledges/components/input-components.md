# Component Guide: Input Components (`@components`)

Skalfa App provides a comprehensive set of input components located under `components/base.components/input/`. These are primitive inputs designed to be used in forms.

---

## 1. Text Input (`InputComponent`)

Standard single-line text input field.

```typescript
export interface InputProps extends InputHTMLAttributes<HTMLInputElement> {
  label        ?: string;
  error        ?: string;  // Error message displayed below the input
  icon         ?: IconDefinition;
  wrapperClass ?: string;
}
```
### Example:
```tsx
import { InputComponent } from "@components";
import { faEnvelope } from "@fortawesome/free-solid-svg-icons";

<InputComponent
  label="Email Address"
  type="email"
  placeholder="enter your email"
  icon={faEnvelope}
/>
```

---

## 2. Password Input (`InputPasswordComponent`)

A password input field equipped with a built-in eye icon to toggle password visibility.

```tsx
import { InputPasswordComponent } from "@components";

<InputPasswordComponent
  label="Password"
  placeholder="Min 8 characters"
/>
```

---

## 3. Textarea Input (`TextareaComponent`)

A multi-line text input field.

```tsx
import { TextareaComponent } from "@components";

<TextareaComponent
  label="Description"
  placeholder="Write something..."
  rows={4}
/>
```

---

## 4. Select Input (`SelectComponent`)

A dropdown selection list.

```typescript
export interface SelectProps extends SelectHTMLAttributes<HTMLSelectElement> {
  label    ?: string;
  options   : { value: any; label: string }[];
  error    ?: string;
}
```
### Example:
```tsx
import { SelectComponent } from "@components";

<SelectComponent
  label="Gender"
  options={[
    { value: "M", label: "Male" },
    { value: "F", label: "Female" }
  ]}
/>
```

---

## 5. Checkbox & Radio (`CheckboxComponent` / `RadioComponent`)

Used for binary selections or selecting from a list of options.

```tsx
import { CheckboxComponent, RadioComponent } from "@components";

// Checkbox
<CheckboxComponent label="I agree to terms and conditions" checked={true} />

// Radio Group
<div className="flex gap-4">
  <RadioComponent label="Option A" name="group1" value="A" />
  <RadioComponent label="Option B" name="group1" value="B" />
</div>
```

---

## 6. Toggle Switch (`ToggleComponent`)

A switch UI element representing a boolean state.

```tsx
import { ToggleComponent } from "@components";
import { useState } from "react";

export function Settings() {
  const [enabled, setEnabled] = useState(false);
  return (
    <ToggleComponent
      label="Enable Notifications"
      checked={enabled}
      onChange={() => setEnabled(!enabled)}
    />
  );
}
```
