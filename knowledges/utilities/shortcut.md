# Panduan Utilitas: Pintasan Keyboard (Shortcut) (`@utils`)

Dokumen ini menjelaskan penggunaan utilitas `shortcut` untuk mendaftarkan tombol pintas keyboard (keyboard shortcuts) secara global di aplikasi Skalfa App.

## 1. Cara Kerja `shortcut`

*   **Pencegahan Input Otomatis**: Secara default, utilitas pintasan ini **tidak akan dipicu** jika pengguna sedang aktif mengetik di dalam elemen input teks (`INPUT`), textarea (`TEXTAREA`), atau elemen dengan atribut `contenteditable`. Hal ini mencegah pintasan terganggu saat menulis teks.
*   **Dukungan Kombinasi Tombol**: Mendukung kombinasi tombol dengan `ctrl`, `shift`, dan `alt` (ditulis dengan huruf kecil dan dipisahkan tanda tambah, contoh: `ctrl+s`, `shift+arrowdown`).

---

## 2. Metode yang Tersedia

### A. Mendaftarkan Pintasan (`shortcut.register`)
Mendaftarkan fungsi handler untuk mendengarkan tombol tertentu.

```typescript
import { shortcut } from "@utils";

// Format: shortcut.register(keyCombo, handler, description?)
shortcut.register("ctrl+s", (e) => {
  console.log("Menyimpan data...");
}, "Menyimpan form transaksi");
```

### B. Menghapus Pintasan (`shortcut.unregister`)
Wajib menghapus pintasan ketika komponen dilepas (*unmounted*) untuk mencegah kebocoran memori (*memory leak*).

```typescript
shortcut.unregister("ctrl+s");
```

---

## 3. Integrasi dalam React Component (`useEffect`)

Pola terbaik mendaftarkan dan membersihkan pintasan di dalam komponen React adalah menggunakan hook `useEffect`:

```tsx
import React, { useEffect } from "react";
import { shortcut } from "@utils";

export function MyComponent({ onClose }: { onClose: () => void }) {
  useEffect(() => {
    // Daftarkan saat komponen dimuat
    shortcut.register("escape", () => {
      onClose();
    }, "Menutup halaman");

    // Bersihkan saat komponen dilepas (unmounted)
    return () => {
      shortcut.unregister("escape");
    };
  }, [onClose]);

  return <div>Konten Komponen</div>;
}
```
*Catatan untuk Agen: Selalu bersihkan pintasan menggunakan return function di dalam `useEffect` agar tidak mengganggu navigasi halaman lain.*
