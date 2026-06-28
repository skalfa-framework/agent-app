# Panduan Komponen: Kartu (Card) (`@components`)

Dokumen ini menjelaskan penggunaan komponen kartu (`CardComponent` dan `DashboardCardComponent`) sebagai kontainer visual standar di Skalfa App.

## 1. Kartu Standar (`CardComponent`)

Digunakan sebagai kontainer pembungkus layout putih bersih dengan bayangan lembut dan border tipis.

### Antarmuka Komponen (`CardProps`)
```typescript
interface CardProps {
  children   : React.ReactNode;
  className ?: string;          // Kustom class Tailwind
}
```

### Contoh Penggunaan:
```tsx
import { CardComponent } from "@components";

<CardComponent className="shadow-sm max-w-md">
  <h3 className="font-semibold text-lg">Informasi Pelanggan</h3>
  <p className="text-sm text-slate-500">Nama: Ahmad Budi</p>
</CardComponent>
```

---

## 2. Kartu Dashboard Statistik (`DashboardCardComponent`)

Kartu khusus untuk menampilkan ringkasan informasi, data statistik, atau pintasan menu navigasi di halaman ringkasan (dashboard).

### Antarmuka Komponen (`DashboardCardProps`)
```typescript
interface DashboardCardProps {
  content      ?: string | ReactNode; // Konten utama (biasanya angka statistik / ikon besar)
  title        ?: string | ReactNode; // Judul statistik di bawah konten
  rightContent ?: string | ReactNode; // Konten tambahan di sisi kanan (misal: grafik mini/badge persentase)
  path         ?: string;             // Rute tujuan Next.js (jika diisi, kartu bertindak sebagai Link)
  className    ?: string;             // Kustom class Tailwind
}
```

### Fitur Otomatisasi Link (`path`):
Jika properti `path` diisi, seluruh komponen kartu otomatis dibungkus menggunakan tag `<Link>` Next.js sehingga kartu tersebut dapat diklik untuk navigasi instan tanpa reload halaman.

### Contoh Penggunaan di Dashboard:
```tsx
import React from "react";
import { DashboardCardComponent } from "@components";
import { faUsers, faChevronRight } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";

export function StatsGrid() {
  return (
    <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
      <DashboardCardComponent
        path="/dashboard/users"
        content={
          <div className="flex items-center gap-3">
            <FontAwesomeIcon icon={faUsers} className="text-primary text-xl" />
            <span className="text-2xl font-bold">1,240</span>
          </div>
        }
        title={<span className="text-sm text-slate-500">Total Pengguna Aktif</span>}
        rightContent={<FontAwesomeIcon icon={faChevronRight} className="text-slate-300" />}
      />
    </div>
  );
}
```
*Catatan untuk Agen: Selalu gunakan `DashboardCardComponent` untuk membuat grid widget statistik pada dashboard demi konsistensi bentuk visual kartu.*
