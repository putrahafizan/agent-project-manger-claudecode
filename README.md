# PM Brief Agent — Claude Code Tool untuk Project Manager

Agent ini membantu Project Manager membuat brief terstruktur untuk programmer,
dengan merujuk pada database error yang sudah terdokumentasi.

---

## Cara Pakai

1. Buka Claude Code
2. Ketik `/pm-brief`
3. Ikuti instruksi di layar

---

## Setup Awal

Sebelum pertama kali pakai, konfigurasi lokasi file Excel kamu:

1. Buka `config/paths.json`
2. Isi path sesuai lokasi file di komputer kamu:

```json
{
  "excel_error_report": "C:/Users/teamd/report_error_wa/WhatsApp_Error_Report.xlsx",
  "excel_action_plan": "C:/Users/teamd/report_error_wa/WhatsApp_Technical_ActionPlan.xlsx"
}
```

---

## Struktur Folder

```
pm-brief/
├── CLAUDE.md                              ← Dokumentasi agent
├── README.md                              ← Panduan ini
├── config/
│   └── paths.json                         ← Konfigurasi lokasi file Excel
├── docs/
│   ├── WhatsApp_Error_Report_SAMPLE.xlsx  ← Contoh format error report
│   └── WhatsApp_Technical_ActionPlan_SAMPLE.xlsx
└── .claude/
    ├── commands/
    │   └── pm-brief.md                    ← Command trigger
    └── agents/
        └── pm-brief-agent.md              ← Agent definition
```

---

## Alur Kerja Agent

```
PM ketik /pm-brief
       ↓
LANGKAH 1 → Sambutan + minta brief dari PM
LANGKAH 2 → Baca Excel error report + action plan (background)
LANGKAH 3 → Tanya nama project, target programmer, deadline
LANGKAH 4 → Generate BRIEF document (format 8 section)
LANGKAH 5 → Self-review sebelum output
LANGKAH 6 → Tawarkan edit sebelum diserahkaan ke programmer
```

---

## Format Output Brief

Brief yang dihasilkan memiliki 8 section:

1. Ringkasan
2. Background & Konteks
3. Yang Diminta (Requirements)
4. Error / Masalah Teknis (jika ada)
5. Error Database Referensi
6. Acceptance Criteria
7. Catatan & Catatan Tambahan
8. Referensi

---

## Catatan

- Agent hanya aktif setelah `/pm-brief` diketik
- Baca `CLAUDE.md` untuk detail lengkap alur kerja agent
- Jika lokasi file Excel berubah, update `config/paths.json`
