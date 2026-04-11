# CHANGELOG — PM Brief Agent

---

## [1.3.0] — 2026-04-09

### Added

**Two-Path Workflow:**

Orchestrator sekarang punya 2 pilihan alur kerja di LANGKAH 1:

- **Path A — Laporan Error Baru**: Brief baru -> programmer kerja -> extract ke Error Bank (existing flow)
- **Path B — Laporan Error Yang Sudah Diperbaiki**: Update Error Bank dengan winning solution (tanpa brief flow)

**Error Bank Mode RESOLVE:**

Sub-agent `errorbank.md` tambah Mode 5: RESOLVE

- Input: error_id, solution_final, resolved_by, date_resolved, solution_tried[]
- Action: update entry -> set status "resolved" -> simpan winning solution
- Append ke solution_tried[] (jangan overwrite yang sudah ada)

**Error Bank Entry Schema v1.1:**

Tambah field baru untuk track resolved error:

```json
{
  "solution_tried": [],
  "solution_final": "",
  "date_resolved": "",
  "resolved_by": ""
}
```

### Changed

**Orchestrator (pm-brief-agent.md):**

- LANGKAH 1: Tambah pilihan Path A vs Path B
- Path A: LANGKAH 2-10 (existing flow, renumbered)
- Path B: LANGKAH 11-16 (resolved error flow)
- Update sub-agents table: ERRORBANK sekarang termasuk resolve

**Command file (.claude/commands/pm-brief.md):**
- Rewrite lengkap dari versi lama (8-step tanpa pilihan)
- Tulis Two-Path Workflow secara eksplisit
- Path A: 9 langkah, Path B: 6 langkah
- PENTING: File ini yang dibaca saat `/pm-brief` dipanggil

**Error Bank (errorbank.md):**

- Tambah Mode 5: RESOLVE
- Update Error Bank Entry Format ke v1.1
- Update tips: tambahkan "solution tried penting"
- REPORT: tambah section "Recently Resolved"

### Error Bank v1.1 Features

- Tracking winning solution untuk error yang sudah fix
- Catat siapa yang fix + tanggal fix
- Catat semua solusi yang sudah dicoba (failed attempts)
- Status "resolved" untuk error yang sudah diperbaiki
- REFERENCE: tim lain bisa lihat apa yang sudah berhasil dan apa yang tidak

---

## [1.2.0] — 2026-04-09

### Added

**Error Bank System:**
- `data/error-bank.json` — Error Bank knowledge base database
- `.claude/agents/errorbank.md` — Sub-agent untuk manage Error Bank
- EXTRACT: Ambil error info dari brief, simpan ke Error Bank
- QUERY: Cari error yang sudah ada di Error Bank
- CHECK: Quick check apakah error sudah ada
- REPORT: Generate summary Error Bank

**Error Bank Integration:**
- Orchestrator updated: Error Bank check saat ANALYZER
- Orchestrator updated: Error Bank extract saat brief selesai (LANGKAH 7B)
- Brief dengan Error -> otomatis masuk ke Error Bank
- Brief dengan Fitur Baru / Pengembangan Fitur -> tidak masuk Error Bank

### Changed

**Orchestrator (pm-brief-agent.md):**
- Tambah ERRORBANK ke list sub-agents
- LANGKAH 7B: Extract ke Error Bank (untuk Error saja)
- LANGKAH 9: Tanya Simpan Brief (untuk non-Error)
- LANGKAH 10: Selesai (re-numbered dari 9 ke 10)

### Error Bank Features

- Counter otomatis: times_occurred += 1 setiap error muncul lagi
- Root cause tracking: selalu update root_cause terbaru
- Solution tracking: simpan semua solusi yang pernah dipakai
- Link ke brief IDs: tracking error muncul di brief mana saja
- Severity + Category: untuk filtering dan reporting

---

## [1.1.0] — 2026-04-08

### Added

**EXTRACTOR Sub-Agent:**
- `.claude/agents/extractor.md` — Parse ZIP export chat WhatsApp
- Identifikasi topics dari chat (Error / Fitur Baru / Pengembangan Fitur)
- Group pesan berdasarkan topik
- Comparison dengan brief history otomatis

**WhatsApp ZIP Input Support:**
- Orchestrator: 3 input types (teks / Excel / ZIP)
- Brief format: section "Comparison dengan Temuan Sebelumnya"

---

## [1.0.0] — 2026-04-08

### Added

- `pm-brief-agent.md` — Orchestrator
- `classifier.md` — Klasifikasi jenis request + project
- `analyzer.md` — Analisis detail
- `writer.md` — Generate brief 9 section
- `tracker.md` — Brief history + tracking
- `config/paths.json` — Lokasi Excel
- `config/projects.json` — Daftar project
- `README.md`, `CLAUDE.md`, `CHANGELOG.md`
- `docs/WhatsApp_Error_Report_SAMPLE.xlsx`
- `docs/WhatsApp_Technical_ActionPlan_SAMPLE.xlsx`