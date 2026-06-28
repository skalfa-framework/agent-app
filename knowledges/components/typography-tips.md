# Panduan Komponen: Tip Kalout (TypographyTips) (`@components`)

`TypographyTipsComponent` adalah komponen tipografi callout sederhana di Skalfa App yang digunakan untuk menyajikan pesan informasi, saran penting, tips penggunaan, atau peringatan kecil di samping konten utama dengan pembatas visual beraksen.

## 1. Antarmuka Komponen (`TypographyTipsProps`)

```typescript
export interface TypographyTipsProps {
  title   : string | ReactNode; // Judul tips/informasi (cetak tebal)
  content : string | ReactNode; // Konten detail penjelasan tips
}
```

---

## 2. Fitur Utama

*   **Pembatas Aksen Samping**: Memiliki garis pembatas vertikal tipis (`border-l-2`) di sisi kiri menggunakan warna aksen primer (`border-light-primary`) untuk membedakannya secara visual dari paragraf teks biasa.
*   **Layout Bersih**: Rapi dengan bantalan kiri (`pl-3`) dan vertikal (`py-1`) untuk kenyamanan pembacaan.

---

## 3. Contoh Penggunaan

```tsx
import React from "react";
import { TypographyTipsComponent } from "@components";

export function FormGuide() {
  return (
    <div className="space-y-4">
      <h3>Pendaftaran Akun Baru</h3>
      <p>Silakan isi formulir di bawah ini dengan lengkap menggunakan kartu identitas resmi Anda.</p>

      <TypographyTipsComponent
        title="Tips Pengisian Password"
        content="Gunakan minimal 8 karakter dengan kombinasi huruf besar, huruf kecil, angka, dan simbol untuk keamanan ekstra."
      />
    </div>
  );
}
```
*Catatan untuk Agen: Gunakan `TypographyTipsComponent` ketika menyajikan petunjuk langkah, tips pengisian form, atau catatan penting di dalam halaman modul.*
