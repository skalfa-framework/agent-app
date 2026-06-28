# Registri Pengetahuan Skalfa (Knowledge Registry)

Sebelum membuat rencana implementasi (`Stage 1 — Planning`), agen **wajib membaca indeks ini** dan hanya membuka berkas dokumentasi spesifik yang relevan dengan fitur yang akan dibuat.

## 1. Komponen UI (Components)

*   [FormSupervisionComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/form-supervision.md): Gunakan ini jika fitur membutuhkan pembuatan Form, Input data, atau Validasi form di frontend.
*   [TableSupervisionComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/table-supervision.md): Gunakan ini jika fitur membutuhkan penampilan data dalam bentuk Tabel, List, Paginasi, atau Filter di frontend.
*   [ButtonComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/button.md): Gunakan ini jika fitur membutuhkan tombol aksi, tombol loading, navigasi link tombol, atau tombol dengan ikon.
*   [ModalConfirmComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/modal-confirm.md): Gunakan ini jika fitur membutuhkan dialog pop-up konfirmasi untuk tindakan krusial (seperti hapus data atau persetujuan transaksi).
*   [CardComponent & DashboardCardComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/card.md): Gunakan ini jika fitur membutuhkan kontainer visual putih bersih, panel pembungkus konten, atau widget kartu statistik pada halaman ringkasan (dashboard).
*   [ChipComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/chip.md): Gunakan ini jika fitur membutuhkan penampilan opsi tag, lencana kategori kecil, atau data pilihan jamak dengan tombol hapus instan.
*   [AccordionComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/accordion.md): Gunakan ini jika fitur membutuhkan panel lipat untuk menampilkan konten bertahap (seperti FAQ, opsi pembayaran, atau pemisah form panjang).
*   [BreadcrumbComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/breadcrumb.md): Gunakan ini jika fitur membutuhkan penunjuk lokasi halaman aktif (breadcrumb) dalam hierarki navigasi.
*   [OutsideClickComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/outside-click.md): Gunakan ini jika fitur membutuhkan penutupan otomatis elemen (seperti dropdown kustom, tooltip, atau popover) saat pengguna mengklik area luar.
*   [WizardComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/wizard.md): Gunakan ini jika fitur membutuhkan indikator visual proses berurutan multi-langkah (stepper/process bar).
*   [TabbarComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/tabbar.md): Gunakan ini jika fitur membutuhkan navigasi tab horizontal untuk beralih tampilan di dalam satu halaman yang sama tanpa memicu reload.
*   [InputComponent & SelectComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/input-components.md): Gunakan ini jika fitur membutuhkan pembuatan input teks dasar kustom (dengan saran otomatis, transformasi kapitalisasi, atau filter huruf) atau dropdown pemilih yang mendukung multiseleksi kustom dan pemuatan data otomatis dari API/IndexedDB.
*   [TableComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/table.md): Gunakan ini jika fitur membutuhkan penampilan data grid tabel kustom secara manual dengan paginasi kustom, pengurutan kolom, seleksi baris (checkboxes), dan tombol aksi massal.
*   [FloatingPage, BottomSheet & Toast](file:///d:/_skalfa/agent-frontend/knowledges/components/modal-overlays.md): Gunakan ini jika fitur membutuhkan panel laci meluncur dari samping (FloatingPage), panel mobile-friendly dari bawah (BottomSheet), atau notifikasi popup pesan sukses/gagal sementara dengan timer hitung mundur (Toast).
*   [TypographyTipsComponent](file:///d:/_skalfa/agent-frontend/knowledges/components/typography-tips.md): Gunakan ini jika fitur membutuhkan penyajian catatan penting, tips penggunaan, atau panduan pengisian di dalam halaman modul dengan pembatas visual beraksen.

## 2. Utilitas (Utilities)

*   [API Utility](file:///d:/_skalfa/agent-frontend/knowledges/utilities/api.md): Gunakan ini jika fitur memerlukan pemanggilan HTTP request ke backend atau penggunaan kustom hook data fetching.
*   [IndexedDB (IDB) Utility](file:///d:/_skalfa/agent-frontend/knowledges/utilities/idb.md): Gunakan ini jika fitur membutuhkan penyimpanan data lokal di browser menggunakan IndexedDB.
*   [Conversion Utility](file:///d:/_skalfa/agent-frontend/knowledges/utilities/conversion.md): Gunakan ini jika fitur memerlukan manipulasi string (snake, slug, camel, pascal, plural), format mata uang (Rupiah), atau penulisan format tanggal.
*   [Shortcut Utility](file:///d:/_skalfa/agent-frontend/knowledges/utilities/shortcut.md): Gunakan ini jika fitur membutuhkan pintasan keyboard kustom (misal: `escape` untuk menutup modal, `ctrl+s` untuk menyimpan).
*   [Auth Utility](file:///d:/_skalfa/agent-frontend/knowledges/utilities/auth.md): Gunakan ini jika fitur memerlukan pengecekan login, pengaturan sesi token, atau logout.
*   [Resource Utility](file:///d:/_skalfa/agent-frontend/knowledges/utilities/resource.md): Gunakan ini jika fitur membutuhkan pengambilan data terpadu (`useResource`) dari API atau IndexedDB lokal dengan parameter pencarian, filter, dan paginasi terabstraksi.
*   [Class Name Utility (cn, pcn)](file:///d:/_skalfa/agent-frontend/knowledges/utilities/cn.md): Gunakan ini jika fitur membutuhkan penggabungan kelas Tailwind secara aman atau penyaringan gaya kustom berprefiks (`prefiks::nama-kelas`) pada sub-elemen komponen.
*   [Caching Utility (Cavity)](file:///d:/_skalfa/agent-frontend/knowledges/utilities/cavity.md): Gunakan ini jika fitur membutuhkan penyimpanan cache data lokal berbasis IndexedDB dengan waktu kadaluwarsa (caching API request).
*   [Validation Utility](file:///d:/_skalfa/agent-frontend/knowledges/utilities/validation.md): Gunakan ini jika fitur membutuhkan validasi data input mandiri di sisi frontend secara real-time sebelum diproses.
*   [Form & Table State Hooks (useForm, useTable)](file:///d:/_skalfa/agent-frontend/knowledges/utilities/form-table-hooks.md): Gunakan ini jika fitur membutuhkan pengelolaan state form kustom (registrasi, validasi, pengiriman data otomatis) atau pengelolaan state tabel kustom (sinkronisasi URL query, paginasi, filter) secara manual.
