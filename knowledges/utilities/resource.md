# Panduan Utilitas: Sumber Data Bersatu (Resource) (`@utils`)

`useResource` adalah React hook terpadu di Skalfa App yang mengabstraksi pengambilan data daftar (list/table) baik dari **backend API** maupun dari **IndexedDB lokal** menggunakan antarmuka pemanggilan yang seragam.

## 1. Antarmuka Pemanggilan (`useResource`)

```typescript
import { useResource } from "@utils";

const { loading, data, reset } = useResource(props);
```

### Parameter Konfigurasi (`props`)
Mendukung dua mode pengambilan data berdasarkan properti yang dikirimkan:

```typescript
type UseResourceProps =
  | { path: string; url?: string; params?: ResourceParams } // Mode API
  | { idb: { store: string; schema?: DBSchema }; params?: ResourceParams }; // Mode IndexedDB
```

### Parameter Pencarian & Paginasi (`ResourceParams`)
```typescript
type ResourceParams = {
  page     ?: number;
  paginate ?: number;          // Limit data per halaman
  search   ?: string;          // Kata kunci pencarian global
  sort     ?: string[];        // Aturan pengurutan (contoh: ["created_at desc"])
  expand   ?: string[];        // Eager loading relasi (khusus API)
  filter   ?: any[];           // Filter kolom spesifik
};
```

---

## 2. Mode Operasi

### A. Mode API (Otomatis Aktif jika ada `path` atau `url`)
Menggunakan utilitas `useGetApi` di bawah kap untuk mengambil data dari server.
```typescript
const { loading, data, reset } = useResource({
  path: "bookings",
  params: {
    page:     1,
    paginate: 10,
    search:   "Budi",
    expand:   ["customer"],
  }
});

// Hasil 'data' berupa response API utuh: { data: Booking[], total: number }
```

### B. Mode IndexedDB (Otomatis Aktif jika ada `idb`)
Mengambil data dari penyimpanan lokal IndexedDB menggunakan query builder `@utils/idb`.
*   **Pencarian Otomatis**: Melakukan pencarian kata kunci (`search`) secara sensitif terhadap huruf besar/kecil (*case-insensitive*) di seluruh kolom baris data.
*   **Filter Otomatis**: Mendukung penyaringan kolom menggunakan format `{ field: string, value: any }` di dalam array `filter`.
*   **Urutan Otomatis**: Mengurutkan berdasarkan indeks kolom yang dikirim pada `sort` (contoh: `["created_at desc"]` atau `["id asc"]`).

```typescript
import { myDbSchema } from "@/schema/local-db";

const { loading, data, reset } = useResource({
  idb: {
    store:  "bookings",
    schema: myDbSchema
  },
  params: {
    page:     1,
    paginate: 10,
    search:   "Ahmad",
    filter:   [
      { field: "status", value: "pending" }
    ],
    sort:     ["created_at desc"]
  }
});

// Hasil 'data' disamakan dengan format API: { data: any[], total_row: number }
```

---

## 3. Nilai Kembalian (Return Value)

Hook ini mengembalikan objek seragam:
*   `loading`: `boolean` (status memuat data).
*   `data`: Objek data hasil query.
    *   *Pada Mode API*: Mengikuti struktur dari backend (biasanya `{ data: T[], total: number }`).
    *   *Pada Mode IDB*: Berupa `{ data: T[], total_row: number }`.
*   `reset`: `() => void` (Fungsi untuk memicu pemuatan ulang data/refresh).
```tsx
// Memanggil refresh data setelah aksi sukses:
<button onClick={reset}>Muat Ulang</button>
```
*Catatan untuk Agen: Gunakan `useResource` ketika membuat tabel atau daftar data kustom yang perlu mendukung peralihan sumber data antara API dan lokal secara dinamis.*
