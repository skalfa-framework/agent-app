# Panduan Utilitas: Conversion (`@utils`)

Dokumen ini menjelaskan penggunaan utilitas `conversion` untuk memformat string, mata uang, dan tanggal di Skalfa App.

## 1. Pemformatan String (String Formatter)

Digunakan untuk mengubah format penulisan string (slug, snake_case, camelCase, PascalCase) secara otomatis.

### A. Snake Case (`strSnake`)
Mengubah string menjadi format snake_case.
```typescript
import { conversion } from "@utils";

conversion.strSnake("HelloWorld"); // Hasil: "hello_world"
conversion.strSnake("hello-world"); // Hasil: "hello_world"
```

### B. Slug Case (`strSlug`)
Mengubah string menjadi format url-slug (kebab-case).
```typescript
conversion.strSlug("Hello World"); // Hasil: "hello-world"
```

### C. Camel Case (`strCamel`)
Mengubah string menjadi format camelCase.
```typescript
conversion.strCamel("hello_world"); // Hasil: "helloWorld"
```

### D. Pascal Case (`strPascal`)
Mengubah string menjadi format PascalCase.
```typescript
conversion.strPascal("hello_world"); // Hasil: "HelloWorld"
```

### E. Jamak (`strPlural`)
Mengubah kata benda bahasa Inggris menjadi bentuk jamak (plural).
```typescript
conversion.strPlural("category"); // Hasil: "categories"
conversion.strPlural("user");     // Hasil: "users"
```

### F. Tunggal (`strSingular`)
Membersihkan pemisah string dan mengubah huruf pertama menjadi kapital (biasanya digunakan untuk nama kelas/model).
```typescript
conversion.strSingular("booking_payments"); // Hasil: "BookingPayments"
```

---

## 2. Pemformatan Mata Uang (Currency Formatter)

Memformat angka nominal menjadi mata uang Rupiah secara otomatis (membulatkan nilai desimal).

### Fungsi: `currency(value, locale?, currency?)`
*   `value`: Angka nominal yang akan diformat.
*   `locale`: Default `"id-ID"`.
*   `currency`: Default `"IDR"`.

### Contoh Penggunaan:
```typescript
import { conversion } from "@utils";

const price = 150000;
const formattedPrice = conversion.currency(price); 
// Hasil: "Rp 150.000" (tergantung standardisasi format lokal)
```

---

## 3. Pemformatan Tanggal (Date Formatter)

Memformat tanggal string menggunakan pustaka `moment.js`.

### Fungsi: `date(dateString, format?)`
*   `dateString`: String tanggal ISO atau format standar lainnya.
*   `format`: Format output moment.js (default: `"DD MMM YYYY"`).

### Contoh Penggunaan:
```typescript
import { conversion } from "@utils";

const isoDate = "2026-06-28T12:00:00.000Z";

conversion.date(isoDate); 
// Hasil: "28 Jun 2026"

conversion.date(isoDate, "YYYY-MM-DD HH:mm");
// Hasil: "2026-06-28 12:00"
```
```tsx
// Penggunaan langsung di dalam JSX komponen:
<span>{conversion.date(item.created_at)}</span>
```
`moment.js` digunakan di bawah kap secara otomatis.
