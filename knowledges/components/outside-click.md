# Panduan Komponen: Deteksi Klik Luar (OutsideClick) (`@components`)

`OutsideClickComponent` adalah komponen pembungkus (wrapper component) di Skalfa App yang mendeteksi ketukan atau klik di luar area elemen anak (*children*) yang dibungkusnya. Komponen ini sangat penting digunakan untuk menutup dropdown, menu popover, tooltip, atau modal kustom ketika pengguna mengklik area luar.

## 1. Antarmuka Komponen (`OutsideClickHandlerProps`)

```typescript
export interface OutsideClickHandlerProps {
  children        : ReactNode;         // Elemen yang ingin dilindungi dari deteksi klik luar
  onOutsideClick ?: () => void;        // Callback fungsi saat klik terjadi di luar area children
}
```

---

## 2. Fitur Utama & Keamanan

*   **Penggunaan Pointer Events**: Menggunakan listener `pointerdown` alih-alih `click` biasa untuk mendukung respon cepat di perangkat layar sentuh (mobile) maupun desktop secara konsisten.
*   **Penggunaan `queueMicrotask`**: Di bawah kap, komponen memproses callback di dalam microtask untuk memastikan tidak ada konflik siklus rendering React sewaktu state visibilitas diubah bersamaan dengan event bubbling.
*   **Pembersihan Otomatis**: Secara otomatis menghapus event listener dari `document` saat komponen dilepas (*unmounted*) untuk menghindari kebocoran memori.

---

## 3. Contoh Penggunaan

### Membuat Dropdown Menu Kustom
Contoh di bawah menunjukkan cara membuat menu dropdown sederhana yang otomatis menutup sendiri jika area luar diklik.

```tsx
import React, { useState } from "react";
import { OutsideClickComponent, ButtonComponent } from "@components";

export function CustomDropdown() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div className="relative inline-block">
      <ButtonComponent
        label="Menu Opsi"
        onClick={() => setIsOpen(!isOpen)}
      />

      {isOpen && (
        <OutsideClickComponent onOutsideClick={() => setIsOpen(false)}>
          <div className="absolute right-0 mt-2 w-48 bg-white border rounded-md shadow-lg z-50 py-1">
            <a href="#edit" className="block px-4 py-2 text-sm text-slate-700 hover:bg-slate-50">Ubah Profil</a>
            <a href="#settings" className="block px-4 py-2 text-sm text-slate-700 hover:bg-slate-50">Pengaturan</a>
          </div>
        </OutsideClickComponent>
      )}
    </div>
  );
}
```
*Catatan untuk Agen: Jangan menggunakan deteksi klik dokumen manual di dalam komponen presentasional. Selalu bungkus elemen dropdown kustom Anda menggunakan `OutsideClickComponent`.*
