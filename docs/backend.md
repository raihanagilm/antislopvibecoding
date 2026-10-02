# Backend — Anti-Slop Guide

> Prefix aturan: `BE-`
> Status: **terisi (2 aturan)**

> **Peta dokumen — wajib dibaca semua (WF-01).** Dokumen ini saling terikat; jangan
> membaca satu file saja. Baca juga: [Root README](../README.md) · [Indeks](README.md) ·
> [Prinsip](principles.md) · [Alur Kerja](workflow.md) · [Backend](backend.md) ·
> [Frontend](frontend.md) · [UI/UX](ui-ux.md) · [Database](database.md) · [Lainnya](others.md)

## Ruang lingkup

API, service, autentikasi, validasi input, keamanan (rate limit, pencegahan SQL injection),
error handling, dan logging.

## Aturan

### BE-01 — Rate limit per user

- **Tell:** Tidak ada batas request per user; endpoint bisa dipanggil sepuasnya.
- **Why:** Tanpa batas, bot atau penyalahgunaan bisa membanjiri server (down atau biaya
  membengkak) — website bisa jebol. **Tanpa angka berarti tanpa proteksi.**
- **Fix:** **Wajib ada angka batas yang konkret** — jangan dikosongkan atau ditulis "X".
  Pakai default berikut sebagai titik awal, lalu sesuaikan per proyek:
  - Umum per user: **60 request/menit**
  - Per IP untuk yang belum login: **100 request/menit**
  - Endpoint autentikasi (login / OTP / reset sandi): **5–10 percobaan/menit** per IP
  - Endpoint berat (upload / generate / export): **10 request/menit** per user
  - Saat terlampaui: kembalikan **HTTP 429** (+ header `Retry-After`)
  - Terapkan juga batas ukuran body request dan timeout.
  **Di mana dicatat:** simpan angkanya di konfigurasi/env (mis. `RATE_LIMIT_PER_USER=60`),
  lalu catat ringkasannya di dokumentasi proyek — jangan hardcode tersebar.
  **Presedensi:** cek limit **per user** dulu; kalau belum login, baru **per IP**.
  Angka default di atas titik awal, bukan harga mati — yang wajib: angkanya **ditetapkan
  dan dicatat**, jangan dibiarkan kosong.

### BE-02 — Cegah SQL injection & sanitasi input

- **Tell:** Query dibuat dengan menggabung string dari input user
  (mis. `"SELECT * FROM pelanggan WHERE nama = '" + input + "'"`), atau input user tidak
  divalidasi.
- **Why:** SQL injection dan penyusupan lain bisa membaca, mengubah, atau menghapus data —
  website bisa dibobol.
- **Fix:** Lindungi dari **SQL injection dan penyusupan lain**:
  - Selalu pakai **parameterized query / prepared statement**, jangan gabung string.
  - Validasi & sanitasi semua input sesuai tipe/format yang diharapkan.
  - **XSS:** jangan render input user mentah sebagai HTML; escape/sanitasi output.
  - **CSRF:** beri proteksi token pada request yang mengubah data (POST/PUT/DELETE).
  - Jangan taruh data user mentah ke query, perintah shell, atau output HTML tanpa pengamanan.
  - Terapkan hak akses database seminimal mungkin (least privilege).

## Checklist Rilis

Semua jawaban harus **ya**:

- [ ] Angka batas request per user/IP sudah ditetapkan (konkret, bukan "X") dan rate
      limiting diterapkan. (`BE-01`)
- [ ] Semua query pakai parameterized query; input user divalidasi & disanitasi. (`BE-02`)
