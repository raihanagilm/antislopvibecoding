# Indeks Dokumen

Kumpulan panduan anti-slop, dipisah per area.

> **Peta dokumen — wajib dibaca semua (WF-01).** Dokumen ini saling terikat; jangan
> membaca satu file saja. Baca juga: [Root README](../README.md) · [Indeks](README.md) ·
> [Prinsip](principles.md) · [Alur Kerja](workflow.md) · [Backend](backend.md) ·
> [Frontend](frontend.md) · [UI/UX](ui-ux.md) · [Database](database.md) · [Lainnya](others.md)

## Struktur

```
docs/
  README.md        <- kamu di sini (indeks)
  principles.md    <- definisi "slop" + bentuk baku aturan
  _TEMPLATE.md     <- template untuk membuat/mengisi file area
  backend.md
  frontend.md
  ui-ux.md
  database.md
  others.md
  workflow.md
```

## Struktur folder proyek

Peta lengkap repo ini (sumber kebenaran). Dibaca lewat `WF-01`; perbarui bila struktur
berubah (`WF-09`).

```
antislopvibecoding/
  README.md          <- ringkasan proyek + kebijakan push
  .gitignore         <- daftar file yang TIDAK dipush
  docs/              <- SUMBER KEBENARAN (Bahasa Indonesia, untuk pemilik)
    README.md        <- indeks + peta struktur (kamu di sini)
    principles.md    <- definisi "slop", bentuk aturan, tier, gerbang rilis
    workflow.md      <- aturan proses (WF-xx)
    backend.md       <- aturan backend (BE-xx)
    frontend.md      <- aturan frontend (FE-xx)
    ui-ux.md         <- aturan UI/UX (UX-xx)
    database.md      <- aturan database (DB-xx)
    others.md        <- aturan lainnya / QA / keamanan (OT-xx)
    _TEMPLATE.md     <- template area & entri
  skills/            <- OUTPUT untuk agent (Bahasa Inggris, format SKILL.md)
    antislop-vibecoding/SKILL.md            <- core (prinsip + alur kerja)
    antislop-vibecoding-backend/SKILL.md
    antislop-vibecoding-frontend/SKILL.md
    antislop-vibecoding-uiux/SKILL.md
    antislop-vibecoding-database/SKILL.md
    antislop-vibecoding-others/SKILL.md
```

## Cara menambah aturan

1. Buka file area yang relevan (mis. `database.md`).
2. Salin **blok aturan (`### <Judul>` + Tell/Why/Fix)** dari `_TEMPLATE.md` (bagian "Aturan").
3. Isi **Tell / Why / Fix**, beri **Rule ID** sesuai prefix area (mis. `DB-03`).
4. Nomor naik berurutan. Jangan pernah memakai ulang ID.
5. Kalau aturan menyentuh lebih dari satu area, tulis di area paling utama dan
   rujuk dari area lain lewat ID-nya.

## Prefix per area

| Area | Prefix | Contoh |
|---|---|---|
| Backend | `BE-` | `BE-01` |
| Frontend | `FE-` | `FE-01` |
| UI/UX | `UX-` | `UX-01` |
| Database | `DB-` | `DB-01` |
| Lainnya | `OT-` | `OT-01` |
| Alur Kerja Agent | `WF-` | `WF-01` |

## Aturan emas

- **Jangan mengarang isi.** Aturan hanya boleh ditulis oleh pemilik repo (atau dari
  sumber yang ia berikan). AI hanya merapikan format.
- **Satu aturan, satu ide.** Kalau tidak bisa dijelaskan dalam satu baris alasan,
  pecah aturannya.
- **Selalu ada alasan.** Tiap aturan wajib punya satu kalimat "kenapa".
- **Saling terikat.** Tiap file punya blok "Peta dokumen". Kalau menambah/mengganti
  area atau file baru, update blok itu di **semua** file, plus tabel prefix & status
  di sini.

## Status pengisian

| Area | Aturan terisi |
|---|---|
| Backend | 2 |
| Frontend | 3 |
| UI/UX | 12 |
| Database | 6 |
| Lainnya | 2 |
| Alur Kerja | 9 |
