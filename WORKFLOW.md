# Workflow Pemakaian Skill

Dokumen ini jawab satu pertanyaan: **"lagi ngerjain apa → skill/plugin apa
yang aku panggil?"** Bukan cara install (lihat `README.md`) atau cara rawat
repo (lihat `MAINTENANCE.md`) — ini murni panduan pemakaian sehari-hari.

Ada 2 jenis workflow:
- **Horizontal** — melibatkan Frontend, Backend, Mobile, QA sekaligus,
  biasanya di titik kickoff fitur/project baru. Tujuannya supaya 4 tim bisa
  mulai kerja **paralel**, bukan gantian nunggu.
- **Vertikal** — kerjaan harian di dalam satu tim, tidak perlu nunggu tim lain.

---

## 1. Workflow Horizontal — Kickoff Fitur/Project Baru

Kunci supaya 4 tim bisa paralel: **API spec dibuat & disepakati DULUAN**,
sebelum ada satu baris kode pun. Spec itu jadi "kontrak" — Frontend & Mobile
bisa langsung bangun UI pakai data palsu (mock) sesuai bentuk spec, tanpa
nunggu Backend selesai; QA bisa langsung siapkan skenario test dari spec
yang sama.

| Langkah | Siapa | Yang dikerjakan | Skill/Plugin dipanggil |
|---|---|---|---|
| 1. Definisikan kebutuhan fitur | Semua (PM + lead tiap tim) | Rapat singkat: apa yang dibangun, data apa yang dibutuhkan/dihasilkan | — (diskusi, bukan skill) |
| 2. Tulis API spec (OpenAPI/Swagger) | **Backend** | Definisikan endpoint, request/response, error case | Tidak ada skill khusus di marketplace ini — pakai **connector Postman** kalau sudah aktif, atau tulis manual di file `openapi.yaml` |
| 3. Review & sepakati spec | **Semua tim** | Frontend/Mobile/QA review bentuk data, kasih masukan sebelum dikunci | `mattpocock-skills` → `code-review` (bisa dipakai buat review dokumen spec juga, bukan cuma kode) |
| 4. **Mulai paralel** dari titik ini ⬇ | | | |

Setelah spec disepakati, 4 tim jalan **bersamaan**:

| Tim | Kerjaan paralel | Skill/Plugin dipanggil |
|---|---|---|
| **Backend** | Implementasi endpoint sesuai spec | `supabase-agent-skills` (kalau pakai Postgres/Supabase) atau `cloudflare` (kalau pakai Workers/D1) |
| **Frontend** | Bangun UI pakai mock data sesuai spec | `ui-ux-pro-max` (desain/layout) → `taste-skill` (polish) |
| **Mobile** | Bangun screen pakai mock data sesuai spec | `expo` (build UI, data fetching) |
| **QA** | Tulis skenario test dari spec (belum bisa run, tapi kerangkanya siap) | `playwright-skill` (siapkan skrip E2E draft) |

| Langkah | Siapa | Yang dikerjakan | Skill/Plugin dipanggil |
|---|---|---|---|
| 5. Integrasi | Frontend + Mobile | Ganti mock data → panggil API Backend beneran | — |
| 6. Code review lintas tim | Semua | Review PR sebelum merge | `mattpocock-skills` → `code-review`, `tdd` |
| 7. QA jalankan test penuh | QA | E2E test jalan ke API asli | `playwright-skill`, `accessibility` |
| 8. Pre-release check | Frontend, Mobile, QA | Audit terakhir sebelum rilis | `impeccable` (Frontend), `appstore-review-skill` (Mobile, kalau rilis ke store), `accessibility` (semua) |

---

## 2. Workflow Vertikal — Per Tim

Ini kerjaan harian yang tidak perlu nunggu/melibatkan tim lain.

### Frontend
| Situasi | Skill/Plugin |
|---|---|
| Mulai desain komponen/halaman baru (warna, tipografi, layout) | `ui-ux-pro-max` |
| Rasanya UI "kaku"/generic, mau lebih hidup (motion, density) | `taste-skill` |
| Sebelum PR di-merge, mau audit kualitas & aksesibilitas | `impeccable` |
| Kerjaan umum: TDD, debugging, spec-to-ticket | `mattpocock-skills` |

### Backend
| Situasi | Skill/Plugin |
|---|---|
| Desain schema, migration, RLS policy (Postgres/Supabase) | `supabase-agent-skills` |
| Kerja dengan Cloudflare Workers, D1, R2, KV | `cloudflare` |
| Kerjaan umum: TDD, code review, debugging | `mattpocock-skills` |

### Mobile
| Situasi | Skill/Plugin |
|---|---|
| Build UI/fitur di Expo/React Native | `expo` |
| Butuh referensi desain lintas platform (SwiftUI, Jetpack Compose, dll) | `ui-ux-pro-max` |
| Sebelum submit ke App Store/Play Store | `appstore-review-skill` |

### QA
| Situasi | Skill/Plugin |
|---|---|
| Bikin/jalankan E2E test browser dari bahasa natural | `playwright-skill` |
| Cek kepatuhan WCAG/aksesibilitas | `accessibility` |
| Testing lintas platform (web + Android/iOS/desktop) — tools terpisah, bukan plugin | `qa/midscene` (jalankan manual, lihat README repo aslinya) |

---

## Referensi cepat: semua plugin

| Plugin | Kategori |
|---|---|
| `ui-ux-pro-max` | frontend, mobile |
| `taste-skill` | frontend |
| `impeccable` | frontend |
| `cloudflare` | backend |
| `supabase-agent-skills` | backend |
| `mattpocock-skills` | frontend, backend |
| `playwright-skill` | qa |
| `accessibility` | qa |
| `expo` | mobile |
| `appstore-review-skill` | mobile |

Daftar ini otomatis jadi usang kalau ada plugin baru ditambahkan — sumber
kebenaran terbaru selalu ada di `.claude-plugin/marketplace.json`. Kalau
kamu nambah skill baru (lihat `MAINTENANCE.md`), update juga tabel di file
ini biar tetap sinkron.
