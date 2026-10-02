# antislopvibecoding

Panduan pribadi **anti-slop** untuk sesi *vibe coding*.

Tujuan: saat nge-vibe-code bareng AI agent, hasilnya tidak jadi generic/"slop AI" —
mencakup **backend, frontend, UI/UX, database**, dan area lain.

## Pendekatan saat ini: dokumen dulu

Repo ini dimulai sebagai kumpulan **dokumen markdown** (spec/panduan). Format akhirnya
(skill, MCP, atau sekadar dokumen) diputuskan **setelah isinya matang**.

## Untuk AI agent — baca semua dulu

> **Wajib (WF-01).** Sebelum mengeksekusi apa pun, baca **semua** file di `docs/` **plus
> file ini (`README.md`)**, bukan hanya satu. Aturan di tiap file saling terikat; membaca
> sebagian berisiko melewatkan aturan.

Urutan baca: `README.md` (root ini) → `docs/README.md` → `docs/principles.md` →
`docs/workflow.md` → `docs/backend.md` → `docs/frontend.md` → `docs/ui-ux.md` →
`docs/database.md` → `docs/others.md`.

## Mulai dari sini

- [docs/README.md](docs/README.md) — indeks, struktur, dan cara menambah aturan.
- [docs/principles.md](docs/principles.md) — definisi "slop" dan bentuk baku setiap aturan.
- [docs/_TEMPLATE.md](docs/_TEMPLATE.md) — template untuk mengisi tiap area.
- [docs/workflow.md](docs/workflow.md) — aturan proses untuk agent (baca dulu, tanya dulu, dll).

## Area

| Area | File | Prefix aturan | Status |
|---|---|---|---|
| Alur Kerja Agent | `docs/workflow.md` | `WF-xx` | terisi (9) |
| Backend | `docs/backend.md` | `BE-xx` | terisi (2) |
| Frontend | `docs/frontend.md` | `FE-xx` | terisi (3) |
| UI/UX | `docs/ui-ux.md` | `UX-xx` | terisi (12) |
| Database | `docs/database.md` | `DB-xx` | terisi (6) |
| Lainnya | `docs/others.md` | `OT-xx` | terisi (2) |

> Semua isi aturan ditulis oleh pemilik repo. Placeholder sengaja dibiarkan kosong
> agar tidak ada konten yang "dikarang". AI hanya merapikan format.

## Struktur folder

Peta struktur folder proyek (sumber kebenaran) ada di
**[docs/README.md → Struktur folder proyek](docs/README.md#struktur-folder-proyek)** —
supaya pasti terbaca oleh `WF-01`. Perbarui bila struktur berubah (`WF-09`).

## Dua jalur: sumber (Indonesia) → output (Inggris)

- **`docs/`** = **sumber kebenaran**, Bahasa Indonesia, untuk kamu (perbaikan & update antislopvibecoding).
- **`skills/`** = **output** Bahasa Inggris berformat `SKILL.md`, untuk dipakai AI agent.
- Kalau aturan di `docs/` berubah, **sinkronkan** ke `skills/` yang sesuai (ID sama: WF/BE/FE/UX/DB/OT).
- `skills/` mengikuti konvensi skill: satu folder per skill, berisi `SKILL.md` (frontmatter `name` / `description` / `allowed-tools`).

| Skill | Isi | Mirror dari |
|---|---|---|
| `antislop-vibecoding` | prinsip + alur kerja (WF-01…WF-09) | `principles.md` + `workflow.md` |
| `antislop-vibecoding-backend` | BE-01…BE-02 | `backend.md` |
| `antislop-vibecoding-frontend` | FE-01…FE-03 | `frontend.md` |
| `antislop-vibecoding-uiux` | UX-01…UX-12 | `ui-ux.md` |
| `antislop-vibecoding-database` | DB-01…DB-06 | `database.md` |
| `antislop-vibecoding-others` | OT-01…OT-02 | `others.md` |

## Push ke GitHub — mana yang dipush, mana yang tidak

> Bagian dari `OT-02` + `.gitignore`.

**DIPUSH (aman, tanpa data sensitif):**

- Semua file `*.md` (`README.md`, seluruh `docs/`)
- `.gitignore`
- File kode/konfigurasi non-rahasia (nanti bila ada)

**TIDAK DIPUSH (bisa mengandung data sensitif):**

- `.env`, `.env.*` — kredensial, koneksi DB, token
- `*.pem`, `*.key`, `*.p12`, `*.pfx`, `secrets/`, `credentials.json`, `service-account*.json`
- `node_modules/`, hasil build (`dist/`, `build/`, `.next/`, dll.), log, cache

**Aturan:**

- Jangan pernah commit rahasia (kunci, token, password, string koneksi). Pakai env var / secret manager.
- Kalau rahasia telanjur ter-commit: **rotasi/ubah** rahasianya, bukan sekadar hapus file.
- Sediakan `.env.example` (tanpa nilai asli) sebagai contoh.

### Setup dua akun GitHub (SSH)

Kredensial GitHub **dipakai bersama** antar tool, jadi akun dari satu IDE bisa "bocor" ke IDE lain (Akun A tidak bisa push ke repo Akun B). Solusinya: **SSH key terpisah per akun** lewat *host alias* di `~/.ssh/config`.

| Alias | Akun | Key | Dipakai untuk |
|---|---|---|---|
| `github-raihan` | `raihanagilm` | `~/.ssh/id_ed25519_raihan` | **VS Code**, repo ini |
| `github-intervuai` | `meintervuai` | `~/.ssh/id_ed25519_intervuai` | Antigravity |

Remote repo ini memakai alias: `git@github-raihan:raihanagilm/antislopvibecoding.git`

- **Tes koneksi:** `ssh -T git@github-raihan` → harus membalas `Hi raihanagilm!`
- **Pushing:** `git push -u origin main`
- **Clone repo akun lain:** `git clone git@github-intervuai:<user>/<repo>.git`

> Public key tiap akun harus didaftarkan di GitHub (Settings → SSH and GPG keys) sebelum bisa dipakai.

Catatan: repo ini **dokumen saja** — belum ada kode/data sensitif; `.gitignore` disiapkan untuk proyek turunan.