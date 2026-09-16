# Latihan HMVC CodeIgniter 4 — Pencarian Wilayah Indonesia

Contoh aplikasi **HMVC (Hierarchical Model-View-Controller)** pada CodeIgniter 4:
pencarian kabupaten/kecamatan dengan autocomplete, plus modul CRUD produk
sederhana.

> **Catatan penting:** repositori ini adalah **bahan latihan/tutorial**, bukan
> REST API wilayah Indonesia yang siap pakai. Lihat
> [Status Proyek](#-status-proyek) di bawah sebelum memakainya.

---

## Daftar Isi

- [Apa yang ada di sini](#-apa-yang-ada-di-sini)
- [Status Proyek](#-status-proyek)
- [Menjalankan Secara Lokal](#-menjalankan-secara-lokal)
- [Struktur Proyek](#-struktur-proyek)
- [Peringatan Keamanan](#-peringatan-keamanan)
- [Rencana Perbaikan](#-rencana-perbaikan)
- [Lisensi](#-lisensi)

---

## 📦 Apa yang ada di sini

| Modul | Isi | Keterangan |
|---|---|---|
| `app/Modules/Home` | Autocomplete kabupaten/kecamatan | Mencari dari tabel `m_provinsi`, `m_kabupaten`, `m_kecamatan` |
| `app/Modules/Produk` | CRUD produk | Form tambah/edit/hapus sederhana |

Aplikasi memakai HMVC: setiap modul punya `Controllers/`, `Models/`,
`Views/`, dan `Routes.php` sendiri.

## 📌 Status Proyek

Ditulis apa adanya supaya tidak menyesatkan:

- **Skema database tidak disertakan.** Tabel `m_provinsi`, `m_kabupaten`, dan
  `m_kecamatan` dirujuk oleh `app/Modules/Home/Models/homeModel.php`, tetapi
  tidak ada migration, seeder, maupun berkas `.sql` di repositori ini.
  Tanpa tabel tersebut, fitur pencarian **tidak akan jalan**.
- **Sebagian besar berkas adalah framework.** Dari 601 berkas, 479 di
  antaranya adalah `system/` bawaan CodeIgniter 4. Kode milik sendiri ada di
  `app/`.
- **Halaman utama belum beranda.** Route `/home` menampilkan view `result`
  (`app/Modules/Home/Views/result.php`) yang berisi form pencarian, bukan
  landing page.
- **`app/Controllers/Home__.php`** (dengan akhiran `__`) tidak terpakai —
  sisa percobaan. Berkas ini juga tidak memakai namespace HMVC yang sama
  dengan modul di `app/Modules/`.
- **Tidak ada `.gitignore`.** Sebelum ini, `writable/` (cache, log, sesi,
  unggahan) ikut ter-commit.

## 🚀 Menjalankan Secara Lokal

Butuh PHP 8.1+ dan Composer.

```bash
composer install

# Cara 1: sesuai PETUNJUK.txt — tanpa spark
#   copy ke htdocs, lalu sesuaikan baseURL di app/Config/App.php
#
# Cara 2: server bawaan PHP
php -S localhost:8000 -t public

# Cara 3: spark
php spark serve
```

Lalu buka `http://localhost:8000/home`.

Isi kredensial database di `app/Config/Database.php` (atau lewat `.env`),
lalu siapkan sendiri tabel `m_provinsi`, `m_kabupaten`, dan `m_kecamatan`
karena skemanya belum disertakan.

## 🗂️ Struktur Proyek

```text
wilayah-indonesia-CI-4/
├── app/
│   ├── Config/            # konfigurasi CodeIgniter
│   ├── Controllers/       # BaseController + Home__.php (tidak terpakai)
│   ├── Models/            # BaseModel
│   ├── Modules/
│   │   ├── Home/          # autocomplete wilayah
│   │   └── Produk/        # CRUD produk
│   └── Views/
│       └── themes/modern/ # header & footer tema
├── public/                # document root (index.php)
├── system/                # framework CodeIgniter 4 (jangan diubah)
├── writable/              # cache, log, sesi, unggahan
├── composer.json
├── index.php
└── spark
```

## ⚠️ Peringatan Keamanan

Ada temuan yang perlu ditangani:

1. **API key RajaOngkir ter-hardcode di kode publik.**
   `app/Modules/Home/Controllers/Home.php` memuat key tersebut sebagai nilai
   literal di dalam kelas `Home` (dicari dengan `rajaongkir`). Repositori ini
   publik, jadi key tersebut harus dianggap **sudah bocor**. Cabut/regenerasi
   key di dashboard RajaOngkir, lalu baca dari `.env`.
2. **Ganti key juga di riwayat git.** Menghapusnya dari commit terbaru tidak
   cukup karena nilainya masih tersimpan di riwayat. Setelah dicabut, key
   lama sudah tidak berguna.
3. **`writable/` ikut ter-commit** karena tidak ada `.gitignore`. Berkas
   cache/log/sesi bisa memuat data sensitif.

## 🔧 Rencana Perbaikan

Kalau ingin dijadikan proyek yang benar-benar bisa dipakai:

- [ ] Tambahkan migration + seeder untuk `m_provinsi`, `m_kabupaten`,
      `m_kecamatan` (datanya bisa diambil dari sumber publik seperti
      [emsifa/api-wilayah-indonesia](https://github.com/emsifa/api-wilayah-indonesia))
- [ ] Pindahkan API key ke `.env` dan tambahkan `.env.example`
- [ ] Tambahkan `.gitignore` (minimal: `writable/*`, `vendor/`, `.env`)
- [ ] Buat landing page di route `/`
- [ ] Hapus `app/Controllers/Home__.php` atau perbaiki namespace-nya
- [ ] Tambahkan pengujian untuk `homeModel`

## 📄 Lisensi

Lihat berkas [LICENSE](./LICENSE).

---

<sub>README ini ditulis manual, bukan hasil generate otomasi.</sub>
