# Skills Hub

Repo: [github.com/medilana/skills](https://github.com/medilana/skills)

Satu repo gabungan (via **git submodules**) untuk semua AI coding skill/tool yang
dipakai tim **Frontend, Backend, Mobile, dan QA**. Tiap submodule tetap
terhubung ke repo aslinya, jadi bisa di-update kapan saja tanpa perlu copy-paste
manual.

## Cara pakai

Clone dengan submodule langsung:

```bash
git clone --recurse-submodules https://github.com/medilana/skills.git
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
│   ├── playwright-skill/      → lackeyjb/playwright-skill
│   ├── accessibility-skill/   → deepakkamboj/claude-marketplace
│   ├── qa-use/                → browser-use/qa-use (bukan plugin, lihat catatan)
│   └── midscene/              → web-infra-dev/midscene (bukan plugin, lihat catatan)
├── mobile/
│   ├── expo-skills/           → expo/skills
│   └── appstore-review-skill/ → devsemih/appstore-review-skill
└── shared/
    └── mattpocock-skills/     → mattpocock/skills   (dipakai Frontend + Backend)
```

| Tim | Skill | Fungsi Singkat |
|---|---|---|
| Frontend | `frontend/ui-ux-pro-max-skill` | Generate design system otomatis (warna, tipografi, layout) lintas 22 stack |
| Frontend | `frontend/taste-skill` | Anti-slop UI: layout, motion, density lebih "manusiawi" |
| Frontend | `frontend/impeccable` | Audit/polish/critique kualitas UI & aksesibilitas |
| Backend | `backend/supabase-agent-skills` | Best practice Postgres/Supabase: schema, migration, RLS, query |
| Backend | `backend/cloudflare-skills` | Kelola Cloudflare Workers, D1, R2, KV, Hyperdrive |
| QA | `qa/playwright-skill` | Browser automation E2E testing berbasis bahasa natural |
| QA | `qa/accessibility-skill` | WCAG 2.1 AA compliance check, Playwright a11y testing |
| QA | `qa/qa-use` | Platform E2E testing berbasis AI agent (app mandiri, bukan plugin) |
| QA | `qa/midscene` | GUI agent E2E testing berbasis vision, lintas web/Android/iOS/desktop (SDK, bukan plugin) |
| Mobile | `mobile/expo-skills` | Skill resmi tim Expo: build UI, data fetching, deployment, upgrade SDK |
| Mobile | `mobile/appstore-review-skill` | Audit app sebelum submit ke App Store/Play Store |
| Shared (Frontend + Backend) | `shared/mattpocock-skills` | Workflow umum: TDD, code review, spec-to-ticket, debugging |

### Catatan soal Mobile

Selain `mobile/expo-skills` dan `mobile/appstore-review-skill`, skill lain
yang relevan untuk tim Mobile:
- **`frontend/ui-ux-pro-max-skill`** — mendukung stack SwiftUI, Jetpack Compose, React Native, Flutter untuk urusan desain.
- **`qa/midscene`** — bisa testing Android/iOS/HarmonyOS, bukan cuma web (SDK, jalan terpisah dari Claude Code).

## Maintenance (nambah/update/hapus skill)

Semua workflow teknis untuk merawat repo ini — nambah skill baru, update ke
versi terbaru, hapus, sampai troubleshooting — ada di **[MAINTENANCE.md](./MAINTENANCE.md)**.
Baca file itu kalau kamu yang pegang/maintain repo ini, bukan cuma pakai.

## Plugin Marketplace (Claude Code / Claude.ai)

Repo ini adalah **marketplace Claude Code aktif**. File `.claude-plugin/marketplace.json`
berisi daftar plugin nyata (bukan placeholder) yang sudah terverifikasi bisa
di-install lewat Claude.ai maupun Claude Code CLI.

### Cara pakai

**Di Claude.ai:** Customize → Plugins → Manage marketplaces → Add →
masukkan `https://github.com/medilana/skills` → Sync. Plugin akan muncul di
tab **Discover**.

**Di Claude Code CLI:**
```
/plugin marketplace add medilana/skills
/plugin install ui-ux-pro-max@skills-hub
```
(nama plugin persis bisa dicek lewat `/plugin` setelah marketplace ditambahkan)

### Plugin yang tersedia

| Plugin | Tim | Sumber asli |
|---|---|---|
| `ui-ux-pro-max` | Frontend, Mobile | nextlevelbuilder/ui-ux-pro-max-skill |
| `taste-skill` | Frontend | leonxlnx/taste-skill |
| `impeccable` | Frontend | pbakaus/impeccable |
| `cloudflare` | Backend | cloudflare/skills |
| `supabase-agent-skills` | Backend | supabase/agent-skills |
| `mattpocock-skills` | Frontend + Backend | mattpocock/skills |
| `playwright-skill` | QA | lackeyjb/playwright-skill |
| `accessibility` | QA | deepakkamboj/claude-marketplace (subfolder `plugins/accessibility`) |
| `expo` | Mobile | expo/skills (subfolder `plugins/expo`) |
| `appstore-review-skill` | Mobile | devsemih/appstore-review-skill |

> **Catatan:** `qa-use` dan `midscene` (ada sebagai submodule di folder
> `qa/`) **tidak** ada di daftar plugin di atas — keduanya bukan struktur
> Claude Code plugin. `qa-use` adalah aplikasi Next.js mandiri (jalan lewat
> Docker Compose), `midscene` adalah SDK testing biasa. Jalankan keduanya
> terpisah sesuai README masing-masing repo.

Untuk cara menambah/update/hapus plugin, lihat **[MAINTENANCE.md](./MAINTENANCE.md)**.

