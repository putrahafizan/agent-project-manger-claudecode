# CHANGELOG — PM Brief Agent

All notable changes to this project will be documented in this file.

Format berdasarkan [Keep a Changelog](https://keepachangelog.com/).

---

## [1.0.0] — 2026-04-08

### Added

**Agent System:**
- `pm-brief-agent.md` — Orchestrator (agent utama, trigger `/pm-brief`)
- `classifier.md` — Sub-agent: klasifikasi jenis request + project
- `analyzer.md` — Sub-agent: analisis detail berdasarkan jenis request
- `writer.md` — Sub-agent: generate brief document 9 section
- `tracker.md` — Sub-agent: simpan ke history + tracking status

**Configuration:**
- `config/paths.json` — Konfigurasi lokasi file Excel
- `config/projects.json` — Daftar project + keywords untuk klasifikasi

**Database:**
- `data/brief-history.json` — Database brief dalam format JSON

**Documentation:**
- `README.md` — Panduan penggunaan
- `CLAUDE.md` — Dokumentasi internal untuk Claude Code
- `CHANGELOG.md` — Riwayat perubahan

**Sample Data:**
- `docs/WhatsApp_Error_Report_SAMPLE.xlsx` — Sample format error report
- `docs/WhatsApp_Technical_ActionPlan_SAMPLE.xlsx` — Sample action plan

**Command:**
- `.claude/commands/pm-brief.md` — Command trigger untuk Claude Code

### Features:

1. **Multi-Agent System** — Orchestrator + 4 sub-agents bekerja sama
2. **Klasifikasi Otomatis** — Deteksi jenis request + project dari input PM
3. **Error History** — Cek brief history untuk cegah error berulang
4. **9-Section Brief Format** — Ringkasan, requirements, acceptance criteria, dll
5. **Dampak ke User** — Kolom khusus untuk汇报 stakeholder
6. **Translate Bahasa Indonesia** — Root cause/solusi dari Excel diterjemahkan
7. **Configurable Project** — Tambah project baru via `config/projects.json`
8. **Configurable Path** — Ubah lokasi Excel via `config/paths.json`
9. **Brief History** — Semua brief disimpan untuk referensi future
10. **Checkpoints** — 3 titik konfirmasi PM sebelum brief final

### Jenis Request yang Didukung:

- **Error / Bug** — Kategori otomatis dari keywords
- **Fitur Baru** — Identifikasi scope + complexity
- **Pengembangan Fitur** — Delta analysis + impact

### Project yang Tersedia:

- DMSEDU
- LSP AI
- LSP DMI

(Customizable via `config/projects.json`)

---

## [0.1.0] — 2026-04-07

### Added (Initial Version)

- Single-file agent (CLAUDE.md di Downloads)
- Format brief 8 section
- Baca Excel error database
- Trigger command `/pm-brief`

### Notes

Versi awal — single agent tanpa sub-agents.
