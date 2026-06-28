# Panduan Utilitas: IndexedDB (IDB) (`@utils`)

Dokumen ini menjelaskan penggunaan utilitas `idb` untuk melakukan penyimpanan data lokal di browser menggunakan IndexedDB secara terstruktur.

## 1. Konfigurasi Skema Database (DBSchema)

Sebelum beroperasi, database lokal membutuhkan definisi skema yang jelas.

```typescript
import { DBSchema } from "@utils";

export const myDbSchema: DBSchema = {
  name:    "my_local_database",
  version: 1,
  stores:  {
    users: {
      key:           "id",            // Primary key
      autoIncrement: true,
      fields: {
        id:         "number",
        name:       "string",
        email:      "string",
        is_active:  "boolean",
      },
      indexes: [
        { name: "by_email", fields: "email", unique: true }
      ]
    }
  }
};
```
*Catatan: Kolom `created_at` (date) dan `updated_at` (date) secara otomatis ditambahkan ke seluruh store oleh framework.*

---

## 2. Operasi Dasar Database (`idb`)

Secara default, jika skema sudah diinisialisasi secara global, Anda dapat langsung menggunakan objek `idb`:

### A. Menyimpan Data (`put`)
Menyimpan satu baris data. Jika primary key sudah ada, data akan diperbarui (overwrite). Otomatis mengisi `created_at` dan `updated_at`.
```typescript
import { idb } from "@utils";

await idb.put("users", {
  name:  "Budi",
  email: "budi@example.com",
});
```

### B. Mengupdate Massal (`upsert`)
Melakukan update massal data dengan kebijakan pertentangan kunci (*conflict policy*).
```typescript
// Kebijakan: 'replace' | 'merge' | 'keep-local'
await idb.upsert("users", ArrayOfUsers, "id", "merge");
```

### C. Menghapus Data (`delete`)
Menghapus satu baris data berdasarkan primary key.
```typescript
await idb.delete("users", 1); // Menghapus user dengan ID 1
```

---

## 3. Melakukan Query Data (`IDBQuery`)

Metode `idb.query(storeName)` mengembalikan instance pembangun query `IDBQuery<T>` yang bersifat chainable (berantai).

### Daftar Metode Query Builder:
*   `usingIndex(indexName: string)`: Menggunakan indeks tertentu untuk pencarian cepat.
*   `equals(value: IDBValidKey)`: Mencari data yang memiliki nilai kunci tepat sama.
*   `between(from: IDBValidKey, to: IDBValidKey)`: Mencari data dalam rentang tertentu.
*   `where(fn: (row: T) => boolean)`: Filter kustom menggunakan fungsi JavaScript.
*   `order(dir: "asc" | "desc")`: Mengatur urutan pengurutan data (berdasarkan index atau primary key).
*   `limit(n: number)`: Membatasi jumlah data yang diambil.
*   `paginate(page: number, limit: number)`: Mengambil data pada halaman spesifik.
*   `count()`: Mengembalikan jumlah data hasil query (`Promise<number>`).
*   `get()`: Mengeksekusi query dan mengembalikan array data (`Promise<T[]>`).

### Keterbatasan Kritis: Tidak Ada `.first()`
> [!IMPORTANT]
> Builder `IDBQuery` **TIDAK MEMILIKI** metode `.first()`.
> Untuk mengambil satu data saja (seperti mencari user berdasarkan email), Anda wajib membatasi hasil query menjadi `1` menggunakan `.limit(1)` lalu mengambil elemen pertama dari array hasil kembalian.

### Contoh Meniru Pencarian Data Tunggal (`.first()`):
```typescript
const users = await idb.query("users")
  .usingIndex("by_email")
  .equals("budi@example.com")
  .limit(1)
  .get();

const user = users[0] || null; // Ambil data pertama atau null jika tidak ditemukan
```

### Contoh Query dengan Filter Kustom dan Paginasi:
```typescript
const activeUsers = await idb.query("users")
  .where(user => user.is_active === true)
  .order("desc")
  .paginate(1, 10)
  .get();
```
