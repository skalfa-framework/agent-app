# Panduan Komponen: Komponen Input (`@components`)

Dokumen ini menjelaskan seluruh variasi komponen input kustom bawaan Skalfa App yang dapat digunakan secara mandiri untuk berinteraksi dengan pengguna.

---

## 1. Input Teks Standar (`InputComponent`)

Komponen input teks dasar yang membungkus tag `<input>` HTML dengan fitur label, pesan error, ikon, saran otomatis (*suggestions*), dan transformasi teks.

```typescript
interface InputProps extends Omit<InputHTMLAttributes<HTMLInputElement>, "onChange"> {
  label        ?: string;
  tip          ?: string | ReactNode;
  leftIcon     ?: any;                                            // Ikon FontAwesome kiri
  rightIcon    ?: any;                                            // Ikon FontAwesome kanan
  value        ?: any;
  invalid      ?: string;                                         // Pesan kesalahan validasi
  suggestions  ?: string[];                                       // Daftar saran pelengkapan otomatis
  onlyAlphabet ?: boolean;                                        // Hanya mengizinkan huruf saja
  uppercase    ?: boolean;                                        // Otomatis mengubah input menjadi HURUF BESAR
  lowercase    ?: boolean;                                        // Otomatis mengubah input menjadi huruf kecil
  onChange     ?: (value: any) => void;
}
```

---

## 2. Dropdown Pilihan (`SelectComponent`)

Komponen pilihan (select dropdown) kustom tingkat lanjut yang mendukung pencarian lokal, pemilihan jamak (*multiple*), serta pemuatan opsi langsung dari **API** atau **IndexedDB** secara otomatis.

```typescript
interface SelectProps {
  name                 : string;
  label               ?: string;
  placeholder         ?: string;
  value               ?: string | number | (string | number)[];
  invalid             ?: string;
  multiple            ?: boolean;                                 // Memilih lebih dari satu (render sebagai chip)
  clearable           ?: boolean;                                 // Tombol silang untuk menghapus pilihan
  options             ?: SelectOptionProps[];                     // Opsi manual
  searchable          ?: boolean;                                 // Input pencarian lokal
  serverOptionControl ?: ApiType & { cacheName?: string | false };// Tarik opsi otomatis dari API
  idbOptionControl    ?: { store: string, labelKey: string, valueKey: string }; // Tarik opsi dari IndexedDB
  onChange            ?: (value: any, selectedData?: any) => void;
}
```

---

## 3. Pilihan Ganda Checkbox (`InputCheckboxComponent`)

Komponen grup checkbox untuk memilih satu atau beberapa opsi sekaligus. Mendukung daftar opsi manual maupun dinamis dari server API.

### Antarmuka Komponen (`InputCheckboxProps`)
```typescript
interface InputCheckboxProps {
  name                 : string;
  label               ?: string;
  vertical            ?: boolean;                                 // Susunan vertikal (default: horizontal)
  value               ?: (string | number)[];                     // Array nilai terpilih
  options             ?: { value: string | number; label: string }[]; // Opsi manual
  serverOptionControl ?: ApiType;                                 // Tarik opsi otomatis dari API
  onChange            ?: (value: (string | number)[]) => void;
}
```

---

## 4. Pilihan Tunggal Radio (`InputRadioComponent`)

Komponen grup radio button untuk memilih tepat satu opsi dari daftar pilihan yang tersedia.

### Antarmuka Komponen (`InputRadioProps`)
```typescript
interface InputRadioProps {
  name                 : string;
  label               ?: string;
  vertical            ?: boolean;                                 // Susunan vertikal (default: horizontal)
  value               ?: string | number;                         // Nilai terpilih
  options             ?: { value: string | number; label: string }[];
  serverOptionControl ?: ApiType;
  onChange            ?: (value: string | number) => void;
}
```

---

## 5. Input Nominal Rupiah (`InputCurrencyComponent`)

Komponen input angka khusus mata uang yang secara otomatis memformat input angka mentah menjadi nominal berformat Rupiah dengan pemisah ribuan saat pengguna mengetik.

### Antarmuka Komponen (`InputCurrencyProps`)
```typescript
interface InputCurrencyProps extends Omit<InputHTMLAttributes<HTMLInputElement>, "onChange"> {
  label    ?: string;
  value    ?: number;                                             // Nilai berupa angka murni (number)
  format   ?: { locale?: string; currency?: string };             // Default: id-ID & IDR
  onChange ?: (value: number) => void;                            // Mengembalikan angka murni (bukan terformat)
}
```

---

## 6. Input Tanggal (`InputDateComponent`)

