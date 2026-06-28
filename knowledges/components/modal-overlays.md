# Panduan Komponen: Overlay & Dialog (Modal, BottomSheet, Toast) (`@components`)

Dokumen ini menjelaskan penggunaan komponen overlay interaktif (`FloatingPageComponent`, `BottomSheetComponent`, dan `ToastComponent`) untuk berinteraksi dengan pengguna di Skalfa App.

## 1. Halaman Melayang (`FloatingPageComponent`)

`FloatingPageComponent` adalah panel laci (drawer) yang meluncur dari samping layar (biasanya dari sisi kanan). Komponen ini sangat cocok digunakan untuk menampilkan form detail, form tambah/edit data, atau pengaturan khusus tanpa meninggalkan halaman utama.

### Antarmuka Komponen (`FloatingPageProps`)
```typescript
interface FloatingPageProps {
  show      : boolean;                  // Menampilkan panel (true/false)
  onClose   : () => void;               // Callback saat panel ditutup
  title    ?: string | ReactNode;       // Judul panel di header
  children ?: any;                      // Konten utama di dalam panel
  tip      ?: string | ReactNode;       // Teks bantuan kecil di bawah header
  footer   ?: string | ReactNode;       // Tombol aksi di bagian bawah panel
  className?: string;                   // Kustom class Tailwind
}
```

### Fitur Keamanan & Interaksi:
*   **Pencegahan Scroll**: Saat panel aktif (`show === true`), scrollbar body halaman utama otomatis dimatikan (`overflow = "hidden"`).
*   **Pintasan Keyboard**: Otomatis mendaftarkan tombol `escape` untuk menutup panel dengan aman.
*   **Tombol Silang Bawaan**: Header otomatis menyediakan tombol silang (`faTimes`) di sisi kanan atas untuk menutup panel.

### Contoh Penggunaan:
```tsx
import React from "react";
import { FloatingPageComponent, ButtonComponent } from "@components";

export function DetailPanel({ show, data, onClose }) {
  return (
    <FloatingPageComponent
      show={show}
      onClose={onClose}
      title="Detail Transaksi"
      tip={`ID Transaksi: ${data?.id}`}
      footer={
        <div className="flex justify-end gap-2">
          <ButtonComponent label="Tutup" variant="outline" onClick={onClose} />
          <ButtonComponent label="Cetak PDF" paint="success" />
        </div>
      }
    >
      <div className="space-y-3 py-4">
        <p><strong>Pelanggan:</strong> {data?.customer_name}</p>
        <p><strong>Total:</strong> {data?.total_price}</p>
      </div>
    </FloatingPageComponent>
  );
}
```

---

## 2. Lembar Bawah (`BottomSheetComponent`)

`BottomSheetComponent` adalah panel modal yang meluncur dari bagian bawah layar. Komponen ini dirancang khusus dengan optimasi gestur sentuh (mobile-friendly), sangat cocok untuk antarmuka perangkat seluler.

### Antarmuka Komponen (`BottomSheetProps`)
```typescript
interface BottomSheetProps {
  show       : boolean;
  children   : ReactNode;
  onClose    : () => void;
  size      ?: string | number;         // Tinggi default sheet (contoh: 500, "50vh", "400px")
  maxSize   ?: string | number;         // Tinggi maksimal saat ditarik ke atas (expand)
  footer    ?: ReactNode;
  className ?: string;
}
```

### Fitur Utama:
*   **Swipe/Drag Gestur**: Pengguna dapat menutup panel dengan cara menarik (swipe down) area pegangan atas sheet.
*   **Tinggi Dinamis**: Mendukung tinggi berbasis piksel (`px`) maupun tinggi viewport (`vh`).

### Contoh Penggunaan:
```tsx
import { BottomSheetComponent } from "@components";

<BottomSheetComponent
  show={showOption}
  onClose={() => setShowOption(false)}
  size="40vh"
  maxSize="60vh"
>
  <div className="p-4 text-center">
    <h4 className="font-semibold mb-4">Pilih Opsi Tindakan</h4>
    <button className="w-full py-3 border-b text-sm">Bagikan Rujukan</button>
    <button className="w-full py-3 text-sm text-danger">Laporkan Masalah</button>
  </div>
</BottomSheetComponent>
```

---

## 3. Pesan Toast (`ToastComponent`)

`ToastComponent` adalah kotak notifikasi popup kecil yang muncul sementara (biasanya di pojok layar) untuk memberikan umpan balik instan kepada pengguna (misal: "Data berhasil disimpan").

### Antarmuka Komponen (`ToastProps`)
```typescript
interface ToastProps {
  show      : boolean;
  onClose   : () => void;               // Fungsi untuk menyembunyikan toast
  title    ?: string | ReactNode;       // Judul notifikasi
  children ?: any;                      // Detail pesan
  footer   ?: string | ReactNode;
  className?: string;
}
```

### Fitur Utama:
*   **Penutupan Otomatis (Auto-Dismiss)**: Ketika `show` bernilai `true`, hitung mundur 5 detik dimulai secara otomatis. Setelah waktu habis, fungsi `onClose` akan dipicu untuk menutup toast.
*   **Jeda saat Hover (Pause on Hover)**: Jika pengguna mengarahkan kursor mouse di atas toast (`mouseenter`), hitung mundur otomatis dihentikan sementara, dan akan dilanjutkan kembali saat mouse keluar (`mouseleave`).

### Contoh Penggunaan:
```tsx
import { ToastComponent } from "@components";

<ToastComponent
  show={showToast}
  onClose={() => setShowToast(false)}
  title="Aksi Sukses"
>
  <p className="text-sm text-slate-600">Perubahan data profil Anda berhasil disimpan.</p>
</ToastComponent>
```
*Catatan untuk Agen: Selalu gunakan `ToastComponent` untuk mengonfirmasi keberhasilan aksi asinkron penting kepada pengguna.*
