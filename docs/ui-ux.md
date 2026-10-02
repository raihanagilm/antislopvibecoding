# UI/UX — Anti-Slop Guide

> Prefix aturan: `UX-`
> Status: **terisi (12 aturan)**

> **Peta dokumen — wajib dibaca semua (WF-01).** Dokumen ini saling terikat; jangan
> membaca satu file saja. Baca juga: [Root README](../README.md) · [Indeks](README.md) ·
> [Prinsip](principles.md) · [Alur Kerja](workflow.md) · [Backend](backend.md) ·
> [Frontend](frontend.md) · [UI/UX](ui-ux.md) · [Database](database.md) · [Lainnya](others.md)

## Ruang lingkup

Penamaan elemen/komponen, kualitas visual (premium vs "kelihatan AI"), warna & notifikasi,
ikon, konsistensi antar halaman, rasio, sistem ukuran/kelipatan, dan responsif
(mobile-first, rentang perangkat, orientasi). Untuk urutan render dan struktur kode
komponen, lihat `frontend.md`.

## Aturan

### UX-01 — Sebut nama elemen/komponen, jangan cuma "sudah diperbaiki"

- **Tell:** User minta perbaikan atau bertanya sambil kirim foto / deskripsi kabur
  ("border ini", "kotak ini"), lalu agent langsung memperbaiki tanpa menyebut nama
  komponennya.
- **Why:** User tidak tahu nama elemen, jadi ia tidak berkembang — tidak bisa meminta
  atau mempelajari hal yang sama di lain waktu. Kirim foto tanpa penjelasan = user tidak
  belajar dan agent tak paham maksudnya.
- **Fix:** Setiap kali memperbaiki atau merujuk elemen, sebut **nama teknis** komponennya
  (mis. "ini namanya *pagination*", "ini *card*", "ini *breadcrumb*", "ini *toast*").
  Kalau user kirim foto, analisis dan namai elemennya. Tujuannya user belajar, bukan
  sekadar menerima barang jadi.

### UX-02 — Desain wajib terasa premium, bukan "AI/lebay"

- **Tell:** Tampilan memakai warna mencolok, banyak warna berbeda yang tak berkaitan, atau
  efek bergaya AI seperti glassmorphism berlebihan, gradien norak, dan glow.
- **Why:** Ini ciri khas output AI — terlihat murah dan generik, bukan dirancang presisi.
- **Fix:** Pakai referensi desain premium yang terverifikasi (cari lewat MCP/web) yang
  kalem, tidak lebay, dan tidak "kelihatan AI". Batas konkretnya:
  - Maksimal **1 warna dominan + 1 aksen** (netral sebagai dasar); total hue ≤ **3**.
  - Dilarang: glassmorphism, glow, gradient > 2 warna, drop-shadow berwarna.
  - "Ramai" = lebih dari 1 aksen **atau** > 3 hue.
  - Ambil 1–2 rujukan premium (lewat MCP/web) dan sebutkan alasannya.

### UX-03 — Warna notifikasi/status konsisten

- **Tell:** Warna status (sukses/gagal/peringatan/info) berubah-ubah antar halaman atau
  memakai warna yang tidak lazim untuk status.
- **Why:** Makna warna status harus bisa dipelajari sekali lalu dipakai konsisten; variasi
  membuat user bingung.
- **Fix:** Pakai warna standar yang konsisten: **merah** (gagal), **kuning** (peringatan),
  **hijau** (sukses), **biru** (info). Satu makna = satu warna, sama di seluruh aplikasi.
  Ikon/logo perusahaan boleh memakai warna brand-nya sendiri.

### UX-04 — Ikon: dilarang emoji keyboard & icon set pasaran

- **Tell:** Memakai emoji dari keyboard (✅ 🔥 🚀) sebagai ikon UI, atau icon set yang
  itu-itu saja / sudah pasaran.
- **Why:** Emoji keyboard dan icon set pasaran adalah tanda paling jelas output AI —
  gampang dikenali dan bikin tampilan terlihat tidak dipikirkan.
