# Peta Fitur & Akun Uji Coba (Frontend)

Berkas ini memetakan seluruh rute halaman, fitur, dan akun uji coba yang tersedia pada Skalfa App agar mudah dipahami oleh manusia maupun agen.

## 1. Akun Uji Coba (Test Credentials)

Gunakan akun berikut untuk melakukan simulasi masuk (login) pada browser terintegrasi saat melakukan pengujian runtime:

| No  | Username / Email | Password   | Role / Hak Akses | Keterangan               |
| -----| ------------------| ------------| ------------------| --------------------------|
| 1   | `admin`          | `password` | Administrator    | Akses penuh ke dashboard |
| 2   | `user`           | `password` | Customer         | Untuk simulasi pemesanan |

*(Akun uji coba tambahan dapat ditambahkan di atas sesuai kebutuhan)*

---

## 2. Peta Halaman & Fitur

| Nama Fitur | URL Path (Rute) | Berkas Halaman Utama     | Keterangan                 |
| ------------| -----------------| --------------------------| ----------------------------|
| Login      | `/login`        | `app/login/page.tsx`     | Halaman masuk aplikasi     |
| Dashboard  | `/dashboard`    | `app/dashboard/page.tsx` | Ringkasan statistik & menu |

*(Daftar halaman dan fitur baru wajib didaftarkan di atas oleh agen setiap kali selesai membuat halaman baru)*
