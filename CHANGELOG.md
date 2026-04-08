# CHANGELOG — PM Brief Agent

---

## [1.1.0] — 2026-04-08

### Added

**EXTRACTOR Sub-Agent:**
- `.claude/agents/extractor.md` — Parse ZIP export chat WhatsApp
- Identifikasi topics dari chat (Error / Fitur Baru / Pengembangan Fitur)
- Group pesan berdasarkan topik
- Comparison dengan brief history otomatis

**WhatsApp ZIP Input Support:**
- Orchestrator updated: deteksi 3 jenis input (teks / Excel / ZIP)
- Brief format: tambah section "Temuan dari WhatsApp Chat"
- Brief format: tambah section "Comparison dengan Temuan Sebelumnya"
- EXTRACTOR berjalan sebelum CLASSIFIER jika input ZIP

### Changed

**Orchestrator (pm-brief-agent.md):**
- Update alur kerja dari 8 ke 9 langkah
- LANGKAH 1: Pilihan jenis input (teks / Excel / ZIP)
- LANGKAH 2B: EXTRACTOR untuk ZIP input
- CHECKPOINT 2B: Pilih topik untuk brief (jika multiple topics)
- LANGKAH 6: Writer tambah section untuk WhatsApp input

**README.md:**
- Tambah section "Jenis Input yang Didukung"
- Update alur kerja diagram
- Tambah "WhatsApp Chat Export (ZIP)" section
- Update list sub-agents

---

## [1.0.0] — 2026-04-08

### Added

**Agent System:**
- `pm-brief-agent.md` — Orchestrator
- `classifier.md` — Klasifikasi jenis request + project
- `analyzer.md` — Analisis detail berdasarkan jenis request
- `writer.md` — Generate brief document 9 section
- `tracker.md` — Brief history + status tracking

**Configuration:**
- `config/paths.json` — Lokasi file Excel
- `config/projects.json` — Daftar project + keywords

**Database:**
- `data/brief-history.json` — Database brief (JSON)

**Documentation:**
- `README.md`, `CLAUDE.md`, `CHANGELOG.md`

**Sample Data:**
- `docs/WhatsApp_Error_Report_SAMPLE.xlsx`
- `docs/WhatsApp_Technical_ActionPlan_SAMPLE.xlsx`
