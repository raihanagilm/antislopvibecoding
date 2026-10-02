# Alur Kerja Agent — Anti-Slop Guide

> Prefix aturan: `WF-`
> Status: **terisi (9 aturan)**
> Ruang lingkup: cara AI agent berperilaku dan pendekatan koding dalam sesi vibe
> coding — kapan membaca, kapan bertanya, kapan mencari, dan bagaimana membangun.

> **Peta dokumen — wajib dibaca semua (WF-01).** Dokumen ini saling terikat; jangan
> membaca satu file saja. Baca juga: [Root README](../README.md) · [Indeks](README.md) ·
> [Prinsip](principles.md) · [Alur Kerja](workflow.md) · [Backend](backend.md) ·
> [Frontend](frontend.md) · [UI/UX](ui-ux.md) · [Database](database.md) · [Lainnya](others.md)

## Ruang lingkup

Aturan di sini mengatur **proses**, bukan isi kode. Tujuannya: agent tidak melompat
langsung ke eksekusi dan menebak-nebak. Isi per area teknis ada di file masing-masing
(`backend.md`, `frontend.md`, `ui-ux.md`, `database.md`, `others.md`).

## Aturan

### WF-01 — Baca dokumen dulu, baru eksekusi

- **Tell:** Agent langsung menulis/menjalankan kode tanpa membaca file panduan di
  `docs/` terlebih dahulu.
- **Why:** Tanpa membaca dulu, agent jatuh ke default generik — yang justru jadi slop.
  Panduan tidak ada gunanya kalau tidak dibaca sebelum beraksi.
- **Fix:** Sebelum mengeksekusi apa pun, baca **semua** file di `docs/` — `README.md`,
  `principles.md`, `workflow.md`, dan seluruh file area (`backend.md`, `frontend.md`,
  `ui-ux.md`, `database.md`, `others.md`) — **plus `README.md` di root** (peta struktur +
  kebijakan push). (`_TEMPLATE.md` cuma template, boleh dilewati.) Aturan di tiap file
  saling terikat; membaca sebagian saja berisiko melewatkan aturan. Baru setelah itu boleh
  mulai bekerja.

### WF-02 — Dokumen/konteks masih kosong? Tanya dulu, jangan menebak

- **Tell:** File panduan masih kosong atau konteks proyek belum ada, tapi agent
  langsung menebak dan mulai membangun (mis. langsung bikin struktur project sendiri).
- **Why:** Menebak berarti mengarang asumsi yang belum tentu sesuai kebutuhan, dan itu
  justru sumber slop. Lebih murah bertanya sekali daripada mengerjakan hal yang salah.
- **Fix:** Kalau dokumen masih kosong atau konteks belum jelas, berhenti dan tanya dulu
  dalam bahasa user: mau membangun apa (web app, CLI, API, mobile, script, lainnya)?
  Jangan eksekusi sebelum ada jawaban.

### WF-03 — Rekomendasi tech wajib pakai alasan (pro & kontra)

- **Tell:** Agent langsung memilih tech stack (atau mengikuti default populer) tanpa
  menjelaskan alasan maupun trade-off.
- **Why:** Pilihan tech tanpa alasan = default generik, dan user tidak bisa menilai
  risiko. Keputusan tech harus bisa dipertanggungjawabkan, bukan "karena umum dipakai".
- **Fix:** Tanya kebutuhan/target dulu, lalu beri beberapa opsi tech dengan
  rekomendasi + alasan *kenapa*, disertai **kelebihan dan kekurangan** tiap opsi.
  User yang memutuskan; agent hanya menyediakan informasinya.

### WF-04 — Buntu/frustrasi? Cari di GitHub (star > 1000)

- **Tell:** Agent buntu (≥ 2 percobaan gagal berturut-turut) atau user menyatakan frustrasi
  karena tidak ada progres, lalu agent mengarang solusi dari ketidaktahuan atau menyerah.
- **Why:** Mengarang solusi saat buntu menambah slop. Pola yang sudah terbukti biasanya ada
  di repo terbuka; bintang 1000+ jadi sinyal kualitas dan keterpakaian.
- **Fix:** Cari referensi di GitHub (repo **bintang > 1000**) atau lewat MCP/web, pakai
  sebagai rujukan, lalu **verifikasi dan sesuaikan** dengan konteks — bukan copy buta. Kalau
  **tidak ada akses jaringan**, bilang jujur ke user bahwa kamu tak bisa mencari, lalu
  berhenti dan tanya arahan — jangan mengarang.

### WF-05 — Bangun versi MVP dulu (jangan over-engineer)

- **Tell:** Agent langsung membangun fitur lengkap/berlebihan — banyak fitur,
  abstraksi, konfigurasi, dan "persiapan masa depan" — sebelum ada versi minimal yang
  benar-benar jalan.
