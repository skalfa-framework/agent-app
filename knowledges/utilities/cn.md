# Panduan Utilitas: Class Name & Kustomisasi Gaya (`cn`, `pcn`) (`@utils`)

Dokumen ini menjelaskan penggunaan utilitas `cn` dan `pcn` untuk menggabungkan kelas CSS Tailwind secara aman serta mendukung penulisan gaya kustom berbasis prefiks di Skalfa App.

## 1. Penggabungan Kelas Tailwind Aman (`cn`)

Metode `cn(...classes)` menggabungkan nama-nama kelas CSS menggunakan `clsx` dan menyelaraskan konflik kelas Tailwind menggunakan `tailwind-merge`.

### Contoh Penggunaan:
```typescript
import { cn } from "@utils";

// twMerge akan memastikan 'bg-blue-600' menimpa 'bg-red-500' secara aman
const classes = cn("px-4 py-2 bg-red-500", isPrimary && "bg-blue-600");
```

---

## 2. Kelas Kustom Berprefiks (`pcn` / Prefix Class Name)

Komponen bawaan Skalfa sering kali terdiri dari beberapa sub-elemen (misalnya modal memiliki area *backdrop*, *header*, dan *footer*). Untuk mempermudah kustomisasi gaya sub-elemen tersebut dari luar, Skalfa menggunakan pembatas titik dua ganda (`::`) melalui utilitas `pcn`.

### Cara Kerja:
Fungsi `pcn(className, prefix, pseudoClass?)` menyaring string kelas yang diawali dengan nama prefiks tertentu, lalu membuang prefiks tersebut saat komponen merendernya.

*   *Format Penulisan di Prop*: `prefiks::nama-kelas-tailwind` (contoh: `backdrop::bg-black/50`)
*   *Default*: Jika kelas tidak memiliki prefiks, kelas tersebut otomatis dianggap milik prefiks `"base"` atau `"input"`.

### Contoh Simulasi Penyaringan (`pcn`):
```typescript
import { pcn } from "@utils";

const customClass = "bg-white border backdrop::bg-slate-900/40 header::text-primary";

// Menyaring kelas untuk area backdrop
pcn(customClass, "backdrop"); // Hasil: "bg-slate-900/40"

// Menyaring kelas untuk area header
pcn(customClass, "header");   // Hasil: "text-primary"

// Menyaring kelas dasar (tanpa prefiks)
pcn(customClass, "base");     // Hasil: "bg-white border"
```

### Contoh Implementasi di Komponen Kustom:
Jika Anda membuat komponen kustom dengan sub-elemen, gunakan pola ini agar mudah dikustomisasi dari luar:

```tsx
import React from "react";
import { cn, pcn } from "@utils";

type CT = "container" | "title" | "base";

export function CustomBox({ children, className = "" }: { children: React.ReactNode, className?: string }) {
  return (
    <div className={cn("p-4 border", pcn<CT>(className, "container"))}>
      <h4 className={cn("font-semibold", pcn<CT>(className, "title"))}>
        Judul Box
      </h4>
      <div className={pcn<CT>(className, "base")}>
        {children}
      </div>
    </div>
  );
}

// Cara memanggil komponen dari luar dengan kustomisasi sub-elemen:
<CustomBox className="container::bg-slate-50 title::text-danger text-sm">
  Konten Box
</CustomBox>
```
*Catatan untuk Agen: Selalu gunakan `cn` untuk penggabungan kelas dinamis dan terapkan pola `pcn` jika membuat komponen majemuk yang membutuhkan kustomisasi gaya sub-elemen.*
