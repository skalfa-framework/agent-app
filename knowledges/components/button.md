# Panduan Komponen: `ButtonComponent` (`@components`)

`ButtonComponent` adalah komponen tombol standar di Skalfa App yang mendukung berbagai varian, warna, ukuran, indikator loading, ikon FontAwesome, serta terintegrasi otomatis dengan rute Next.js jika diberikan properti link.

## 1. Antarmuka Komponen (`ButtonProps`)

```typescript
interface ButtonProps {
  type      ?: "submit" | "button";
  label     ?: string | ReactNode;                                       // Teks tombol
  variant   ?: "solid" | "outline" | "light" | "simple";                 // Gaya visual (default: "solid")
  paint     ?: "primary" | "secondary" | "success" | "danger" | "warning"; // Warna tema (default: "primary")
  rounded   ?: boolean | string;                                         // Tombol berbentuk bulat penuh
  block     ?: boolean;                                                  // Lebar penuh (w-full)
  disabled  ?: boolean;
  size      ?: "xs" | "sm" | "md" | "lg";                                // Ukuran (default: "md")
  onClick   ?: (e: any) => void;
  href      ?: string;                                                   // Jika diisi, tombol dirender sebagai Next.js <Link>
  icon      ?: any;                                                      // Ikon FontAwesome
  loading   ?: boolean;                                                  // Menampilkan spinner memuat data
  className ?: string;                                                   // Kustom class Tailwind
}
```

---

## 2. Fitur Utama

### A. Otomatis Menjadi Link (`href`)
Jika properti `href` diisi, tombol tidak akan merender tag `<button>` melainkan tag `<Link>` dari Next.js. Hal ini menjaga performa Single Page Application (SPA).

```tsx
import { ButtonComponent } from "@components";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";

// Otomatis bertindak sebagai navigasi tanpa reload
<ButtonComponent
  label="Kembali ke Dashboard"
  href="/dashboard"
  icon={faArrowLeft}
  variant="outline"
/>
```

### B. Indikator Memuat Data (`loading`)
Jika `loading` bernilai `true`, tombol otomatis dinonaktifkan (`disabled`) dan ikon/teks akan digantikan oleh animasi pemutar (*loading spinner*).

```tsx
import { ButtonComponent } from "@components";

<ButtonComponent
  label="Simpan Data"
  type="submit"
  loading={isSubmitting} // Jika true, spinner otomatis muncul
/>
```

---

## 3. Contoh Varian Visual

```tsx
// Tombol Sukses Solid
<ButtonComponent label="Selesai" paint="success" variant="solid" />

// Tombol Bahaya Outline
<ButtonComponent label="Hapus" paint="danger" variant="outline" />

// Tombol Sederhana Tanpa Border (Simple)
<ButtonComponent label="Batal" paint="secondary" variant="simple" />

// Tombol Kecil Bulat dengan Ikon
import { faPlus } from "@fortawesome/free-solid-svg-icons";
<ButtonComponent icon={faPlus} size="sm" rounded={true} />
```
*Catatan untuk Agen: Gunakan selalu `ButtonComponent` daripada tag HTML `<button>` biasa untuk menjaga keselarasan desain visual sistem.*
