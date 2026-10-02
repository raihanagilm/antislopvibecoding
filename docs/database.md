# Database — Anti-Slop Guide

> Prefix aturan: `DB-`
> Status: **terisi (6 aturan)**

> **Peta dokumen — wajib dibaca semua (WF-01).** Dokumen ini saling terikat; jangan
> membaca satu file saja. Baca juga: [Root README](../README.md) · [Indeks](README.md) ·
> [Prinsip](principles.md) · [Alur Kerja](workflow.md) · [Backend](backend.md) ·
> [Frontend](frontend.md) · [UI/UX](ui-ux.md) · [Database](database.md) · [Lainnya](others.md)

## Ruang lingkup

Pemilihan jenis database, skema, relasi antar tabel, penamaan entitas, format ID/UID,
dan dokumentasi skema. Aturan untuk isi kode ada di area lain (`backend.md`,
`frontend.md`, dst); baca semuanya sesuai `WF-01`.

## Aturan

### DB-01 — Rekomendasi & pilih jenis database dulu

- **Tell:** Agent langsung membuat skema atau memilih jenis database (SQL relasional,
  NoSQL, dll.) tanpa bertanya ke user lebih dulu.
- **Why:** Jenis database menentukan struktur, cara query, biaya, dan cara maintain.
  Menebaknya berarti salah arah sejak awal.
- **Fix:** Sebelum membuat skema, sajikan rekomendasi jenis database beserta alasan,
  kelebihan, dan kekurangan tiap opsi (rujuk `WF-03`), lalu **tanya** user mau memakai
  yang mana. Jangan lanjut sebelum user memilih.

### DB-02 — Relasi antar tabel wajib eksplisit

- **Tell:** Tabel dibuat lepas tanpa foreign key; hubungan antar data hanya "disimpan
  di kepala" atau digabung manual di kode aplikasi.
- **Why:** Tanpa relasi eksplisit, integritas data rapuh dan struktur mustahil ditelusuri
  maupun dimaintain.
- **Fix:** Definisikan relasi primary key → foreign key untuk setiap hubungan antar
  entitas. Kalau sengaja non-relasional (mis. NoSQL — lihat `DB-01`), tulis alasannya
  tertulis **dan** jelaskan bagaimana integritas hubungan dijaga. Jadi "boleh pakai NoSQL"
  bukan berarti "boleh tanpa relasi".

### DB-03 — Skema wajib tercatat & bisa dilihat

- **Tell:** Skema hanya ada di migrasi/kode; tidak ada diagram relasi atau dokumentasi
  tabel.
- **Why:** Sulit dilihat dan dimaintain. Orang atau agent berikutnya harus membongkar
  migrasi hanya untuk tahu struktur.
- **Fix:** Catat skema di repo: diagram relasi (ERD) + dokumentasi tabel (nama, kolom,
  tipe data, relasi). **Simpan di satu lokasi tetap** yang disepakati — default
  `db/schema.md` (buat foldernya bila belum ada) — dan catat lokasinya di peta struktur
  (`WF-09`). Update-nya ikut aturan `WF-07`.

### DB-04 — Nama entitas/tabel wajib Bahasa Indonesia

- **Tell:** Nama tabel/entitas memakai Bahasa Inggris atau campur-campur
  (mis. `users`, `order_items`).
- **Why:** Aturan proyek: konsistensi dan keterbacaan. Campur bahasa memicu kebingungan
  dan inkonsistensi.
- **Fix:** Pakai Bahasa Indonesia untuk nama **tabel dan kolom** (mis. tabel `pelanggan`,
  kolom `nama`, `tanggal_lahir`), format `snake_case`, bentuk tunggal untuk tabel. Kolom PK
  pakai `<tabel>_id` (mis. `pelanggan_id`); FK mengikuti nama tabel tujuannya.

### DB-05 — Format ID/UID wajib bersistem

- **Tell:** ID berupa angka besar tanpa makna (mis. `6000`, `1234567`) atau
  auto-increment polos.
- **Why:** Angka besar tidak self-explanatory — sulit dibaca, dibedakan, dan didebug,
  serta rawan typo.
- **Fix:** Pakai format UID konsisten `<prefix><YYYYMMDD><4 digit acak>` (tanpa pemisah):
  - **prefix** = kode entitas, huruf kecil Bahasa Inggris (mis. `cust`, `ord`, `prd`)
  - **YYYYMMDD** = tanggal record dibuat
  - **4 digit acak**
  - contoh pelanggan: `cust202601014821`
  - **ID wajib unik.** Karena 4 digit acak bisa **tabrakan**, regenerate saat bentrok atau
    cek dulu ke database sebelum simpan (retry).
  - **Daftar prefix per entitas wajib dicatat** (nyambung `DB-03`) supaya seragam.

  | Entitas | Prefix | Contoh ID |
  |---|---|---|
  | pelanggan | `cust` | `cust202601014821` |
  | pesanan | `ord` | `ord202601014821` |
  | produk | `prd` | `prd202601014821` |

### DB-06 — Index untuk query data besar

- **Tell:** Tabel dengan banyak baris di-query, difilter, atau di-sort tanpa index, sehingga
  halaman lambat merender data yang banyak.
- **Why:** Tanpa index, database memindai seluruh baris (full scan) di tiap query. Makin
  banyak data, makin lambat.
- **Fix:** Tambahkan **index** pada kolom yang sering dipakai untuk filter, join, dan sort
  (foreign key, `created_at`, kolom pencarian, dll.). Catat index-nya di skema (`DB-03`).
  Untuk data besar, batasi juga jumlah baris per tampilan (paginasi). Jangan mengindeks
  semua kolom tanpa alasan — index juga menambah biaya tulis.

## Checklist Rilis

Semua jawaban harus **ya**:

- [ ] Sudah menyajikan rekomendasi jenis database + alasan, pro, dan kontra, dan user
      sudah memilih. (`DB-01`)
- [ ] Semua relasi antar tabel sudah eksplisit (PK → FK), atau alasan non-relasional
      sudah ditulis. (`DB-02`)
- [ ] Skema tercatat: ada ERD + dokumentasi tabel di repo. (`DB-03`)
- [ ] Nama tabel & kolom memakai Bahasa Indonesia, `snake_case`, bentuk tunggal. (`DB-04`)
- [ ] Semua ID memakai format `<prefix><YYYYMMDD><4 digit acak>`, unik, dan daftar prefix
      tercatat. (`DB-05`)
- [ ] Kolom yang sering di-query/filter/sort sudah diindeks, dan data besar dipaginasi. (`DB-06`)
