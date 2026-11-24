# Caliper

**Brief-to-Design Orchestrator Plugin untuk Tim UI/UX di Antigravity**

Caliper (`/caliper`) adalah *4-Gate Pipeline* untuk mengubah PRD, User Story, Acceptance Criteria (AC), atau product brief menjadi layar UI/UX yang patuh Design System dan terverifikasi secara visual di **Figma** maupun **FigCraft** — tanpa tebak-tebakan (*zero guesswork*).

```text
[Gate 1: Brief & Gap Check] ──(HARD STOP #1)──▶ [Gate 2: Screen Contract & DS Audit] ──(HARD STOP #2)──▶ [Gate 3: Canvas Execution] ──▶ [Gate 4: Evidence-Based QA]
```

---

## Instalasi

### Windows (PowerShell)

```powershell
git clone https://github.com/rhmdee/caliper.git "$env:USERPROFILE\.gemini\config\plugins\caliper"
```

### macOS / Linux

```bash
git clone https://github.com/rhmdee/caliper.git ~/.gemini/config/plugins/caliper
```

### Menggunakan GitHub CLI (`gh`)

```bash
# Windows (PowerShell)
gh repo clone rhmdee/caliper "$env:USERPROFILE\.gemini\config\plugins\caliper"

# macOS / Linux
gh repo clone rhmdee/caliper ~/.gemini/config/plugins/caliper
```

Setelah di-clone, buka halaman **Customizations / Plugins** di Antigravity dan pastikan plugin **Caliper** dalam status aktif (*enabled*).

### Update ke Versi Terbaru

```bash
# Windows (PowerShell)
git -C "$env:USERPROFILE\.gemini\config\plugins\caliper" pull

# macOS / Linux
git -C ~/.gemini/config/plugins/caliper pull
```

---

## Prasyarat Plugin & MCP

| Plugin / MCP | Status | Kegunaan |
| :--- | :--- | :--- |
| **Figma** (plugin Marketplace) *atau* **FigCraft MCP** | **Wajib** *(pilih sesuai kanvas)* | Dibutuhkan agar agent dapat mengaudit Design System dan menggambar di Figma (`figma-use`) atau FigCraft |
| **`gdrive` & `gsheets`** (plugin Marketplace) | Opsional | Hanya dibutuhkan jika PRD/AC diberikan melalui tautan Google Drive atau Google Sheets |
| **`superpowers`** (plugin Marketplace) | Opsional | Menambah dukungan `brainstorming` (Gate 1) & `verification-before-completion` (Gate 4). Jika tidak terpasang, Caliper otomatis memakai fallback verifikasi mandiri |
| **`anti-slop`** (plugin Marketplace) | Opsional | Tambahan filter `antislop-copywriting` & `antislop-human`. Aturan inti copywriting sudah tertanam di `rules/AGENTS.md` |

---

## Isi Paket Plugin

### Skills (`skills/`)

| Skill | Peran di Pipeline |
| :--- | :--- |
| **`caliper`** (`/caliper`) | Orchestrator utama 4-Gate Pipeline (Brief → Screen Contract → Canvas Execution → Evidence-Based QA) |
| `product-thinking-uiux` | Gate 1: Ekstraksi kebutuhan bisnis, aktor, flow makro, KPI, dan daftar `[Perlu Konfirmasi]` |
| `generate-prd` | Gate 1 *(opsional)*: Membantu menyusun PRD jika input masih berupa ide mentah |
| `ui-end-to-end-flow` | Gate 2–4: Flow map, Screen Contract, happy path & edge cases, serta QA checklist FigCraft |
| `figma-design-system-composer` | Gate 2–3 *(Figma)*: Audit library/token DS, publish check, dan komposisi instance komponen |
| `figma-prototype-composer` | Gate 3 *(Figma, opsional)*: Prototyping interaktif berbasis variable & reaction |
| `figma-uiux-reviewer` | Gate 4 *(Figma)*: Audit UI/UX dual-layer (telemetri + screenshot + traceability AC) |
| `ux-audit` | Gate 4 *(opsional)*: Evaluasi heuristik UX, aksesibilitas, dan kejelasan copywriting |

### Rules (`rules/AGENTS.md`)

Aktif otomatis selama plugin **Caliper** menyala:
- **Struktur Overlay & Popup:** Auto Layout layar utama tetap terjaga, scrim menggunakan fill opacity pada `*Overlay`, dan `*Content` menjadi child langsung.
- **Copywriting UI Bersih:** Bebas kode dokumen internal (`US-xx`, `BR-xx`), catatan rapat/FGD, atau label `(contoh)`.
- **Judul Board Anti-Slop:** Bebas jargon metodologi dan bebas em-dash (`—`).
- **Notifikasi Toast:** Posisi pojok kanan-bawah tanpa wrapper viewport, tombol tutup `×` khusus error toast.
- **Transparansi Wrapper:** Semua frame pembungkus layout wajib transparan (`fills: []`).

---

## Cara Menggunakan

Panggil `/caliper` diikuti brief, PRD, User Story, atau link dokumen spesifikasi:

```text
/caliper buatkan desain alur pengajuan cuti dari PRD ini untuk Figma menggunakan Design System yang sedang terbuka.
```

```text
/caliper bedah User Story dan Acceptance Criteria berikut lalu susun Screen Contract sebelum masuk ke FigCraft.
```

### Alur Interaksi Singkat
1. **Gate 1 (Brief & Gap Check):** Agent membedah kebutuhan dan berhenti (**Hard Stop #1**) jika ada celah `[Perlu Konfirmasi]`, sekaligus menanyakan kanvas target (`Figma` / `FigCraft`).
2. **Gate 2 (Screen Contract & Library Audit):** Agent mengaudit komponen DS dan menyajikan **Screen Contract Table**, lalu berhenti (**Hard Stop #2**) menunggu persetujuanmu.
3. **Gate 3 (Canvas Execution):** Setelah disetujui, agent membangun seluruh layar secara otonom menggunakan instance Design System.
4. **Gate 4 (Evidence-Based QA):** Agent memeriksa screenshot hasil render, memperbaiki teks terpotong/overlap/hardcoded value, lalu melaporkan hasil beserta bukti visualnya.
