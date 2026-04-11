# PM Brief Agent

Agent Claude Code untuk Project Manager membuat brief terstruktur bagi programmer, dengan sistem multi-agent untuk klasifikasi, analisis, dan tracking.

**Mendukung 3 jenis input:** Teks Langsung / File Excel / File ZIP Export Chat WhatsApp.

**Error Bank:** Knowledge base centralized untuk semua error, root cause, dan solusi.

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
│   ├── brief-history.json            ← Database brief (JSON, lokal)
│   └── error-bank.json              ← Error Bank knowledge base (JSON, lokal)
├── docs/
│   ├── WhatsApp_Error_Report_SAMPLE.xlsx
│   └── WhatsApp_Technical_ActionPlan_SAMPLE.xlsx
└── .claude/
    ├── commands/
    │   └── pm-brief.md              ← Command trigger (/pm-brief)
    └── agents/
        ├── pm-brief-agent.md         ← Orchestrator (agent utama)
        ├── classifier.md             ← Sub-agent: klasifikasi
        ├── extractor.md              ← Sub-agent: parse WhatsApp ZIP
        ├── analyzer.md               ← Sub-agent: analisis detail
        ├── writer.md                 ← Sub-agent: generate brief
        ├── errorbank.md             ← Sub-agent: Error Bank
        └── tracker.md                ← Sub-agent: tracking & history
```

---

## Alur Kerja Agent — Two-Path Workflow

```
PM ketik /pm-brief
       ↓
ORCHESTRATOR aktif
       ↓
LANGKAH 1: Pilih Path
       │
       ├── A) Laporan Error Baru
       │         ↓
       │    2. Deteksi Input (teks/Excel/ZIP)
       │         ↓
       │    3. CLASSIFIER → klasifikasi jenis + project
       │         ↓
       │    CHECKPOINT 1: Konfirmasi
       │         ↓
       │    4. ANALYZER → analisis detail
       │         ↓
       │    CHECKPOINT 2: Review
       │         ↓
       │    5. Tanya 3 pertanyaan
       │         ↓
       │    6. WRITER → generate brief
       │         ↓
       │    CHECKPOINT 3: Edit brief
       │         ↓
       │    7. ERROR BANK → EXTRACT error baru
       │         ↓
       │    8. TRACKER → simpan ke history
       │         ↓
       │    9. Selesai ✅
       │
       └── B) Laporan Error Yang Sudah Diperbaiki
                  ↓
             2. Input Error ID / Deskripsi
                  ↓
             3. ERROR BANK CHECK → cari entry
                  ↓
             4. Tanya Winning Solution
                  ↓
             5. ERROR BANK RESOLVE → update entry
                  ↓
             6. Selesai ✅
```

---

## Error Bank

Error Bank adalah knowledge base centralized untuk semua error, root cause, dan solusi.
Setiap brief dengan Error secara otomatis di-extract dan disimpan ke Error Bank.

### Gunanya Error Bank

| Manfaat | Penjelasan |
|---------|-----------|
| Knowledge Repository | Semua error + root cause + solusi di satu tempat |
| Prevent Repeat Errors | Error baru auto-check Error Bank sebelum fix |
| Root Cause Pattern | Bisa lihat pola error yang sama di project berbeda |
| Onboarding | Programmer baru bisa baca Error Bank untuk understand sistem |

### Error Bank Entry (v1.1)

```json
{
  "error_id": "ERR-001",
  "error_name": "Login Failed - Token Expired",
  "category": "Authentication",
  "project": "DMSEDU",
  "root_cause": "Token expiry time terlalu pendek (15 menit)",
  "solution": "Update expiry time jadi 24 jam + middleware refresh",
  "times_occurred": 4,
  "first_occurred": "2026-03-15",
  "last_occurred": "2026-04-09",
  "brief_ids": ["BR-001", "BR-003", "BR-007", "BR-012"],
  "severity": "HIGH",
  "status": "resolved",
  "notes": "",
  "solution_tried": [
    "Clear browser cache saja — tidak cukup",
    "Hanya refresh token tanpa middleware — token tetap expired"
  ],
  "solution_final": "Clear browser cache + refresh token jadi 24 jam + middleware refresh otomatis",
  "date_resolved": "2026-04-09",
  "resolved_by": "Andi (Backend Developer)"
}
```

### Kategori Error

| Kategori | Contoh |
|----------|--------|
| Authentication | Login gagal, token expired, session timeout |
| Database | Query error, connection failed |
| API | Timeout, 500 error, endpoint not found |
| UI/UX | Button tidak berfungsi, form error |
| Network | Connection refused, timeout |
| File/Upload | File too large, upload failed |
| Performance | Slow loading, memory leak |
| Security | Unauthorized access, CORS error |
| Integration | Third-party API error |
| Other | Tidak termasuk di atas |

---

## Brief History & Tracking

Setiap brief disimpan ke `data/brief-history.json` (lokal).

Brief dengan Error → juga masuk ke Error Bank.

---

## Catatan Penting

- Agent hanya aktif setelah `/pm-brief` diketik
- `data/brief-history.json` dan `data/error-bank.json` adalah data lokal — tidak di-push ke GitHub
- `config/paths.json` perlu di-update dengan path lokal masing-masing
- Semua Gespräch dengan agent dalam Bahasa Indonesia

---

## Lisensi

Open source — bebas digunakan dan dimodifikasi.
