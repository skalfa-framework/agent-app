# Panduan Komponen: `TableSupervisionComponent` (`@components`)

`TableSupervisionComponent` adalah komponen tabel data (datatable) tingkat lanjut di Skalfa App. Komponen ini mengotomatisasi pemuatan data dari API/IndexedDB, paginasi, pencarian, pengurutan, filter kolom, ekspor/impor Excel, serta modal aksi CRUD (Create, Read, Update, Delete) secara instan.

## 1. Antarmuka Komponen (`TableSupervisionProps`)

```typescript
type TableSupervisionProps = {
  fetchControl          : UseResourceProps;                                     // Sumber data (API/IndexedDB)
  title                ?: string;                                               // Judul tabel/halaman
  id                   ?: string;                                               // ID unik tabel
  columnControl        ?: string[] | TableSupervisionColumnProps[];             // Konfigurasi kolom
  formControl          ?: TableSupervisionFormProps;                            // Konfigurasi form tambah/edit data
  actionControl        ?: boolean | ('EDIT' | 'DELETE' | CustomAction)[];       // Aksi pada setiap baris data
  controlBar           ?: ("CREATE" | "IMPORT" | "EXPORT" | "PRINT")[];         // Tombol aksi di atas tabel
  onRowClick           ?: (data: Record<string, any>) => void;                  // Event klik pada baris
  noIndex              ?: boolean;                                              // Sembunyikan kolom nomor urut
};
```

### Parameter Sumber Data (`fetchControl`)
Menentukan dari mana data diambil:
```typescript
// Contoh mengambil dari API:
fetchControl={{ path: "bookings" }}

// Contoh mengambil dari IndexedDB:
fetchControl={{ idb: { store: "bookings", schema: myDbSchema } }}
```

---

## 2. Konfigurasi Kolom (`TableSupervisionColumnProps`)

Setiap item dalam `columnControl` mendefinisikan kolom tabel:

```typescript
interface TableSupervisionColumnProps {
  selector     : string;                                          // Nama field data (misal: "customer_name")
  label       ?: string;                                          // Label judul kolom di header
  width       ?: string;                                          // Lebar kolom (misal: "150px" atau "20%")
  sortable    ?: boolean;                                         // Izinkan pengurutan kolom
  searchable  ?: boolean;                                         // Izinkan pencarian teks pada kolom ini
  item        ?: (data: any) => string | ReactNode;               // Custom cell renderer (JSX kustom)
  filterable  ?: boolean | FilterConfig;                          // Konfigurasi filter kolom
}
```

### Contoh Kustomisasi Cell (`item`):
```typescript
{
  selector: "status",
  label:    "Status",
  item:     (row) => <StatusBadge status={row.status} />
}
```

---

## 3. Konfigurasi Form CRUD (`formControl` & `actionControl`)

Jika `formControl` dan `actionControl` diisi, `TableSupervisionComponent` akan otomatis menangani pembuatan tombol **Tambah (Create)**, tombol **Edit**, dan tombol **Hapus (Delete)** beserta modal form pengisiannya.

```typescript
export interface TableSupervisionFormProps {
  fields        : (FormType & { visibility?: "*" | "create" | "update" })[]; // Kolom form (mendukung tipe FormSupervision)
  defaultValue ?: (item: Record<string, any> | null) => Promise<any> | any;  // Mengisi data awal saat Edit
  payload      ?: (values: any) => Promise<any> | object;                    // Mengubah payload sebelum dikirim
}
```

### Contoh Pemicu Tombol Aksi:
```typescript
controlBar:    ["CREATE"], // Memunculkan tombol "Tambah" di baris atas tabel
actionControl: ["EDIT", "DELETE"] // Memunculkan tombol Edit (ikon pensil) dan Hapus (ikon tong sampah) di setiap baris
```

---

## 4. Contoh Implementasi Lengkap

Berikut adalah contoh tabel manajemen pengguna (User Management) dengan fitur pencarian, filter, dan CRUD lengkap:

```tsx
"use client"
import React from "react";
import { TableSupervisionComponent } from "@components";
import { faEnvelope, faUser } from "@fortawesome/free-solid-svg-icons";

export function UserTable() {
  return (
    <TableSupervisionComponent
      title="Manajemen Pengguna"
      fetchControl={{ path: "users" }}
      controlBar={["CREATE", "EXPORT"]}
      actionControl={["EDIT", "DELETE"]}
      columnControl={[
        {
          selector:   "username",
          label:      "Username",
          sortable:   true,
          searchable: true,
        },
        {
          selector:   "email",
          label:      "Email",
          sortable:   true,
          searchable: true,
        },
        {
          selector:   "created_at",
          label:      "Tanggal Registrasi",
          sortable:   true,
        }
      ]}
      formControl={{
        fields: [
          {
            col:  12,
            type: "default",
            construction: {
              name:        "username",
              label:       "Username",
              placeholder: "Masukkan username",
              leftIcon:    faUser,
              validations: { required: [true, "Username wajib diisi"] }
            }
          },
          {
            col:  12,
            type: "default",
            construction: {
              name:        "email",
              label:       "Email",
              placeholder: "Masukkan email",
              leftIcon:    faEnvelope,
              validations: { required: [true, "Email wajib diisi"] }
            }
          }
        ]
      }}
    />
  );
}
```
Rujuk langsung ke kode sumber [TableSupervisionComponent](file:///d:/_skalfa/skalfa-app/components/base.components/supervision/TableSupervision.component.tsx) jika membutuhkan integrasi tombol aksi kustom, pintasan keyboard (*shortcuts*), atau perilaku penangan impor data kustom.
