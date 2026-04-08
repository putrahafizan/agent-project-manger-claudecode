# PM Brief Agent

Agent Claude Code untuk Project Manager membuat brief terstruktur bagi programmer, dengan sistem multi-agent untuk klasifikasi, analisis, dan tracking.

---

## Cara Pakai

1. Buka Claude Code — arahkan ke folder `pm-brief/`
2. Ketik `/pm-brief`
3. Ikuti instruksi di layar
4. Brief siap diserahkan ke programmer

---

## Setup Awal

### 1. Konfigurasi Lokasi Excel

Buka `config/paths.json` — isi path lokasi file Excel kamu:

```json
{
  "excel_error_report": "C:/Users/teamd/report_error_wa/WhatsApp_Error_Report.xlsx",
  "excel_action_plan": "C:/Users/teamd/report_error_wa/WhatsApp_Technical_ActionPlan.xlsx"
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
├── CLAUDE.md                          ← Dokumentasi utama agent (dibaca Claude Code)
├── README.md                          ← Panduan ini
├── CHANGELOG.md                       ← Riwayat perubahan
├── config/
│   ├── paths.json                    ← Lokasi file Excel
│   └── projects.json                 ← Daftar project + keywords
├── data/
│   └── brief-history.json            ← Database brief (JSON)
├── docs/
│   ├── WhatsApp_Error_Report_SAMPLE.xlsx
│   └── WhatsApp_Technical_ActionPlan_SAMPLE.xlsx
└── .claude/
    ├── commands/
    │   └── pm-brief.md              ← Command trigger (/pm-brief)
    └── agents/
        ├── pm-brief-agent.md        ← Orchestrator (agent utama)
        ├── classifier.md            ← Sub-agent: klasifikasi
        ├── analyzer.md              ← Sub-agent: analisis detail
        ├── writer.md                ← Sub-agent: generate brief
        └── tracker.md               ← Sub-agent: tracking & history
```

---

## Alur Kerja Agent

```
PM ketik /pm-brief
       ↓
ORCHESTRATOR (pm-brief-agent) aktif
       │
       ├─ 1. Sambutan
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

## Jenis Request yang Didukung

| Jenis | Indicator |
|-------|-----------|
| **Error / Bug** | "error", "gagal", "crash", "tidak bisa", "bug" |
| **Fitur Baru** | "tambah fitur", "mau bikin", "butuh fitur baru" |
| **Pengembangan Fitur** | "upgrade", "improve", "perbaikan", "modifikasi" |

---

## Format Output Brief (9 Section)

1. **Ringkasan** — APA yang diminta dan MENGAPA perlu dilakukan
2. **Background & Konteks** — dari mana request, masalah yang mendasari
3. **Yang Diminta (Requirements)** — list requirement spesifik
4. **Dampak ke User (After Fix)** — apa yang berubah setelah selesai
5. **Error / Masalah Teknis** — detail error (jika ada)
6. **Error Database Referensi** — solusi dari Excel / history
7. **Acceptance Criteria** — kriteria testable untuk tahu task selesai
8. **Catatan & Catatan Tambahan** — priority, complexity, dependencies
9. **Referensi** — file, database, brief history terkait

---

## Brief History & Tracking

Setiap brief yang dibuat disimpan ke `data/brief-history.json`.

**Fungsi:**
- Mencegah error berulang (sistem cek error serupa sebelum generate)
- Tracking status brief (pending → on-progress → done)
- Korelasi error antar project

**Contoh output tracking:**

```
✅ Brief BR-001 berhasil disimpan!

Brief ID    : BR-001
Project     : DMSEDU
Jenis       : Error
Tanggal     : 2026-04-08

Total brief dalam history: 1
```

---

## Contoh Penggunaan

### Error Report:
> "Ada error login di DMSEDU, user没法 masuk setelah update tadi pagi"

Agent otomatis:
- Klasifikasi: Error + DMSEDU
- Cek Excel database + brief history
- Analisis severity dan solusi yang pernah dipakai
- Generate brief dengan acceptance criteria

### Fitur Baru:
> "Saya mau bikin fitur notifikasi email untuk LSP AI"

Agent otomatis:
- Klasifikasi: Fitur Baru + LSP AI
- Identifikasi scope dan complexity
- Generate brief dengan dependencies

### Pengembangan Fitur:
> "Upgrade dashboard DMSEDU biar lebih user-friendly"

Agent otomatis:
- Klasifikasi: Pengembangan Fitur + DMSEDU
- Cari brief history dashboard lama
- Bandingkan dengan request baru
- Generate brief dengan delta analysis

---

## Catatan Penting

- Agent hanya aktif setelah `/pm-brief` diketik
- Baca `CLAUDE.md` untuk detail lengkap alur kerja internal
- Jika lokasi Excel berubah → update `config/paths.json`
- Jika ada project baru → edit `config/projects.json`
- Semua Gespräch dengan agent dalam Bahasa Indonesia

---

## Lisensi

Open source — bebas digunakan dan dimodifikasi.
