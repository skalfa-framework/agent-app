# Panduan Utilitas: Hook Formulir & Tabel (`useForm`, `useTable`) (`@utils`)

Dokumen ini menjelaskan penggunaan kustom hook tingkat lanjut `useForm` dan `useTable` yang mengelola state formulir dan tabel secara mendalam di Skalfa App. Hook ini digunakan di bawah kap oleh komponen `Supervision`, namun dapat digunakan secara mandiri untuk kontrol penuh.

## 1. State Formulir (`useForm`)

`useForm` mengelola seluruh siklus hidup data formulir, mulai dari pendaftaran field, sinkronisasi nilai input, validasi waktu-nyata, hingga pengiriman data otomatis ke API atau IndexedDB.

### Antarmuka Penggunaan
```typescript
import { useForm } from "@utils";

const {
  values,             // Nilai form aktif: Array<{ name, value }>
  setValues,          // Mengisi banyak nilai sekaligus
  setValue,           // Mengisi satu nilai field
  errors,             // Daftar error aktif: Array<{ name, error }>
  setErrors,
  setRegister,        // Mendaftarkan field beserta aturan validasinya
  unregister,         // Menghapus pendaftaran field
  unregisterPrefix,   // Menghapus field berprefiks tertentu (sangat berguna untuk group/repeater)
  submit,             // Memicu aksi pengiriman form
  loading,            // Status loading pengiriman data
  confirm,            // Status modal konfirmasi
} = useForm({
  path          : "bookings",                       // Alamat API tujuan (jika submit ke API)
  method        : "POST",                           // Metode HTTP (POST / PUT / PATCH)
  idb           : { store: "bookings" },            // Atau tentukan store IndexedDB (jika simpan lokal)
  payload       : (vals) => transform(vals),        // Callback transformasi data sebelum dikirim
  confirmation  : true,                             // Tampilkan dialog konfirmasi
  onSuccess     : (data) => handleSuccess(data),    // Callback saat berhasil
  onFailed      : (code) => handleFailed(code),     // Callback saat gagal (contoh: 422)
});
```

---

## 2. State Tabel & URL Sync (`useTable`)

`useTable` mengelola state tabel (halaman aktif, jumlah baris, kata kunci pencarian, filter kolom, dan pengurutan) serta **sinkronisasi otomatis ke URL search parameters**.

### Keunggulan Utama (Penyimpanan Status di URL):
Setiap kali status tabel berubah (misal pindah halaman atau mengetik pencarian), `useTable` otomatis memperbarui parameter URL. Hal ini memungkinkan pengguna menyalin link URL dan membagikannya ke orang lain dengan status pencarian/paginasi yang sama persis.

### Pilihan Kompresi (`compressed`):
Mendukung kompresi status menggunakan algoritma **LZString** agar URL tetap pendek meskipun terdapat filter kolom yang sangat kompleks.

### Antarmuka Penggunaan
```typescript
import { useTable } from "@utils";

const {
  tableKey,      // Kunci unik tabel
  data,          // Data hasil query: { data: T[], total: number } atau { data: T[], total_row: number }
  selected,      // Baris data yang sedang dipilih
  setSelected,
  checks,        // ID baris yang sedang dicentang (untuk bulk actions)
  setChecks,
  reset,         // Memicu pemuatan ulang data (refresh)
  focus,         // Indeks baris yang sedang difokuskan (untuk keyboard navigation)
  setFocus,
} = useTable(
  fetchControl,  // Objek { path } untuk API atau { idb } untuk IndexedDB
  id,            // ID unik tabel (opsional)
  title,         // Judul tabel (opsional, digunakan sebagai kunci URL)
  urlParam       // boolean atau konfigurasi kompresi: { compressed: boolean }
);
```

### Contoh Integrasi Kustom (Tabel Mandiri):
```tsx
import React from "react";
import { useTable, ButtonComponent } from "@utils";
import { TableComponent } from "@components";

export function CustomUserPage() {
  // Mengambil data dari API 'users' dan mensinkronisasikan ke URL
  const { data, loading, reset } = useTable(
    { path: "users" },
    "user-table",
    "Daftar Pengguna",
    true // Aktifkan sinkronisasi URL tanpa kompresi
  );

  return (
    <div className="space-y-4">
      <div className="flex justify-between">
        <h3>Manajemen Pengguna</h3>
        <ButtonComponent label="Refresh" onClick={reset} loading={loading} />
      </div>

      <TableComponent
        columns={[
          { selector: "username", label: "Username" },
          { selector: "email",    label: "Email" }
        ]}
        data={data?.data || []}
        loading={loading}
      />
    </div>
  );
}
```
*Catatan untuk Agen: Gunakan `useForm` dan `useTable` secara langsung jika Anda memerlukan penanganan form atau tabel yang sangat kustom yang tidak dapat diakomodasi oleh komponen `FormSupervisionComponent` atau `TableSupervisionComponent` standar.*
