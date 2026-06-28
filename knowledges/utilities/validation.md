# Panduan Utilitas: Validasi Formulir Frontend (Validation) (`@utils`)

`validation` adalah mesin validasi data mandiri di sisi frontend Skalfa App. Utilitas ini digunakan secara internal oleh komponen form (seperti `InputComponent` dan `FormSupervisionComponent`) untuk memvalidasi input secara real-time sebelum dikirimkan ke backend atau disimpan ke IndexedDB.

## 1. Cara Kerja `validation`

*   **Format Aturan**: Mendukung aturan validasi dalam bentuk array string (`string[]`) atau string tunggal dipisahkan pipa (`"required|email|min:8"`).
*   **Response**: Metode evaluasi mengembalikan objek hasil berupa `{ valid: boolean, message: string }`.

---

## 2. Daftar Aturan Validasi yang Didukung

*   `required`: Nilai tidak boleh kosong atau berupa array kosong.
*   `numeric`: Nilai harus berupa karakter angka.
*   `email`: Nilai harus berupa format email yang valid.
*   `url`: Nilai harus berupa format URL yang valid.
*   `date`: Nilai harus berupa tanggal yang valid.
*   `boolean`: Nilai harus bernilai boolean.
*   `min:N`: Panjang teks minimal `N` karakter (contoh: `min:8`).
*   `max:N`: Panjang teks maksimal `N` karakter (contoh: `max:50`).
*   `between:MIN,MAX`: Panjang teks harus antara `MIN` dan `MAX` karakter (contoh: `between:3,20`).
*   `in:VAL1,VAL2,...`: Nilai harus berupa salah satu dari opsi yang ditentukan (contoh: `in:admin,member`).
*   `not_in:VAL1,VAL2,...`: Nilai tidak boleh berupa salah satu dari opsi yang ditentukan.

---

## 3. Contoh Penggunaan Mandiri

Jika Anda menulis input kustom secara manual di luar `FormSupervisionComponent`, Anda dapat memvalidasi nilai input secara manual menggunakan utilitas ini:

```typescript
import { validation } from "@utils";

const checkEmail = validation.check({
  value: "budi@email",
  rules: "required|email"
});

if (!checkEmail.valid) {
  console.log("Validasi Gagal:", checkEmail.message); 
  // Output: "Format email tidak valid" (sesuai bahasa lokalisasi)
}
```

---

## 4. Lokalisasi Pesan Kesalahan (`validation.langs`)
Pesan kesalahan otomatis disajikan dalam Bahasa Indonesia secara default melalui berkas konfigurasi bahasa internal:
*   `required`: `"Wajib diisi"`
*   `email`: `"Format email tidak valid"`
*   `numeric`: `"Harus berupa angka"`
*   `min`: `"Minimal [min] karakter"`
*   `max`: `"Maksimal [max] karakter"`
*   `between`: `"Harus diantara [min] - [max] karakter"`
*   `in`: `"Harus salah satu dari [in]"`
```typescript
// Contoh pesan kesalahan dinamis:
// Jika aturan "min:8" dilanggar, pesan otomatis disesuaikan menjadi "Minimal 8 karakter"
```
*Catatan untuk Agen: Selalu daftarkan aturan validasi pada properti `validations` di setiap objek field di dalam `FormSupervisionComponent` agar validasi ini berjalan secara otomatis.*
