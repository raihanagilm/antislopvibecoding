# Lainnya — Anti-Slop Guide

> Prefix aturan: `OT-`
> Status: **terisi (2 aturan)**

> **Peta dokumen — wajib dibaca semua (WF-01).** Dokumen ini saling terikat; jangan
> membaca satu file saja. Baca juga: [Root README](../README.md) · [Indeks](README.md) ·
> [Prinsip](principles.md) · [Alur Kerja](workflow.md) · [Backend](backend.md) ·
> [Frontend](frontend.md) · [UI/UX](ui-ux.md) · [Database](database.md) · [Lainnya](others.md)

## Ruang lingkup

Area di luar backend/frontend/UI-UX/database: pengujian/QA, keamanan (pengujian), CI/CD,
git & commit, DevOps, dan dokumentasi.

## Aturan

### OT-01 — AI berperan sebagai QA/tester

- **Tell:** Fitur dinyatakan selesai tanpa diuji; keamanan tidak diverifikasi.
- **Why:** Tanpa pengujian, bug dan celah keamanan lolos ke produksi. User jadi tidak tahu
  web-nya aman atau belum.
- **Fix:** Sebelum menyatakan selesai, AI berperan sebagai **QA/tester**:
  - Uji **fungsi** (happy path + edge case), **batas** (rate limit, input kosong/aneh/
    berbahaya), dan **keamanan** (SQL injection, akses tak sah, kebocoran data sensitif).
  - Pakai alat yang ada di proyek (test runner/framework); kalau belum ada, uji manual dan
    catat langkahnya.
  - **Format laporan:** daftar bernomor — (1) apa yang diuji, (2) hasil (lulus/gagal),
    (3) temuan + tingkat keparahan, (4) rekomendasi. Nyambung `WF-08`.

### OT-02 — .gitignore & file sensitif

- **Tell:** Tidak ada `.gitignore` (atau tidak lengkap), dan tidak jelas file mana yang aman
  dipush vs mengandung rahasia.
- **Why:** Tanpa ini, kredensial/data sensitif bisa ikut ter-commit ke GitHub — bocor dan
  sulit ditarik kembali.
- **Fix:** Sediakan `.gitignore` yang **lengkap**: dependensi (`node_modules/`), env
  (`.env*`), kunci (`*.pem`, `*.key`, `*.p12`), `secrets/`, `credentials.json`, build
  (`dist/`, `build/`, `.next/`), log, cache, dan file OS/editor. Lalu **dokumentasikan**
  mana yang dipush dan mana yang tidak, serta **tandai file yang mengandung data sensitif**.
  Jangan pernah commit rahasia (kunci/token/password/string koneksi) — pakai env var atau
  secret manager. (nyambung `WF-07`)

## Checklist Rilis

Semua jawaban harus **ya**:

- [ ] Sebelum selesai, fitur & keamanan sudah diuji sebagai QA/tester dan hasilnya
      dilaporkan. (`OT-01`)
- [ ] `.gitignore` lengkap; file sensitif sudah ditandai dan tidak dipush. (`OT-02`)
