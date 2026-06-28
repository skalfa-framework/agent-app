# Panduan Komponen: Tag Pilihan (Chip) (`@components`)

`ChipComponent` adalah komponen badge kecil interaktif di Skalfa App yang digunakan untuk menampilkan daftar opsi terpilih, tag kategori, kata kunci pencarian aktif, atau data array kecil dengan opsi hapus instan.

## 1. Antarmuka Komponen (`ChipComponent`)

```typescript
export function ChipComponent({
  items,
  onClick,
  onDelete,
  className,
}: {
  items      : any[];                                           // Array string atau objek data yang ditampilkan
  onClick   ?: (item: any, index: number) => void;              // Event klik pada item chip
  onDelete  ?: (item: any, index: number) => void;              // Renders delete icon (faTimes) and triggers event
  className ?: string;
})
```

---

## 2. Fitur Utama

*   **Tombol Hapus Otomatis (`onDelete`)**: Jika fungsi `onDelete` dilewatkan ke properti, komponen otomatis merender ikon silang kecil (`faTimes`) di sisi kanan setiap chip. Tombol ini memiliki efek hover merah (`hover:text-danger`) untuk indikasi tindakan hapus.
*   **Bungkus Baris (Flex Wrap)**: Secara default, wadah chip menggunakan kelas `flex-wrap` sehingga jika jumlah chip melebihi lebar layar, chip akan otomatis turun ke baris berikutnya secara rapi.

---

## 3. Contoh Penggunaan

### A. Daftar Tag Kategori Sederhana (Read-Only)
Menampilkan daftar tag biasa tanpa aksi hapus.

```tsx
import { ChipComponent } from "@components";

const categories = ["Teknologi", "Kesehatan", "Pendidikan"];

<ChipComponent items={categories} />
```

### B. Tag Interaktif dengan Fitur Hapus (Contoh Pilihan Filter)
Menampilkan tag aktif dan menghapusnya dari state ketika tombol silang diklik.

```tsx
import React, { useState } from "react";
import { ChipComponent } from "@components";

export function ActiveFilters() {
  const [selectedTags, setSelectedTags] = useState(["Budi", "Lunas", "Bandung"]);

  const handleRemoveTag = (itemToRemove, index) => {
    setSelectedTags(prev => prev.filter((_, i) => i !== index));
  };

  return (
    <div className="flex flex-col gap-2">
      <span className="text-xs text-slate-400">Filter Aktif:</span>
      <ChipComponent
        items={selectedTags}
        onDelete={handleRemoveTag}
      />
    </div>
  );
}
```
*Catatan untuk Agen: Gunakan `ChipComponent` untuk menampilkan data pilihan jamak (multiple selection) atau tag pencarian aktif agar user dapat melihat dan membatalkan pilihannya dengan cepat.*