- **Fix:** Jangan pakai emoji keyboard sebagai ikon, dan jangan pakai set ikon yang paling
  umum dipakai output AI (ciri: muncul di hampir semua hasil generatif). Cari ikon dari
  penyedia/referensi terverifikasi (lewat MCP atau repo GitHub **star > 1000**, rujuk
  `WF-04`). Gaya ikon harus konsisten dalam satu keluarga.
  Catatan: daftar set ikon terlarang boleh ditambah pemilik kapan saja.

### UX-05 — Konsistensi antar halaman (tolok ukur: halaman yang sudah ada)

- **Tell:** Halaman/menu baru dibuat dengan ukuran, warna, atau gaya yang beda dari
  halaman yang sudah ada; agent "mengarang" lagi dari nol.
- **Why:** Inkonsistensi merusak kesan rapi & premium, dan memaksa user mengoreksi
  hal yang sama berulang.
- **Fix:** Sebelum membuat halaman/menu baru, **lihat halaman/menu yang sudah ada** sebagai
  contoh, lalu samakan ukuran tinggi/lebar, warna, spacing, dan tipografi. Nilai-nilai ini
  disimpan sebagai **token desain** — lihat `FE-03` (jangan dicatat di dua tempat).

### UX-06 — Rasio wajib (card, gambar, border)

- **Tell:** Card, gambar, atau border dibuat "sekadar pas" tanpa rasio tetap; tiap
  komponen beda rasio.
- **Why:** Rasio yang konsisten bikin layout terasa terencana; rasio asal-asalan bikin
  berantakan.
- **Fix:** Pakai rasio tetap untuk card/gambar/border. Set baku: **`1:1`, `4:3`, `3:4`,
  `16:9`, `9:16`**. "Naik satu tingkat" = perbesar sisi yang lebih panjang (mis. `3:4` →
  `9:16` untuk portrait; `4:3` → `16:9` untuk landscape). Jangan ubah rasio sembarangan.

### UX-07 — Ukuran wajib kelipatan / skala konsisten

- **Tell:** Ukuran tinggi/lebar dibuat angka acak (mis. 37px, 123px) yang tidak mengikuti
  kelipatan dari ukuran yang sudah ada.
- **Why:** Sistem kelipatan menjaga konsistensi dan memudahkan memperbesar/memperkecil
  tanpa merusak layout.
- **Fix:** Semua ukuran mengikuti **base scale** tetap (default **kelipatan 4**: 4, 8, 12,
  16, 32, 64) dan satuannya konsisten (px untuk ukuran, rem untuk tipografi). Kalau nilai
  yang ada `4x4`: "besarkan" = kali 2 → `8x8`; "kecilkan" = bagi 2 → `2x2`. Kalau user
  bilang masih **"terlalu besar"** padahal kamu sudah ikut kelipatan, **jangan mengarang** —
  jelaskan bahwa kamu sudah mengikuti aturan, lalu tawarkan kelipatan baru (mis. `1,5x`);
  kalau disetujui, jadikan nilai itu panduan berikutnya.

### UX-08 — Satu warna = satu palet (shade ramp)

- **Tell:** User minta "biru", lalu agent memakai satu nilai biru saja — atau biru yang
  berbeda-beda di tiap tempat — tanpa tingkatan shade.
- **Why:** Satu nilai warna bikin UI datar/kaku; warna yang tidak konsisten antar komponen
  bikin terlihat berantakan dan "kelihatan AI".
- **Fix:** Untuk satu warna, buat **palet gradasi (shade ramp)** dari gelap → terang sampai
  mendekati putih, minimal **5 tingkat** dengan penamaan konsisten (mis. `biru-900` gelap →
  `biru-700` → `biru-500` dasar → `biru-300` → `biru-100` terang). Semua komponen memakai
  nilai dari palet itu, bukan biru acak. (nyambung `FE-03`)

### UX-09 — Kontras wajib (teks vs background)

- **Tell:** Teks dan background memakai warna sekeluarga tanpa mengecek kontras — mis.
  background biru gelap dengan teks biru gelap juga.
- **Why:** Teks jadi tidak terbaca. Ini kesalahan paling umum dan langsung merusak kualitas
  tampilan.
