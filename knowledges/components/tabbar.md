# Panduan Komponen: Bar Tab (Tabbar) (`@components`)

`TabbarComponent` adalah komponen pemindah tab horizontal di Skalfa App yang digunakan untuk beralih tampilan antar kategori data, memfilter tabel, atau memisahkan panel informasi tanpa melakukan navigasi rute halaman.

## 1. Antarmuka Komponen (`TabbarProps`)

```typescript
export interface TabbarProps {
  items      : string[] | TabbarItemProps[];          // Daftar item tab (bisa array string atau objek)
  active    ?: string | number;                       // Nilai tab yang sedang aktif
  onChange  ?: (value: string | number) => void;      // Callback saat tab diklik
  className ?: string;                                // Kustom class Tailwind
}

interface TabbarItemProps {
  label : string;                                     // Label teks yang ditampilkan di UI
  value : string | number;                            // Nilai data internal tab
}
```

---

## 2. Fitur Utama

*   **Pilihan Aktif Berbentuk Kapsul**: Item tab yang aktif akan otomatis memiliki latar belakang putih transparan (`bg-white/60`) dengan font tebal (`font-semibold`) dan warna teks primer (`text-primary`).
*   **Efek Hover Halus**: Tab non-aktif memiliki efek hover latar belakang lembut dan kursor pointer untuk kenyamanan interaksi pengguna.
*   **Dua Tipe Data Input**:
    *   *Sederhana (Array String)*: Jika data label dan value sama (contoh: `["Semua", "Aktif", "Batal"]`).
    *   *Kompleks (Array Objek)*: Jika label yang ditampilkan berbeda dengan value pemrosesan data (contoh: `[{ label: "Sudah Bayar", value: "paid" }]`).

---

## 3. Contoh Penggunaan

### A. Tab Sederhana untuk Filter Kategori
```tsx
import React, { useState } from "react";
import { TabbarComponent } from "@components";

export function CategoryFilter() {
  const [activeTab, setActiveTab] = useState("Semua");

  return (
    <TabbarComponent
      active={activeTab}
      items={["Semua", "Makanan", "Minuman", "Pakaian"]}
      onChange={(val) => setActiveTab(String(val))}
    />
  );
}
```

### B. Tab Kompleks dengan Nilai Data Berbeda
```tsx
import React, { useState } from "react";
import { TabbarComponent } from "@components";

export function TransactionTabs({ onFilterChange }) {
  const [status, setStatus] = useState("all");

  const handleTabChange = (val: string | number) => {
    setStatus(String(val));
    onFilterChange(val);
  };

  return (
    <TabbarComponent
      active={status}
      onChange={handleTabChange}
      items={[
        { label: "Semua Transaksi", value: "all" },
        { label: "Menunggu",        value: "pending" },
        { label: "Lunas",           value: "paid" }
      ]}
    />
  );
}
```
*Catatan untuk Agen: Gunakan `TabbarComponent` untuk beralih tampilan di dalam satu halaman yang sama tanpa memicu perubahan URL path.*
