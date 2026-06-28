# Panduan Utilitas: API (`@utils`)

Dokumen ini menjelaskan penggunaan utilitas API untuk melakukan komunikasi data HTTP ke backend Skalfa API.

## 1. Fungsi Utama: `api` (Asynchronous Fetcher)

Digunakan untuk melakukan HTTP request manual (seperti `POST`, `PUT`, `PATCH`, `DELETE`, atau `GET` sekali jalan).

### Tipe Parameter (`ApiType`)
```typescript
type ApiType = {
  path          ?: string;                  // Sub-path API (misal: "bookings")
  url           ?: string;                  // URL penuh jika menembak API luar
  method        ?: "GET" | "POST" | "PUT" | "PATCH" | "DELETE";
  params        ?: ApiParamsType;           // Parameter query khusus Skalfa
  payload       ?: any;                     // Body request (otomatis berupa FormData / multipart)
  includeParams ?: Record<string, any>;     // Query parameter tambahan
  headers       ?: Record<string, any>;     // Custom headers
  bearer        ?: string;                  // Token otorisasi kustom (default: token user login)
};
```

### Parameter Query Khusus (`ApiParamsType`)
Skalfa API mendukung standarisasi query parameter berikut:
```typescript
type ApiParamsType = {
  page             ?: number;
  paginate         ?: number;
  sort             ?: string[];             // Contoh: ["created_at", "-id"]
  search           ?: string;
  searchable       ?: string[];
  selectable       ?: string[];
  expand           ?: string[];             // Eager load relasi (contoh: ["user"])
  selectableOption ?: string[];
  filter           ?: ApiFilterType[];
};

type ApiFilterType = {
  column  ?: string;
  type    ?: "eq" | "ne" | "in" | "ni" | "bw" | ""; // eq=Equal, ne=NotEqual, in=In, ni=NotIn, bw=Between
  value   ?: string | number | number[] | string[] | null;
  logic   ?: "and" | "or";
};
```

### Contoh Penggunaan `api` dalam Service
```typescript
import { api } from "@utils";

export async function createBooking(payload: any) {
  const response = await api({
    path:    "bookings",
    method:  "POST",
    payload: payload, // Dikirim sebagai multipart/form-data secara default
  });

  return response?.data;
}
```

---

## 2. React Hook: `useGetApi` (Data Fetching Hook)

Digunakan untuk melakukan `GET` request di dalam komponen dengan penanganan state `loading`, `code`, `data`, dan `reset` (revalidation) secara otomatis.

### Parameter Hook
```typescript
useGetApi(
  props: ApiType & { 
    method?: "GET"; 
    cacheName?: string; 
    expired?: number; // Waktu kedaluwarsa cache dalam milidetik (jika menggunakan cache)
  }, 
  sleep?: boolean // Jika true, hook tidak akan melakukan fetch otomatis saat render pertama
)
```

### Nilai Kembalian (Return Value)
Mengembalikan objek berupa:
*   `loading`: `boolean` (status memuat data)
*   `code`: `number | null` (HTTP Status Code)
*   `data`: `any | null` (Response body dari API)
*   `reset`: `() => void` (Fungsi untuk memicu fetch ulang data / revalidate)

### Contoh Penggunaan `useGetApi` di Service (Custom Hook)
Sesuai aturan pemisahan logika, panggil `useGetApi` di dalam custom hook layanan:

```typescript
// app/booking/_services/booking.service.ts
import { useGetApi } from "@utils";

export function useBookingList(page: number, search: string) {
  const { loading, code, data, reset } = useGetApi({
    path: "bookings",
    params: {
      page:     page,
      paginate: 10,
      search:   search,
      expand:   ["customer", "unit"],
    }
  });

  return {
    bookings:   data?.data || [],
    totalData:  data?.total || 0,
    loading:    loading,
    refresh:    reset,
  };
}
```

### Pengecualian Pemanggilan Langsung di Komponen
Jika hanya membutuhkan pemanggilan API sederhana (tanpa logika bisnis tambahan), Anda diperbolehkan memanggil `useGetApi` secara langsung di berkas halaman atau komponen tanpa membuat custom hook terpisah:

```tsx
// app/booking/page.tsx
"use client"
import React from "react";
import { useGetApi } from "@utils";
import { TableSupervisionComponent } from "@components";

export default function BookingPage() {
  // Panggilan langsung diperbolehkan karena tidak ada logika bisnis tambahan
  const { data: dataBooking, loading } = useGetApi({ path: "bookings" });

  return (
    <div>
      <h1>Daftar Booking</h1>
      {loading ? <p>Memuat...</p> : <pre>{JSON.stringify(dataBooking)}</pre>}
    </div>
  );
}
```
*Catatan: Jika ada lebih dari satu `useGetApi`, lakukan rename saat destrukturisasi (contoh: `const { data: dataProduct } = useGetApi(...)`).*
