# PM Brief Agent

Agent Claude Code untuk Project Manager membuat brief terstruktur bagi programmer, dengan sistem multi-agent untuk klasifikasi, analisis, dan tracking.

**Mendukung 3 jenis input:** Teks Langsung / File Excel / File ZIP Export Chat WhatsApp.

---

## Cara Pakai

1. Buka Claude Code — arahkan ke folder `pm-brief/`
2. Ketik `/pm-brief`
3. Pilih jenis input: teks langsung / file Excel / file ZIP WhatsApp
4. Ikuti instruksi di layar
5. Brief siap diserahkan ke programmer

---

## Setup Awal

### 1. Konfigurasi Lokasi Excel

Buka `config/paths.json` — isi path lokasi file Excel kamu:

```json
{
  "excel_error_report": "ISI_PATH_DISINI/WhatsApp_Error_Report.xlsx",
  "excel_action_plan": "ISI_PATH_DISINI/WhatsApp_Technical_ActionPlan.xlsx"
}
```

### 2. Konfigurasi Project

Buka `config/projects.json` — edit atau tambah project sesuai kebutuhan:

```json
{
  "projects": [
    { "id": "dmsedu", "name": "DMSEDU", "keywords": ["dmsedu", "manajemen pendidikan"] },
    { "id": "lsp-ai", "name": "LSP AI", "keywords": ["lsp ai", "sertifikasi ai"] },
    { "id": "lsp-dmi", "name": "LSP DMI", "keywords": ["lsp dmi", "digital media"] }
  ]
}
```

---

## Struktur Folder

```
pm-brief/
├── CLAUDE.md                          ← Dokumentasi utama agent
├── README.md                          ← Panduan ini
├── CHANGELOG.md                       ← Riwayat perubahan
├── config/
│   ├── paths.json                    ← Lokasi file Excel
│   └── projects.json                 ← Daftar project + keywords
├── data/
│   └── brief-history.json            ← Database brief (JSON, lokal)
├── docs/
│   ├── WhatsApp_Error_Report_SAMPLE.xlsx
│   └── WhatsApp_Technical_ActionPlan_SAMPLE.xlsx
└── .claude/
    ├── commands/
    │   └── pm-brief.md              ← Command trigger (/pm-brief)
    └── agents/
        ├── pm-brief-agent.md        ← Orchestrator (agent utama)
        ├── classifier.md            ← Sub-agent: klasifikasi
        ├── extractor.md             ← Sub-agent: parse WhatsApp ZIP
        ├── analyzer.md              ← Sub-agent: analisis detail
        ├── writer.md                ← Sub-agent: generate brief
        └── tracker.md               ← Sub-agent: tracking & history
```

---

## Jenis Input yang Didukung

| Input | Cara Pakai | Fungsi |
|-------|-----------|--------|
| **Teks Langsung** | Ketik langsung di chat | Input manual PM |
| **File Excel** | Kirim file `.xlsx` | Database error report |
| **File ZIP** | Kirim export chat WhatsApp `.zip` | Parse WhatsApp group chat |

---

## Alur Kerja Agent

```
PM ketik /pm-brief
       ↓
ORCHESTRATOR (pm-brief-agent) aktif
       │
       ├─ 1. Sambutan & Pilih Input
       │
       ├─ [Jika ZIP] → EXTRACTOR → parse WhatsApp chat
       │       │
       │       └─ CHECKPOINT: Pilih topik untuk brief
       │
       ├─ 2. CLASSIFIER → klasifikasi jenis request + project
       │       │
       │       └─ CHECKPOINT 1: PM konfirmasi klasifikasi
       │
       ├─ 3. ANALYZER → analisis detail sesuai jenis request
       │       │
       │       └─ CHECKPOINT 2: PM review analisis
       │
       ├─ 4. Tanya 3 pertanyaan (task, programmer, deadline)
       │
       ├─ 5. WRITER → generate brief 9 section
       │       │
       │       └─ CHECKPOINT 3: PM edit brief
       │
       ├─ 6. Tanya solusi (untuk error)
       │
       ├─ 7. TRACKER → simpan ke history
       │
       └─ 8. Selesai — brief siap diserahkan
```

---

## WhatsApp Chat Export (ZIP)

Agent bisa menerima file ZIP hasil export chat WhatsApp group.

**Cara export dari WhatsApp:**
1. Buka grup WhatsApp
2. Setelan → Kirim Media → Export Chat (tanpa media)
3. ZIP akan ter-download

**Isi ZIP yang diproses:**
- `_chat.txt` — file chat utama (di-parse)
- File gambar/video — dideteksi sebagai attachment (tidak diproses isinya)

**Output EXTRACTOR:**
- Total pesan
- Topics teridentifikasi (Error / Fitur Baru / Pengembangan Fitur)
- Warning jika ada topik yang pernah ada di brief history
- Sample pesan dari setiap topik

---

## Jenis Request yang Didukung

| Jenis | Indicator |
|-------|-----------|
| **Error / Bug** | "error", "gagal", "crash", "tidak bisa", "bug" |
| **Fitur Baru** | "tambah fitur", "mau bikin", "butuh fitur baru" |
| **Pengembangan Fitur** | "upgrade", "improve", "perbaikan", "modifikasi" |

---

## Format Output Brief (9 Section + Comparison)

1. **Ringkasan** — APA yang diminta dan MENGAPA perlu dilakukan
2. **Background & Konteks** — dari mana request, masalah yang mendasari
3. **Yang Diminta (Requirements)** — list requirement spesifik
4. **Dampak ke User (After Fix)** — apa yang berubah setelah selesai
5. **Error / Masalah Teknis** — detail error (jika ada)
6. **Comparison dengan Temuan Sebelumnya** — warning jika error pernah terjadi
7. **Acceptance Criteria** — kriteria testable untuk tahu task selesai
8. **Catatan & Catatan Tambahan** — priority, complexity, dependencies
9. **Referensi** — file, database, brief history terkait

---

## Brief History & Tracking

Setiap brief yang dibuat disimpan ke `data/brief-history.json` (lokal, tidak di-push ke GitHub).

**Fungsi:**
- Mencegah error berulang (sistem cek error serupa sebelum generate)
- Tracking status brief (pending → on-progress → done)
- Korelasi error antar project
- Comparison otomatis saat input ZIP baru masuk

---

## Contoh Penggunaan

### Input: Teks Langsung
> "Ada error login di DMSEDU, user tidak bisa masuk setelah update tadi pagi"

Agent otomatis:
- Klasifikasi: Error + DMSEDU
- Cek Excel database + brief history
- Generate brief dengan acceptance criteria

### Input: File ZIP WhatsApp
> User export chat grup "Support DMSEDU" — 50+ pesan

Agent otomatis:
- EXTRACTOR: Parse chat, identifikasi 3 topik
- CLASSIFIER: Kategorikan setiap topik
- ANALYZER: Cek history untuk setiap topik
- WRITER: Generate brief per topik
- TRACKER: Simpan semua ke history

---

## Catatan Penting

- Agent hanya aktif setelah `/pm-brief` diketik
- `data/brief-history.json` adalah data lokal — tidak di-push ke GitHub
- `config/paths.json` perlu di-update dengan path lokal masing-masing
- Semua Gespräch dengan agent dalam Bahasa Indonesia

---

## Lisensi

Open source — bebas digunakan dan dimodifikasi.
