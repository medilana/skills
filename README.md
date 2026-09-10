# Skills Hub

Satu repo gabungan (via **git submodules**) untuk semua AI coding skill/tool yang
dipakai tim **Frontend, Backend, Mobile, dan QA**. Tiap submodule tetap
terhubung ke repo aslinya, jadi bisa di-update kapan saja tanpa perlu copy-paste
manual.

## Cara pakai

Clone dengan submodule langsung:

```bash
git clone --recurse-submodules <url-repo-ini>
```

Kalau sudah terlanjur clone biasa:

```bash
git submodule update --init --recursive
```

Update semua submodule ke commit terbaru dari repo asal masing-masing:

```bash
git submodule update --remote --merge
```

## Struktur & Pemetaan Tim

```
skills-hub/
├── frontend/
│   ├── ui-ux-pro-max-skill/   → nextlevelbuilder/ui-ux-pro-max-skill
│   ├── taste-skill/           → leonxlnx/taste-skill
│   └── impeccable/            → pbakaus/impeccable
├── backend/
│   ├── supabase-agent-skills/ → supabase/agent-skills
│   └── cloudflare-skills/     → cloudflare/skills
├── qa/
│   ├── qa-use/                → browser-use/qa-use
│   └── midscene/              → web-infra-dev/midscene
├── shared/
│   └── mattpocock-skills/     → mattpocock/skills   (dipakai Frontend + Backend)
└── mobile/                    → lihat catatan di bawah
```

| Tim | Skill | Fungsi Singkat |
|---|---|---|
| Frontend | `frontend/ui-ux-pro-max-skill` | Generate design system otomatis (warna, tipografi, layout) lintas 22 stack |
| Frontend | `frontend/taste-skill` | Anti-slop UI: layout, motion, density lebih "manusiawi" |
| Frontend | `frontend/impeccable` | Audit/polish/critique kualitas UI & aksesibilitas |
| Backend | `backend/supabase-agent-skills` | Best practice Postgres/Supabase: schema, migration, RLS, query |
| Backend | `backend/cloudflare-skills` | Kelola Cloudflare Workers, D1, R2, KV, Hyperdrive |
| QA | `qa/qa-use` | Platform E2E testing berbasis AI agent, test case bahasa natural |
| QA | `qa/midscene` | GUI agent E2E testing berbasis vision, lintas web/Android/iOS/desktop |
| Shared (Frontend + Backend) | `shared/mattpocock-skills` | Workflow umum: TDD, code review, spec-to-ticket, debugging |

### Catatan soal Mobile

Belum ada skill yang murni native mobile (logic Swift/Kotlin, publish App
Store/Play Store). Yang paling relevan untuk tim Mobile:
- **`frontend/ui-ux-pro-max-skill`** — mendukung stack SwiftUI, Jetpack Compose, React Native, Flutter untuk urusan desain.
- **`qa/midscene`** — bisa testing Android/iOS/HarmonyOS, bukan cuma web.

## Menambah skill baru

```bash
git submodule add -b main <url-repo> <folder-tim>/<nama-skill>
git commit -m "Add <nama-skill> submodule"
```

## Update satu submodule saja

```bash
cd <folder-tim>/<nama-skill>
git checkout main
git pull
cd -
git add <folder-tim>/<nama-skill>
git commit -m "Bump <nama-skill> to latest"
```
