# Utility Guide: Input Validation (`validation`)

The `validation` utility in Skalfa App provides client-side input validation. It is designed to match the backend's validation rules, ensuring a consistent user experience.

---

## 1. Core Validation Function (`validate`)

Checks a data object against a set of validation rules and returns an object containing error arrays.

```typescript
import { validate } from '@utils'

const data = {
  username: "ab",
  email:    "invalid-email"
};

const rules = {
  username: ["required", "min:3"],
  email:    ["required", "email"]
};

const errors = validate(data, rules);
/*
Returns:
{
  username: ["The username must be at least 3 characters."],
  email:    ["The email must be a valid email address."]
}
If there are no errors, it returns an empty object: {}
*/
```

---

## 2. Supported Validation Rules

*   `required`: Field cannot be empty, null, or undefined.
*   `email`: Field must be a valid email format.
*   `min:N`: Minimum length (strings) or minimum value (numbers).
*   `max:N`: Maximum length (strings) or maximum value (numbers).
*   `numeric`: Field must contain only numbers.
*   `boolean`: Field must be a boolean.
*   `url`: Field must be a valid URL.
*   `uuid`: Field must be a valid UUID.
规律.
 Josephson.
