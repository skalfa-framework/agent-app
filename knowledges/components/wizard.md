# Panduan Komponen: Penunjuk Langkah (Wizard) (`@components`)

`WizardComponent` adalah komponen indikator langkah (stepper/process bar) di Skalfa App yang digunakan untuk menampilkan visualisasi alur proses berurutan (multi-step process) seperti proses pendaftaran bertahap atau form pemesanan multi-halaman.

## 1. Antarmuka Komponen (`WizardProps`)

```typescript
export interface WizardProps {
  items  : { label: string; circle_content: ReactNode }[]; // Daftar langkah (label & simbol lingkaran)
  active : number;                                          // Indeks langkah aktif saat ini (0-indexed)
}
```

---

## 2. Fitur Utama

*   **Garis Progres Otomatis**: Komponen menggambar garis penghubung antar lingkaran langkah secara otomatis. Garis akan berwarna primer (`bg-primary`) untuk langkah yang telah dilalui, gradasi berwarna (`bg-gradient-to-r`) untuk langkah transisi aktif berikutnya, dan abu-abu (`bg-light-primary`) untuk langkah yang belum dilalui.
*   **Simbol Lingkaran Kustom (`circle_content`)**: Lingkaran langkah dapat diisi angka biasa, teks pendek, atau komponen ikon kustom (misal: FontAwesomeIcon).
*   **Penyelarasan Lebar Dinamis**: Lebar wadah setiap langkah dibagi rata secara matematis sesuai jumlah item menggunakan rumus `calc(100% * 1 / jumlah_item)` untuk menjamin visual yang rapi di berbagai ukuran layar.

---

## 3. Contoh Penggunaan

Berikut adalah contoh visualisasi proses pemesanan hotel 3 langkah:

```tsx
import React from "react";
import { WizardComponent } from "@components";
import { faCheck, faUser, faCreditCard, faClipboardCheck } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";

export function BookingProcess({ currentStep }: { currentStep: number }) {
  return (
    <WizardComponent
      active={currentStep} // Nilai dinamis (0, 1, atau 2)
      items={[
        {
          label:          "Identitas",
          circle_content: currentStep > 0 ? <FontAwesomeIcon icon={faCheck} /> : <FontAwesomeIcon icon={faUser} />
        },
        {
          label:          "Pembayaran",
          circle_content: currentStep > 1 ? <FontAwesomeIcon icon={faCheck} /> : <FontAwesomeIcon icon={faCreditCard} />
        },
        {
          label:          "Konfirmasi",
          circle_content: <FontAwesomeIcon icon={faClipboardCheck} />
        }
      ]}
    />
  );
}
```
*Catatan untuk Agen: Gunakan `WizardComponent` hanya sebagai komponen indikator alur visual. Manajemen state data antar langkah dan tombol navigasi (Selanjutnya/Kembali) dikelola secara terpisah di dalam berkas kustom hook layanan Anda.*
