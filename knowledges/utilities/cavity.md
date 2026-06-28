# Panduan Utilitas: Mesin Cache Lokal (Cavity) (`@utils`)

`cavity` adalah utilitas mesin penyimpanan cache lokal di browser berbasis IndexedDB. Utilitas ini digunakan secara internal oleh `useGetApi` untuk menyimpan data sementara agar aplikasi dapat berjalan sangat cepat dan mengurangi beban request ke server.

## 1. Cara Kerja `cavity`

*   **Penyimpanan**: Menyimpan objek data di dalam database IndexedDB dengan nama `<nama-aplikasi>.cavity` pada object store bernama `cache`.
*   **Waktu Kadaluwarsa (Expiration)**: Masa aktif cache ditentukan dalam satuan **menit** saat data disimpan.
*   **Pembersihan Otomatis**: Ketika aplikasi meminta data menggunakan `cavity.get(key)`, mesin akan memeriksa waktu kadaluwarsa. Jika data telah kadaluwarsa, data tersebut otomatis dihapus dari database lokal dan mengembalikan status kosong.

---

## 2. Metode yang Tersedia

### A. Menyimpan Data ke Cache (`cavity.set`)
Menyimpan data dengan menentukan masa aktif (dalam menit).

```typescript
import { cavity } from "@utils";

await cavity.set({
  key:     "active_announcements",
  data:    [ { id: 1, text: "Maintenance besok!" } ],
  expired: 60 // Masa aktif 60 menit (1 jam)
});
```

### B. Mengambil Data dari Cache (`cavity.get`)
Mengambil data yang masih aktif. Mengembalikan data langsung jika masih valid, atau mengembalikan struktur kosong jika kadaluwarsa atau tidak ditemukan.

```typescript
import { cavity } from "@utils";

const cacheData = await cavity.get("active_announcements");

if (cacheData && !("message" in cacheData)) {
  // Data cache valid ditemukan
  console.log("Data Cache:", cacheData);
} else {
  // Cache kosong atau kadaluwarsa
  console.log("Cache tidak ditemukan atau sudah kadaluwarsa");
}
```

### C. Menghapus Data Cache (`cavity.delete`)
Menghapus kunci cache secara manual (misal: setelah melakukan update data agar cache diperbarui).

```typescript
import { cavity } from "@utils";

await cavity.delete("active_announcements");
```

---

## 3. Integrasi dalam API Fetching (`useGetApi`)
Utilitas ini biasanya tidak dipanggil secara langsung melainkan diintegrasikan ke dalam `useGetApi` untuk mengotomatiskan caching request `GET`:

```typescript
// Meminta data dari API dengan caching selama 30 menit
const { data, loading } = useGetApi({
  path:      "announcements",
  expired:   30, // 30 menit cache
  cacheName: "announcements_list" // Kunci cache kustom
});
```
*Catatan untuk Agen: Gunakan `cavity` untuk mengoptimalkan performa halaman dashboard atau daftar data referensi statis yang jarang berubah.*
