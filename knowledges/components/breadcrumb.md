# Panduan Komponen: Breadcrumb (`@components`)

`BreadcrumbComponent` adalah komponen navigasi remah roti (breadcrumb) di Skalfa App yang digunakan untuk menunjukkan lokasi halaman aktif pengguna dalam hierarki rute aplikasi.

## 1. Antarmuka Komponen (`BreadcrumbProps`)

```typescript
export interface BreadcrumbProps {
  items             : BreadcrumbItemProps[];            // Daftar rute navigasi
  square           ?: boolean;                           // Gaya visual berkotak (pill background)
  separatorContent ?: string | ReactNode;                // Karakter pemisah kustom (default: faChevronRight)
  className        ?: string;                            // Kustom class Tailwind
}

interface BreadcrumbItemProps {
  path       : string;                                   // Rute tujuan Next.js
  label      : string;                                   // Label teks navigasi
  className ?: string;
}
```

---

## 2. Fitur Utama

*   **Penyelarasan SPA otomatis**: Setiap item menggunakan tag `<Link>` Next.js sehingga transisi halaman terjadi secara instan tanpa memicu reload penuh.
*   **Gaya Berkotak (`square`)**: Jika disetel ke `true`, setiap item navigasi memiliki latar belakang abu-abu transparan lembut (`bg-light-foreground/10`), dan item aktif akan berwarna latar belakang primer (`bg-primary/10`).
*   **Deteksi Halaman Aktif**: Item terakhir dalam array `items` otomatis dianggap sebagai halaman aktif (`isActive`) dan diberi warna primer (`text-primary`).

---

## 3. Contoh Penggunaan

### A. Navigasi Standar dengan Ikon Pemisah Chevron
```tsx
import React from "react";
import { BreadcrumbComponent } from "@components";

export function PageHeader() {
  return (
    <BreadcrumbComponent
      items={[
        { path: "/dashboard", label: "Dashboard" },
        { path: "/dashboard/bookings", label: "Pemesanan" },
        { path: "/dashboard/bookings/create", label: "Tambah Baru" }
      ]}
    />
  );
}
```

### B. Navigasi Gaya Pil Berkotak (`square`)
```tsx
<BreadcrumbComponent
  square={true}
  items={[
    { path: "/dashboard", label: "Home" },
    { path: "/dashboard/profile", label: "Profil Saya" }
  ]}
/>
```
*Catatan untuk Agen: Selalu letakkan `BreadcrumbComponent` di bagian atas halaman detail atau halaman sub-fitur untuk memudahkan pengguna kembali ke halaman induk.*