- **Fix:** Pastikan kontras teks terhadap background cukup: jangan pakai teks dan background
  dari shade yang sama/berdekatan. Di background gelap pakai teks terang dari palet yang
  sama, dan sebaliknya. Angka minimalnya:
  - Teks normal: **≥ 4.5:1**
  - Teks besar (≥ 24px, atau ≥ 18.66px **bold**): **≥ 3:1**
  - Ikon/komponen/border: **≥ 3:1**
  Cek kontrasnya, jangan dikira-kira.

### UX-10 — Mobile first

- **Tell:** Halaman dirancang untuk desktop dulu, lalu mobile "dipaksa muat" (diperkecil,
  kebesaran, atau muncul scroll horizontal).
- **Why:** Mayoritas akses datang dari layar kecil. Desain desktop-first bikin layout mobile
  jeblok: kepotong, sempit, atau elemen tumpang tindih.
- **Fix:** Rancang dan bangun dari **layar terkecil dulu** (mobile), lalu naik bertahap ke
  layar besar. Pastikan tidak ada scroll horizontal di mobile. (Cakupan ukuran ada di
  `UX-11`; orientasi ada di `UX-12`.)

### UX-11 — Semua ukuran perangkat (mobile terkecil → laptop/desktop terbesar)

- **Tell:** Hanya dicek di satu ukuran (biasanya desktop), lalu dianggap selesai.
- **Why:** Layout harus tetap rapi di seluruh rentang device; bug sering muncul justru di
  ukuran ekstrem.
- **Fix:** Pastikan layout benar dari **mobile terkecil** sampai **laptop/desktop terbesar**.
  Breakpoint baku (px): **320 · 480 · 768 · 1024 · 1280 · 1440 · 1920**, disimpan sebagai
  token (`FE-03`). Uji di ukuran ekstrem, bukan cuma satu titik.

### UX-12 — Orientasi (rotasi)

- **Tell:** Hanya didesain untuk satu orientasi; saat perangkat dirotasi (portrait ↔
  landscape) layout rusak atau kepotong.
- **Why:** User memutar layar (nonton video, main game, pakai tablet). Kalau diabaikan,
  tampilan pecah.
- **Fix:** Tangani **portrait dan landscape**. Saat dirotasi, layout harus reflow dengan benar
  (tinggi viewport berubah) tanpa elemen kepotong atau scroll horizontal. Uji dengan rotasi
  nyata, minimal di mobile dan tablet.

## Checklist Rilis

Semua jawaban harus **ya**:

- [ ] Nama teknis tiap komponen yang diperbaiki/dirujuk sudah disebut. (`UX-01`)
- [ ] Desain terasa premium, bukan "AI/lebay": tanpa glassmorphism/glow, maks 1 warna
      dominan + 1 aksen (hue ≤ 3). (`UX-02`)
- [ ] Warna notifikasi/status konsisten (merah/kuning/hijau/biru; satu makna satu warna). (`UX-03`)
- [ ] Tidak memakai emoji keyboard atau icon set pasaran; ikon dari sumber terverifikasi
      (MCP/GitHub star > 1000). (`UX-04`)
- [ ] Halaman/menu baru mengikuti contoh halaman yang sudah ada (ukuran, warna, spacing,
      tipografi). (`UX-05`)
- [ ] Card/gambar/border memakai rasio tetap & proporsional. (`UX-06`)
- [ ] Semua ukuran mengikuti kelipatan/skala tetap. (`UX-07`)
- [ ] Satu warna diwujudkan sebagai palet gradasi (shade ramp), bukan nilai acak. (`UX-08`)
- [ ] Kontras aman: teks normal ≥ 4.5:1, teks besar & ikon/komponen ≥ 3:1. (`UX-09`)
- [ ] Dibangun mobile-first, tanpa scroll horizontal di mobile. (`UX-10`)
- [ ] Layout benar di seluruh breakpoint baku 320–1920px. (`UX-11`)
- [ ] Portrait & landscape (rotasi) sudah diuji; layout reflow tanpa kepotong. (`UX-12`)
