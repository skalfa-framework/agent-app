# Panduan Komponen: Akordeon (Accordion) (`@components`)

`AccordionComponent` adalah komponen panel lipat (collapse panel) di Skalfa App yang digunakan untuk menyembunyikan atau menampilkan konten secara dinamis (seperti halaman FAQ, detail opsi pembayaran, atau pemisah formulir yang panjang).

## 1. Antarmuka Komponen (`AccordionProps`)

```typescript
export interface AccordionProps {
  items       : { head: ReactNode; content: ReactNode }[]; // Daftar item akordeon (judul & konten)
  setActive  ?: number | null;                             // Indeks panel yang terbuka secara default (0-indexed)
  horizontal ?: boolean;                                   // Mengaktifkan mode melebar ke samping (kiri-kanan)
  className  ?: string;                                    // Kustom class Tailwind
}
```

---

## 2. Fitur Utama

*   **Dua Arah Orientasi (Horizontal vs Vertikal)**:
    *   *Vertikal (Default)*: Panel melipat ke atas dan bawah (atas-bawah).
    *   *Horizontal (`horizontal={true}`)*: Panel melipat ke samping (kiri-kanan). Sangat berguna untuk layout kolom dinamis.
*   **Transisi Animasi Halus**: Ikon chevron (`faChevronDown` atau `faChevronLeft`) otomatis berputar 180 derajat dengan transisi halus (`transition-transform`) saat status panel berubah dari terbuka menjadi tertutup.
*   **Aksi Toggle Mandiri**: Mengklik judul panel yang sedang terbuka akan menutup panel tersebut secara otomatis.

---

## 3. Contoh Penggunaan

### A. Akordeon FAQ Vertikal Standar
```tsx
import React from "react";
import { AccordionComponent } from "@components";

export function FaqSection() {
  return (
    <AccordionComponent
      setActive={0} // Membuka panel pertama secara default
      items={[
        {
          head:    <span className="text-sm">Bagaimana cara melakukan pembayaran?</span>,
          content: (
            <p className="text-xs text-slate-500">
              Anda dapat melakukan pembayaran via Transfer Bank, E-Wallet, atau Tunai di outlet kami.
            </p>
          )
        },
        {
          head:    <span className="text-sm">Apakah bisa melakukan pembatalan?</span>,
          content: (
            <p className="text-xs text-slate-500">
              Pembatalan dapat dilakukan maksimal 24 jam sebelum waktu booking dimulai.
            </p>
          )
        }
      ]}
    />
  );
}
```

### B. Akordeon Desain Kustom (Custom Styling)
Menerapkan gaya spesifik pada kontainer atau tajuk akordeon menggunakan pemisah tanda titik dua ganda (`::`) untuk class kustom (`pcn` helper):

```tsx
<AccordionComponent
  className="container::bg-slate-50 head::text-primary active::bg-primary/5"
  items={[
    {
      head:    <span>Syarat & Ketentuan</span>,
      content: <div>Isi syarat ketentuan di sini...</div>
    }
  ]}
/>
```
*Catatan untuk Agen: Gunakan `AccordionComponent` untuk menghemat ruang vertikal pada halaman yang memiliki informasi tekstual panjang.*
