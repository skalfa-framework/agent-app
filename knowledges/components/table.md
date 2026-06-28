# Panduan Komponen: Tabel Data (Table) (`@components`)

`TableComponent` adalah komponen presentasi tabel data (datatable) dasar di Skalfa App yang digunakan untuk merender baris data dalam format grid, mendukung paginasi kustom, pengurutan kolom, seleksi baris (checkboxes), dan optimasi tampilan mobile.

## 1. Antarmuka Komponen (`TableProps`)

```typescript
export interface TableProps {
  columns            : TableColumnType[];                       // Definisi kolom tabel
  data               : Record<string, any>[];                   // Baris data (array objek)
  pagination        ?: PaginationProps | false;                 // Konfigurasi komponen paginasi
  loading           ?: boolean;                                 // Tampilkan state memuat data
  sortBy            ?: string[];                                // Status pengurutan aktif (contoh: ["id", "desc"])
  onChangeSortBy    ?: (sort: string[]) => void;                // Callback saat kolom diurutkan
  search            ?: string;                                  // Kata kunci pencarian
  onChangeSearch    ?: (search: string) => void;
  checks            ?: (string | number)[];                    // Array ID baris yang dicentang
  onChangeChecks    ?: (checks: (string | number)[]) => void;   // Callback saat baris dicentang
  actionBulking     ?: ((checks: (string | number)[]) => ReactNode) | false; // Tombol aksi massal (bulk actions)
  onRowClick        ?: (data: Record<string, any>, key: number) => void;     // Callback klik baris
  noIndex           ?: boolean;                                 // Sembunyikan kolom nomor urut otomatis
}
```

---

## 2. Definisi Kolom (`TableColumnType`)

```typescript
export interface TableColumnType {
  selector    : string;                                         // Nama properti objek data
  label       : string | ReactNode;                             // Judul kolom di header
  width      ?: string;                                         // Lebar kolom (misal: "10%" atau "120px")
  sortable   ?: boolean;                                        // Izinkan pengurutan kolom ini
  item       ?: (data: any) => string | ReactNode;              // Custom cell renderer (JSX kustom)
}
```

---

## 3. Fitur Utama

### A. Seleksi Baris & Aksi Massal (`checks` & `actionBulking`)
Jika properti `checks` dan `onChangeChecks` disediakan, kolom pertama tabel otomatis berubah menjadi checkbox. Tombol aksi massal (`actionBulking`) akan muncul secara dinamis di atas tabel ketika ada baris yang dicentang.

```tsx
actionBulking={(selectedIds) => (
  <ButtonComponent
    label={`Hapus ${selectedIds.length} Data`}
    paint="danger"
    onClick={() => handleBulkDelete(selectedIds)}
  />
)}
```

### B. Pengurutan Kolom (`sortBy` & `onChangeSortBy`)
Kolom yang didefinisikan sebagai `sortable: true` akan menampilkan indikator panah pengurutan di samping judul kolom. Mengkliknya akan memicu callback `onChangeSortBy`.

---

## 4. Contoh Penggunaan

Berikut adalah contoh tabel sederhana menampilkan daftar pengguna:

```tsx
import React, { useState } from "react";
import { TableComponent } from "@components";

export function SimpleUserList() {
  const [sort, setSort] = useState(["created_at", "desc"]);

  const columns = [
    { selector: "name",  label: "Nama Pengguna", sortable: true },
    { selector: "email", label: "Alamat Email" },
    {
      selector: "role",
      label:    "Role",
      item:     (row) => <span className="badge">{row.role}</span>
    }
  ];

  const data = [
    { name: "Ahmad", email: "ahmad@mail.com", role: "Admin" },
    { name: "Budi",  email: "budi@mail.com",  role: "User" }
  ];

  return (
    <TableComponent
      columns={columns}
      data={data}
      sortBy={sort}
      onChangeSortBy={setSort}
      noIndex={false}
    />
  );
}
```
*Catatan untuk Agen: Gunakan `TableComponent` jika Anda membutuhkan kontrol tampilan tabel kustom secara manual. Jika membuat halaman manajemen data standar (CRUD), lebih disarankan menggunakan `TableSupervisionComponent` yang membungkus komponen ini dan otomatis menangani query data.*
