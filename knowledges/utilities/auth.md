# Panduan Utilitas: Autentikasi (Auth) (`@utils`)

Dokumen ini menjelaskan penggunaan utilitas `auth` untuk mengelola sesi masuk pengguna dan token otorisasi di Skalfa App.

## 1. Sesi & Cookie Token

*   **Penyimpanan**: Token otorisasi disimpan dalam cookie aman (`secure: true`) dengan masa aktif default 7 hari.
*   **Nama Cookie**: Otomatis dibuat secara dinamis menggunakan slug nama aplikasi Next.js (ditambah akhiran `.user.token`).

---

## 2. Metode yang Tersedia

### A. Mengambil Token Otorisasi (`auth.getAccessToken`)
Mengambil token aktif dari cookie (otomatis di-dekripsi).
```typescript
import { auth } from "@utils";

const token = auth.getAccessToken();
if (token) {
  console.log("User sedang masuk:", token);
}
```

### B. Menyimpan Token Otorisasi (`auth.setAccessToken`)
Menyimpan token baru hasil login ke dalam cookie (otomatis di-enkripsi).
```typescript
// Format: auth.setAccessToken(token, expiredInDays?)
auth.setAccessToken("my-jwt-token-from-backend", 7);
```

### C. Menghapus Sesi / Keluar (`auth.deleteAccessToken`)
Menghapus cookie token (digunakan saat logout).
```typescript
auth.deleteAccessToken();
```

### D. Mengamankan Rute Halaman (`auth.check`)
Memeriksa apakah token ada. Jika tidak ada token aktif, otomatis memicu pengalihan (*redirect*) ke halaman login `/auth/login`.
```typescript
import { auth } from "@utils";

// Jalankan ini di tingkat atas halaman atau layout yang butuh proteksi login
auth.check();
```

---

## 3. Contoh Penggunaan dalam Layanan Login

```typescript
// app/auth/_services/login.service.ts
import { api, auth } from "@utils";

export async function loginUser(payload: any) {
  const response = await api({
    path:    "auth/login",
    method:  "POST",
    payload: payload
  });

  if (response?.status === 200 && response.data?.token) {
    // Simpan token ke cookie
    auth.setAccessToken(response.data.token);
    return { success: true };
  }

  return { success: false };
}
```
*Catatan untuk Agen: Gunakan `auth.check()` di bagian paling atas berkas halaman (`page.tsx`) atau layout untuk halaman-halaman yang membutuhkan hak akses login (seperti dashboard).*