- **Why:** Membangun besar di awal menghasilkan banyak kode yang belum tentu dipakai:
  sumber bug dan slop, serta susah diverifikasi. Kebutuhan inti jadi tenggelam.
- **Fix:** Bangun dulu **versi paling kecil yang jalan** dan menyelesaikan kebutuhan
  inti (MVP). Tahan diri dari fitur/abstraksi tambahan. Baru tambah fitur setelah MVP
  terbukti jalan.

### WF-06 — Dokumentasi di bagian paling atas file

- **Tell:** File kode langsung dimulai dengan import/statement tanpa penjelasan, atau
  agent tidak menulis dokumentasi sama sekali.
- **Why:** Tanpa header, pembaca (termasuk agent berikutnya) harus menebak tujuan file —
  pemicu duplikasi dan slop.
- **Fix:** Awali setiap file dengan dokumentasi singkat di bagian paling atas (header
  komentar / docstring): tujuan file + isi utamanya. **Ringkas** (1–3 baris); jangan
  bertele-tele atau sekadar mengulang kode (lihat semangat anti-slop komentar).

### WF-07 — Selalu update README.md

- **Tell:** Ada perubahan (fitur, struktur, cara pakai, atau tech) tapi `README.md`
  dibiarkan usang / tidak disentuh.
- **Why:** README usang justru menyesatkan. Dokumentasi yang tidak ikut berubah = slop
  dokumentasi.
- **Fix:** Setiap kali struktur/cara pakai/tech berubah, update `README.md` di pekerjaan
  yang sama. Sertakan juga update indeks `docs/README.md` bila aturan/area berubah.

### WF-08 — Skill/MCP wajib dipakai, dan dievaluasi dari feedback user

- **Tell:** Skill/MCP sudah dijalankan, lalu user mengoreksi suatu bagian (mis. "table ini
  gak cocok", "elemen ini salah") dan agent hanya memperbaiki di kode, tanpa mengevaluasi
  skill/MCP-nya.
- **Why:** Kalau koreksi berulang di hal yang sama, berarti skill/MCP-nya kurang tepat.
  Memperbaiki kode saja tidak mencegah masalah yang sama muncul lagi.
- **Fix:** Terapkan skill/MCP yang relevan secara **wajib** — selama sistem ini masih berupa
  dokumen (lihat root `README.md`), "skill/MCP" = **dokumen `docs/` ini sendiri**; kalau
  nanti sudah jadi skill/MCP nyata, pakai itu. Lalu perhatikan koreksi user: kalau ia
  mengoreksi bagian tertentu, anggap sinyal aturan kurang rekom, lalu **perbarui aturan**
  (tambah atau perketat). Catat perubahannya di **file area terkait** — ini beda dengan
  `WF-07` yang cuma soal memperbarui root `README.md`.

### WF-09 — Catat struktur folder

- **Tell:** Struktur folder proyek tidak tercatat; untuk tahu isi repo, agent harus mencari
  atau membaca file satu per satu.
- **Why:** Tanpa peta struktur, tiap perbaikan dimulai dengan "buta" — lambat, dan berisiko
  menyentuh tempat yang salah.
- **Fix:** Simpan peta struktur folder di file yang **dibaca WF-01** (mis. `docs/README.md`
  atau root `README.md`): daftar folder penting + isinya + file kunci. Kalau ditaruh di file
  yang tidak dibaca WF-01, peta itu **tidak akan terbaca**. Perbarui saat struktur berubah
  (nyambung `WF-07`). Agent membaca peta ini **dulu** sebelum menjelajah.

## Checklist Rilis

Semua jawaban harus **ya**:

- [ ] Sebelum eksekusi, **semua** file di `docs/` **+ root `README.md`** sudah dibaca. (WF-01)
- [ ] Kalau dokumen/konteks kosong, sudah bertanya ke user sebelum membangun. (WF-02)
- [ ] Setiap pilihan tech punya alasan + kelebihan dan kekurangan yang ditulis. (WF-03)
- [ ] Saat buntu, sudah mencari referensi GitHub (star > 1000) dan memverifikasinya,
      bukan mengarang. (WF-04)
- [ ] Yang dibangun adalah versi MVP yang jalan dulu, bukan fitur lengkap/over-engineered.
      (WF-05)
- [ ] Setiap file punya dokumentasi singkat di bagian paling atas. (WF-06)
- [ ] `README.md` sudah diupdate mengikuti perubahan terakhir. (WF-07)
- [ ] Skill/MCP sudah diterapkan, dan koreksi user sudah dievaluasi untuk memperbarui
      aturan. (WF-08)
- [ ] Peta struktur folder sudah sesuai kondisi repo terakhir. (WF-09)
