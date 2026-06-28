# Panduan Komponen: `ModalConfirmComponent` (`@components`)

`ModalConfirmComponent` adalah komponen dialog modal untuk konfirmasi tindakan penting (seperti menghapus data, memicu aksi API asinkron, atau menulis ke database lokal) di Skalfa App.

## 1. Antarmuka Komponen (`ModalConfirmProps`)

```typescript
export interface ModalConfirmProps {
  show            : boolean;                                     // Menampilkan modal (true/false)
  onClose         : () => void;                                  // Event saat modal ditutup
  title          ?: string | ReactNode;                          // Judul konfirmasi
  children       ?: any;                                         // Konten/deskripsi di dalam modal
  icon           ?: any;                                         // Ikon FontAwesome (default: faQuestion)
  footer         ?: string | ReactNode;                          // Kustom tombol footer
  submitControl  ?: ButtonProps & {                              // Kontrol tombol Submit otomatis
    onSubmit     ?: ApiType | SubmitIDB | (() => void);          // Aksi saat submit diklik
    onSuccess    ?: () => void;                                  // Callback sukses
    onError      ?: () => void;                                  // Callback gagal
  };
  className      ?: string;
}
```

---

## 2. Fitur Utama

### A. Integrasi Aksi Otomatis (`submitControl`)
Modal ini dapat menangani pemanggilan API atau penyimpanan IndexedDB secara mandiri saat tombol konfirmasi diklik.
*   **Pemicu API**: Jika `onSubmit` berupa `ApiType`, modal akan memicu loading spinner, memanggil API, dan memicu toast sukses/gagal.
*   **Pemicu IndexedDB**: Jika `onSubmit` berupa `{ idb: { store, id } }`, modal akan menghapus data di store IndexedDB tersebut.

### B. Navigasi Pintasan Keyboard (`escape`)
Secara otomatis, ketika modal ditampilkan (`show === true`), utilitas pintasan mendaftarkan tombol `escape` untuk menutup modal. Pintasan ini otomatis di-unregister saat modal ditutup.

---

## 3. Contoh Implementasi

### A. Konfirmasi Hapus Data dengan API
```tsx
import React from "react";
import { ModalConfirmComponent } from "@components";
import { faTrash } from "@fortawesome/free-solid-svg-icons";

export function DeleteConfirmation({ show, id, onClose, onDeleteSuccess }) {
  return (
    <ModalConfirmComponent
      show={show}
      onClose={onClose}
      title="Hapus Data Transaksi?"
      icon={faTrash}
      submitControl={{
        label:     "Ya, Hapus",
        paint:     "danger",
        onSubmit:  {
          path:   `bookings/${id}`,
          method: "DELETE"
        },
        onSuccess: () => {
          onDeleteSuccess();
          onClose();
        }
      }}
    >
      <p className="text-center text-slate-500 text-sm">
        Tindakan ini tidak dapat dibatalkan. Apakah Anda yakin ingin menghapus data ini?
      </p>
    </ModalConfirmComponent>
  );
}
```

### B. Konfirmasi dengan Aksi Kustom (Callback Fungsi)
```tsx
<ModalConfirmComponent
  show={show}
  onClose={onClose}
  title="Konfirmasi Pembayaran?"
  submitControl={{
    label:    "Konfirmasi",
    onSubmit: () => {
      // Jalankan fungsi kustom Anda
      handleApprovePayment();
    }
  }}
>
  <p className="text-center text-sm">Apakah Anda ingin menyetujui pembayaran ini?</p>
</ModalConfirmComponent>
```
*Catatan untuk Agen: Selalu gunakan `ModalConfirmComponent` untuk setiap aksi destruktif (seperti menghapus atau membatalkan data) demi menjaga pengalaman pengguna yang aman.*
