# Maintenance Guide

Panduan teknis untuk merawat repo `skills-hub` ini: nambah skill baru, update,
hapus, sampai troubleshooting. Baca ini kalau kamu yang pegang repo — bukan
untuk end-user yang cuma mau install plugin (mereka cukup baca `README.md`).

## Sebelum mulai: pahami dulu 2 lapisan repo ini

1. **Git submodule** (folder `frontend/`, `backend/`, `qa/`, `mobile/`,
   `shared/`) — ini cuma buat **dokumentasi & tracking versi**, supaya orang
   bisa lihat/clone isi source skill secara lokal. Ini **tidak** berpengaruh
   ke apa yang muncul di Claude.ai.
2. **`.claude-plugin/marketplace.json`** — ini yang **benar-benar dibaca**
   Claude.ai/Claude Code untuk menampilkan daftar plugin. Submodule di atas
   dan file ini **terpisah**, harus di-maintain dua-duanya secara manual.

---

## 1. Menambah skill/plugin baru

### Langkah 1 — Cek dulu struktur repo sumbernya

Ini langkah paling penting, sering jadi sumber bug (lihat kasus `expo` &
`accessibility` sebelumnya). Clone dulu repo sumbernya, lalu cek:

```bash
git clone --depth 1 https://github.com/<owner>/<repo>.git /tmp/check
find /tmp/check -maxdepth 3 -path "*.claude-plugin*"
```

Ada 2 kemungkinan hasil:

| Hasil `find` | Artinya | Source yang dipakai di marketplace.json kita |
|---|---|---|
| `.claude-plugin/plugin.json` ada di **root** | Repo itu sendiri = 1 plugin | `{"source": "github", "repo": "<owner>/<repo>"}` |
| `.claude-plugin/marketplace.json` di root, tapi `plugin.json` ada di **subfolder** (misal `plugins/nama-plugin/`) | Repo itu sendiri = marketplace berisi banyak plugin, harus ambil subfolder-nya | `{"source": "git-subdir", "url": "https://github.com/<owner>/<repo>.git", "path": "plugins/nama-plugin", "ref": "main"}` |
| Tidak ada `plugin.json` sama sekali, cuma folder `skills/` langsung di root | Repo skill "polos" tanpa manifest plugin | `{"source": "github", "repo": "<owner>/<repo>"}, "strict": false, "skills": ["./skills"]` |

Kalau ragu, buka `.claude-plugin/marketplace.json` repo sumbernya (kalau ada)
dan lihat field `"source"` di situ — itu petunjuk paling akurat.

### Langkah 2 — Tambah sebagai submodule (dokumentasi)

```bash
git submodule add -b main https://github.com/<owner>/<repo>.git <folder-tim>/<nama-skill>
```

`<folder-tim>` = `frontend`, `backend`, `mobile`, `qa`, atau `shared` (kalau
dipakai lebih dari satu tim).

### Langkah 3 — Tambah entry ke `.claude-plugin/marketplace.json`

Tambahkan object baru ke array `"plugins"`, isi `source` sesuai tabel di
Langkah 1. Contoh (kasus paling umum, root = 1 plugin):

```json
{
  "name": "nama-plugin",
  "source": { "source": "github", "repo": "<owner>/<repo>" },
  "description": "Deskripsi singkat, jelas fungsinya buat apa",
  "category": "frontend"
}
```

Naikkan juga `metadata.version` di bagian atas file (semver bebas, misal
`1.3.1` → `1.4.0`).

### Langkah 4 — Validasi sebelum commit

```bash
python3 -c "import json; json.load(open('.claude-plugin/marketplace.json')); print('JSON valid')"
claude plugin validate .   # kalau Claude Code CLI terinstall
```

### Langkah 5 — Update dokumentasi

- Tambahkan baris baru di tabel "Struktur & Pemetaan Tim" di `README.md`.
- Kalau relevan untuk lebih dari 1 tim, catat juga di README tim lain
  (seperti catatan Mobile yang merujuk ke `ui-ux-pro-max` & `midscene`).

### Langkah 6 — Commit & push

```bash
git add .claude-plugin/marketplace.json .gitmodules README.md <folder-tim>/<nama-skill>
git commit -m "Add <nama-skill> plugin for <tim>"
git push origin main
```

### Langkah 7 — Sync di Claude.ai

Buka **Manage marketplaces** → cari marketplace `skills` → **Sync/Refresh**.
Plugin baru akan muncul di tab **Plugins → Discover** dalam beberapa saat.

---

## 2. Update skill yang sudah ada ke versi terbaru

```bash
cd <folder-tim>/<nama-skill>
git checkout main
git pull
cd -
git add <folder-tim>/<nama-skill>
git commit -m "Bump <nama-skill> to latest"
git push origin main
```

Kalau source di `marketplace.json` pakai `sha` yang di-pin (bukan cuma
`ref`), update juga sha-nya secara manual — jalankan
`git ls-remote https://github.com/<owner>/<repo>.git refs/heads/main` untuk
ambil commit terbaru.

## 3. Menghapus skill

```bash
git submodule deinit -f <folder-tim>/<nama-skill>
git rm -f <folder-tim>/<nama-skill>
rm -rf .git/modules/<folder-tim>/<nama-skill>
```

Lalu hapus juga entry-nya dari `.claude-plugin/marketplace.json`, commit,
push, dan **Sync** ulang marketplace di Claude.ai (plugin yang dihapus akan
otomatis ter-uninstall dari akun yang sudah install, sesuai warning di
"Manage marketplaces").

## 4. Kalau butuh "1 repo lagi" (repo baru dari nol)

Kalau suatu saat perlu bikin repo `skills-hub` versi lain (misal untuk unit
bisnis/organisasi berbeda), duplikasi struktur ini:

```
repo-baru/
├── .claude-plugin/marketplace.json   ← copy struktur dari repo ini, kosongkan "plugins"
├── .claude/settings.json             ← opsional, kalau mau extraKnownMarketplaces juga
├── <folder-tim>/                     ← sesuai kebutuhan tim di organisasi itu
├── README.md
└── MAINTENANCE.md                    ← copy file ini
```

Lalu ulangi Langkah 1-7 di atas untuk tiap skill yang mau dimasukkan.

---

## Known issues / Troubleshooting

### `git submodule update --recursive` gagal karena sub-submodule privat
Beberapa repo (misal `expo/skills`) punya submodule internal mereka sendiri
(`eval-harness`) yang privat/tidak publik. Ini **tidak mempengaruhi** isi
skill yang kita pakai. Solusi:
```bash
cd <folder-tim>/<nama-skill>
git submodule deinit -f <nama-sub-submodule-yang-gagal>
```

### Plugin tidak muncul di tab Discover Claude.ai
Kemungkinan penyebab:
1. **Source salah** (root vs subfolder) — cek ulang Langkah 1 di atas.
2. **Nama bentrok** dengan skill lain yang sudah aktif dari sumber berbeda
   di akun yang sama — coba cari manual lewat search bar di halaman Plugins.
3. **Belum sync** — marketplace di Claude.ai butuh di-refresh manual setelah
   push, tidak otomatis real-time.

### `sed`/edit lokal muncul warning "preserving permissions"
Aman diabaikan — cuma warning filesystem, bukan kegagalan proses.
