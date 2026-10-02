# Frontend — Anti-Slop Guide

> Prefix aturan: `FE-`
> Status: **terisi (3 aturan)**

> **Peta dokumen — wajib dibaca semua (WF-01).** Dokumen ini saling terikat; jangan
> membaca satu file saja. Baca juga: [Root README](../README.md) · [Indeks](README.md) ·
> [Prinsip](principles.md) · [Alur Kerja](workflow.md) · [Backend](backend.md) ·
> [Frontend](frontend.md) · [UI/UX](ui-ux.md) · [Database](database.md) · [Lainnya](others.md)

## Ruang lingkup

Struktur & alur kode frontend: urutan render/eksekusi, penerapan konsep OOP/komponen, dan
reuse kode. Untuk penamaan elemen dan kualitas visual, lihat `ui-ux.md`.

## Aturan

### FE-01 — Urutan render: konten utama dulu, sekunder setelahnya

- **Tell:** Saat pindah menu/halaman, elemen sekunder (mis. notifikasi) ter-render lebih
  dulu, baru konten utama.
- **Why:** Konten utama adalah tujuan user. Kalau elemen sekunder muncul lebih dulu,
  terasa salah/janky dan menyimpang dari urutan yang logis.
- **Fix:** Susun urutan render/eksekusi sesuai prioritas: **konten utama halaman lebih
  dulu**, elemen sekunder (notifikasi, toast, sidebar, dll.) setelahnya. Saat memperbaiki
  atau meng-update kode, periksa **urutan**-nya, bukan hanya tampilannya.

### FE-02 — Konsep OOP & reuse: jangan tulis ulang yang sama

- **Tell:** Komponen yang sama (input, notifikasi, potongan halaman) ditulis ulang di
  beberapa halaman — dan hasilnya pun tidak identik.
- **Why:** Duplikasi bikin kode panjang, susah dimaintain, dan memunculkan inkonsistensi
  antar halaman.
- **Fix:** Terapkan konsep OOP/komponen: buat sekali komponennya (mis. `InputField`,
  `Notification`, `CardProduk`), lalu **panggil** di halaman mana pun yang membutuhkannya —
  jangan menulis ulang. Satu sumber kebenaran untuk tiap komponen (`DRY`, nyambung `UX-05`).

### FE-03 — Pisahkan token desain (warna, ukuran, format)

- **Tell:** Warna/ukuran/format ditulis langsung di tiap halaman atau komponen (hardcode),
  sehingga nilainya berbeda-beda antar halaman.
- **Why:** Hardcode bikin inkonsistensi dan susah diubah; tiap halaman "mengarang" nilainya
  sendiri, jadi warna, ukuran, dan format tidak seragam.
- **Fix:** Pisahkan **token desain** ke satu tempat (mis. file tema/`theme`, atau variabel
  CSS: warna, ukuran, spacing, radius, rasio). Halaman dan komponen **memakai** token itu,
  bukan menulis nilai mentah. Tujuannya: warna, ukuran, dan format konsisten antar halaman
  (nyambung `UX-05`, `UX-08`, `FE-02`).

## Checklist Rilis

Semua jawaban harus **ya**:

- [ ] Urutan render benar: konten utama dulu, elemen sekunder (notifikasi, dst.) setelahnya. (`FE-01`)
- [ ] Tidak ada komponen/kode yang sama ditulis ulang; dipanggil dari satu sumber
      (OOP/reuse). (`FE-02`)
- [ ] Warna/ukuran/format berasal dari token desain terpusat, tidak di-hardcode per halaman. (`FE-03`)