Komponen pemilih tanggal (date picker). Pada layar desktop menampilkan popover kalender di dekat input, sedangkan pada layar seluler (mobile) otomatis memicu **BottomSheet** dari bawah.

```typescript
interface InputDateProps extends Omit<InputHTMLAttributes<HTMLInputElement>, "onChange"> {
  label    ?: string;
  value    ?: string;                                             // Format ISO (YYYY-MM-DD)
  onChange ?: (value: string) => void;
}
```

---

## 7. Input Waktu (`InputTimeComponent`)

Komponen pemilih waktu (time picker) kustom dengan visualisasi pemutar jam dan menit yang mudah disentuh.

```typescript
interface InputTimeProps extends Omit<InputHTMLAttributes<HTMLInputElement>, "onChange"> {
  label    ?: string;
  value    ?: string;                                             // Format waktu (HH:mm)
  onChange ?: (value: string) => void;
}
```

---

## 8. Input Tanggal & Waktu (`InputDatetimeComponent`)

Komponen pemilih gabungan tanggal dan waktu sekaligus. Menyediakan bilah tab (`TabbarComponent`) internal untuk beralih antara memilih "Tanggal" dan "Waktu".

```typescript
interface InputDateTimeProps extends Omit<InputHTMLAttributes<HTMLInputElement>, "onChange"> {
  label    ?: string;
  value    ?: string;                                             // Format tanggal-waktu ISO
  onChange ?: (value: string) => void;
}
```

---

## 9. Unggah Gambar & Potong (`InputImageComponent`)

Komponen unggah gambar dengan pratinjau gambar, area seret-dan-taruh (*drag-and-drop*), serta **fitur pemotongan gambar (crop modal)** sebelum disimpan.

```typescript
interface InputImageProps {
  name      : string;
  label    ?: string;
  value    ?: string | File;
  aspect   ?: string;                                             // Rasio pemotongan, contoh: "1/1", "16/9"
  onChange ?: (file?: File | null) => void;                       // Mengembalikan objek File hasil potongan
}
```

---

## 10. Unggah Dokumen (`InputDocumentComponent`)

Komponen pengunggahan berkas dokumen umum (seperti PDF, Excel, Word, dll.) dengan visualisasi ikon ekstensi file serta penampil dokumen bawaan (*document viewer*).

```typescript
interface InputDocumentProps extends Omit<InputHTMLAttributes<HTMLInputElement>, "onChange"> {
  label    ?: string;
  value    ?: any;                                                // URL berkas atau objek File
  onChange ?: (value: any) => void;
}
```

---

## 11. Verifikasi OTP (`InputOtpComponent`)

Komponen kotak input khusus OTP (One-Time Password) yang membagi input menjadi beberapa kotak karakter terpisah.

```typescript
interface InputOtpProps {
  name     : string;
  label   ?: string;
  length  ?: number;                                              // Jumlah kotak karakter (default: 6)
  value   ?: string;
  onChange?: (value: string) => void;
}
```

---

## 12. Kata Sandi & Konfirmasi (`InputPasswordComponent`)

Komponen khusus untuk input kata sandi yang memiliki indikator kekuatan sandi serta kolom konfirmasi kata sandi otomatis jika diaktifkan.

```typescript
interface InputPasswordProps extends Omit<InputHTMLAttributes<HTMLInputElement>, "onChange"> {
  label    ?: string;
  value    ?: string;
  onChange ?: (value: string, confirmValue?: string) => void;
}
```

---

## 13. Peta & Koordinat Lokasi (`InputMapComponent`)

Komponen input khusus pemilihan lokasi geospasial menggunakan Google Maps API. Pengguna dapat memilih lokasi dengan mengklik peta atau mengetik alamat di kotak pencarian.

### Antarmuka Komponen (`InputMapProps`)
```typescript
interface InputMapProps extends Omit<React.InputHTMLAttributes<HTMLInputElement>, "onChange"> {
  label    ?: string;
  value    ?: ValueMapProps;                                      // Koordinat { lat, lng, address }
  onChange ?: (value: ValueMapProps) => void;
}

interface ValueMapProps {
  lat      : number | null;
  lng      : number | null;
  address ?: string;
}
```
*Fitur Utama*:
*   **Auto-Geocoding**: Mengklik koordinat pada peta otomatis memicu pencarian nama alamat secara terbalik (*reverse geocoding*) via Google Maps Geocoding API.
*   **Lokasi Saat Ini**: Menyediakan tombol pintas dengan ikon target (`faLocationCrosshairs`) untuk mendeteksi koordinat GPS pengguna secara instan (*geolocation*).
*   **Mobile-Friendly**: Pada perangkat seluler, tampilan peta otomatis dimuat di dalam laci `BottomSheet` agar nyaman dioperasikan.
