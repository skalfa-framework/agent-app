# Panduan Komponen: `FormSupervisionComponent` (`@components`)

`FormSupervisionComponent` adalah komponen form generator tingkat lanjut di Skalfa App. Komponen ini secara otomatis menangani rendering UI, manajemen state internal, validasi input, pemicuan loading state, konfirmasi sebelum submit, dan pengiriman data ke API atau IndexedDB.

## 1. Antarmuka Komponen (`formSupervisionProps`)

```typescript
interface formSupervisionProps {
  title          ?: string;
  fields          : FormType[];                                         // Daftar field di dalam form
  submitControl   : ApiType | { idb: { store: string, schema?: DBSchema }}; // Tujuan submit (API / IndexedDB)
  defaultValue   ?: object | null;                                      // Nilai awal form
  payload        ?: (values: any) => Promise<object> | object;          // Transformasi data sebelum dikirim
  confirmation   ?: boolean;                                            // Tampilkan dialog konfirmasi (Ya/Tidak)
  onSuccess      ?: (data: any) => void;                                // Callback sukses
  onError        ?: (code: number) => void;                             // Callback gagal (misal: 422, 500)
  footerControl  ?: ({ loading }: { loading: boolean }) => ReactNode;   // Custom tombol/footer
  className      ?: string;
}
```

---

## 2. Struktur Field (`FormType`)

Setiap item di dalam array `fields` dikonfigurasi sebagai berikut:

```typescript
interface FormType<T extends TypeKeys = keyof ConstructionMap> {
  col          ?: 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | string; // Lebar grid Tailwind (default: 12)
  type         ?: T;                                                      // Tipe input (lihat tabel di bawah)
  construction ?: ConstructionMap[T];                                     // Properti/konfigurasi input
  className    ?: string;
  onHide       ?: (values: any) => boolean;                               // Sembunyikan field secara dinamis
  watch        ?: (ctx: WatchContext) => WatchAction | undefined;          // Reaksi dinamis terhadap perubahan field lain
}
```

### Daftar Tipe Input (`type`):
| Nilai `type` | Deskripsi | Properti `construction` |
|--------------|-----------|-------------------------|
| `default` (default)| Input teks standar (text, email, dsb) | `InputProps` (name, label, placeholder, validations) |
| `select`     | Dropdown pilihan | `SelectProps` (options: `Array<{ value, label }>`, dll) |
| `number`     | Input angka | `InputNumberProps` |
| `currency`   | Input nominal mata uang Rupiah | `InputCurrencyProps` |
| `date`       | Pilihan tanggal (datepicker) | `InputDateProps` |
| `datetime`   | Pilihan tanggal & waktu | `InputDateTimeProps` |
| `time`       | Pilihan waktu | `InputTimeProps` |
| `enter-password`| Input kata sandi | `InputPasswordProps` |
| `check`      | Checkbox | `InputCheckboxProps` |
| `radio`      | Pilihan radio | `InputRadioProps` |
| `image`      | Upload gambar | `InputImageProps` |
| `map`        | Pilihan koordinat peta | `InputMapProps` |
| `cluster`    | Repeater / Form dinamis (array data) | `ClusterConstruction` |
| `custom`     | Render kustom manual menggunakan fungsi | `formCustomConstructionProps` |

---

## 3. Logika Dinamis (`watch` & `onHide`)

### A. Sembunyikan Field (`onHide`)
Menyembunyikan field berdasarkan nilai field lainnya.
```typescript
{
  col: 6,
  type: "default",
  construction: { name: "npwp", label: "Nomor NPWP" },
  // Sembunyikan jika status pernikahan bukan 'menikah'
  onHide: (values) => values.status_pernikahan !== "menikah"
}
```

### B. Memantau Perubahan (`watch`)
Mengubah properti field (seperti `disabled`, `required`, `value`, `hidden`, `readonly`) secara dinamis saat nilai field lain berubah.
```typescript
{
  col: 6,
  type: "default",
  construction: { name: "alamat_domisili", label: "Alamat Domisili" },
  watch: ({ values, self, prev }) => {
    // Jika checkbox "alamat sesuai ktp" dicentang, isi otomatis dan buat readonly
    if (values.alamat_sesuai_ktp) {
      return {
        value:    values.alamat_ktp,
        readonly: true
      };
    }
    return { readonly: false };
  }
}
```

---

## 4. Contoh Implementasi Lengkap

Berikut adalah contoh form Login sederhana:

```tsx
import React from "react";
import { FormSupervisionComponent } from "@components";
import { faUser, faLock } from "@fortawesome/free-solid-svg-icons";

export function LoginForm({ onSuccess }: { onSuccess: (data: any) => void }) {
  return (
    <FormSupervisionComponent
      title="Masuk ke Akun Anda"
      confirmation={false}
      submitControl={{
        path:   "auth/login",
        method: "POST"
      }}
      onSuccess={onSuccess}
      fields={[
        {
          col:  12,
          type: "default",
          construction: {
            name:        "username",
            label:       "Username",
            placeholder: "Masukkan username Anda",
            leftIcon:    faUser,
            validations: {
              required:  [true, "Username wajib diisi"]
            }
          }
        },
        {
          col:  12,
          type: "enter-password",
          construction: {
            name:        "password",
            label:       "Password",
            placeholder: "Masukkan password Anda",
            leftIcon:    faLock,
            validations: {
              required:  [true, "Password wajib diisi"]
            }
          }
        }
      ]}
    />
  );
}
```
Untuk kustomisasi tingkat lanjut seperti repeater (cluster) atau render manual (custom), rujuk langsung ke kode sumber [FormSupervisionComponent](file:///d:/_skalfa/skalfa-app/components/base.components/supervision/FormSupervision.component.tsx).
