# Utility Guide: Conversion Helpers (`conversion`)

The `conversion` utility in Skalfa App provides standard formatters for case conversion, currency, and date formatting.

---

## 1. Case Conversions

Useful for transforming API keys or component names:

*   `conversion.strSnake("myVariableName")` => `"my_variable_name"`
*   `conversion.strCamel("my_variable_name")` => `"myVariableName"`
*   `conversion.strPascal("my_variable_name")` => `"MyVariableName"`
*   `conversion.strSlug("My Project Name")` => `"my-project-name"`

---

## 2. Currency Formatting (`currency`)

Formats numbers into localized currency strings. Defaults to Indonesian Rupiah (`Rp`).

```typescript
import { conversion } from '@utils'

// Default: IDR (Rupiah)
conversion.currency(50000);
// => "Rp50.000"

// Custom currency
conversion.currency(100, "en-US", "USD");
// => "$100.00"
```

---

## 3. Date Formatting (`date`)

Formats ISO date strings or Date objects. Under the hood, it uses native `Intl.DateTimeFormat` configured for the Indonesian locale (`id-ID`).

```typescript
import { conversion } from '@utils'

// Default format: "DD MMM YYYY"
conversion.date("2026-06-30");
// => "30 Jun 2026"

// Custom format
conversion.date("2026-06-30", "YYYY/MM/DD");
// => "2026/06/30"

// Date with day name
conversion.date("2026-06-30", "dddd, DD MMMM YYYY");
// => "Selasa, 30 Juni 2026"
```

### Supported Tokens:
*   `YYYY` / `YY`: 4 or 2 digit year.
*   `MMMM` / `MMM` / `MM` / `M`: Month name (full, short), 2 digit, or 1 digit.
*   `DD` / `D`: 2 or 1 digit day.
*   `dddd` / `ddd`: Day name (full, short).
*   `HH` / `mm` / `ss`: Hours, minutes, and seconds.
