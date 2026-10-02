# Prinsip

Dokumen ini menjelaskan **cara** menulis aturan anti-slop, bukan aturan domainnya.
Aturan domain ada di file per area. Aturan **proses** (perilaku agent: kapan membaca,
kapan bertanya, kapan mencari) ada di `workflow.md`.

> **Peta dokumen — wajib dibaca semua (WF-01).** Dokumen ini saling terikat; jangan
> membaca satu file saja. Baca juga: [Root README](../README.md) · [Indeks](README.md) ·
> [Prinsip](principles.md) · [Alur Kerja](workflow.md) · [Backend](backend.md) ·
> [Frontend](frontend.md) · [UI/UX](ui-ux.md) · [Database](database.md) · [Lainnya](others.md)

## Apa itu "slop"?

**Slop** = hasil (kode, UI, teks, skema) yang terlihat dibuat mesin karena **default**,
bukan karena **keputusan**.

Ciri: generik (bisa pasang di proyek apa pun), mengulang pola populer tanpa alasan, dan tak
ada satu pun pilihan yang bisa dijelaskan "kenapa".

Kebalikannya bukan "rapi", melainkan **spesifik & beralasan** — tiap pilihan bisa dijelaskan
dalam satu kalimat.

## Bentuk baku setiap aturan

Setiap aturan memakai bentuk yang sama:

- **Tell** — polanya: apa yang terlihat di kode/desain/output.
- **Why** — kenapa itu masalah; spesifik, boleh 1–3 kalimat, jangan bertele-tele.
- **Fix** — yang harus dilakukan sebagai gantinya.
- **Rule ID** — ditaruh di judul entri, format `### PREFIX-NN — Judul` (mis. `### DB-04 — ...`).

Contoh bentuk (placeholder, *bukan* aturan nyata):

```markdown
### <Judul singkat>

- **Tell:** <pola yang kelihatan slop>
- **Why:** <kenapa itu masalah>
- **Fix:** <yang harus dilakukan>
```

## Tier aturan

| Tier | Arti | Konsekuensi |
|---|---|---|
| **Hard** (Wajib) | Pelanggaran = tidak boleh selesai. | Harus dipenuhi sebelum rilis. |
| **Purpose** (Perlu alasan) | Teknik boleh dipakai, tapi wajib ada alasan tertulis. | Tak bisa dijelaskan → ubah. |
| **Quality** (Konsistensi) | Prioritas lebih rendah, demi kerapian. | Perbaiki bila sempat. |

Catatan: penandaan tier per aturan dilakukan bertahap; bagian ini hanya mendefinisikan
tiernya agar istilahnya baku.

## Penomoran

- Prefix per area (lihat `README.md`).
- Nomor naik berurutan, tidak pernah dipakai ulang, tidak pernah di-renumber.
- Kalau aturan dipindah area, pertahankan ID-nya dan tambahkan catatan.

## Gerbang Rilis (Delivery Gate)

Sebelum pekerjaan dinyatakan selesai, jalankan **semua checklist** dari area yang tersentuh
pekerjaan (bukan hanya satu), plus:

1. Semua file `docs/` sudah dibaca (`WF-01`).
2. Semua checklist area terkait = "ya".
3. Tidak ada aturan **Hard** yang dilanggar.

Kalau ada satu saja "tidak", pekerjaan **belum** selesai.

## Yang dilarang

- Mengarang aturan.
- Menaruh contoh seolah-olah nyata padahal belum dikonfirmasi pemilik.
- Memakai ulang nomor Rule ID atau meng-renumber aturan yang sudah ada.
