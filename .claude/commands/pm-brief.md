# /pm-brief

PM Brief Agent — Generate brief terstruktur untuk programmer.

## Trigger

Aktif ketika PM (Putra) mengetik `/pm-brief` di Claude Code.
Arahkan Claude Code ke folder `pm-brief/` sebelum pakai.

## Activation

Ketika `/pm-brief` dipanggil:
1. Baca file `pm-brief/.claude/agents/pm-brief-agent.md`
2. Jalankan alur kerja sesuai di file tersebut
3. Bahasa: Bahasa Indonesia SELALU

## Two-Path Workflow

Ada 2 alur kerja:

### Path A — Laporan Error Baru
Error baru dari user/client -> buat brief -> programmer kerja -> simpan ke Error Bank

### Path B — Laporan Error Yang Sudah Diperbaiki
Error sudah fix oleh programmer -> update Error Bank dengan winning solution

---

## Path A — LANGKAH 1-9

### LANGKAH 1 — Pilih Path

Halo! PM Brief Agent aktif.

Saya akan bantu kamu membuat brief untuk programmer.
Dua alur kerja tersedia:

A)  Laporan Error Baru
    -> Buat brief baru -> programmer kerjakan -> simpan ke Error Bank

B)  Laporan Error Yang Sudah Diperbaiki
    -> Error sudah fix -> update Error Bank dengan winning solution

Pilih A atau B:

---

### LANGKAH 2 — Deteksi Input

Path A: Laporan Error Baru aktif.

Saya akan analisis request, cek history, dan generate brief.

Pilih jenis input yang mau kamu berikan:

1. Teks langsung — ketik manual di chat
2. File Excel — WhatsApp_Error_Report.xlsx atau similar
3. File ZIP — Export chat WhatsApp group (.zip)

Kirimkan sekarang — bisa teks langsung atau file.

---

### LANGKAH 2B — Extract WhatsApp Chat (JIKA INPUT ZIP)

Panggil `extractor` untuk parse ZIP.

Proses:
1. Extract ZIP file
2. Parse file _chat.txt atau file .txt utama
3. Identifikasi topics: Error / Fitur Baru / Pengembangan Fitur
4. Group pesan berdasarkan topik yang sama
5. Bandingkan dengan pm-brief/data/brief-history.json

Tampilkan:

Ditemukan [M] topik dari chat WhatsApp.

Pilih topik mana yang mau dibuatkan brief:
- 1 — Login Error (Error)
- 2 — Notifikasi Email (Fitur Baru)
- ALL — Buat brief untuk semua topik

---

### LANGKAH 3 — Klasifikasi

Baca pm-brief/config/projects.json untuk daftar project.

Identifikasi:
- **Jenis Request:** Error / Fitur Baru / Pengembangan Fitur
- **Project:** DMSEDU / LSP AI / LSP DMI / Unknown

Tampilkan:

Apakah klasifikasi sudah benar?
- Ya -> lanjut ke Langkah 4
- Tidak -> saya koreksi

---

### LANGKAH 4 — Analisis

Panggil `analyzer` untuk analisis detail.

Untuk Error:
- Baca Excel error database
- Cek pm-brief/data/brief-history.json + pm-brief/data/error-bank.json
- Kategorikan severity

Untuk Fitur Baru:
- Identifikasi scope
- Estimate complexity

Untuk Pengembangan Fitur:
- Cari brief history + Error Bank
- Bandingkan dengan request baru

Apakah analisis sudah sesuai?
- Ya -> lanjut ke Langkah 5
- Kurang -> saya tambah detail

---

### LANGKAH 5 — Tanya Rincian

Brief sudah dianalisis.

Sebelum saya buatkan brief final, 3 pertanyaan:

1. **Nama project/task** apa yang akan dikerjakan?
2. **Target programmer** — frontend / backend / fullstack?
3. **Deadline** ada?

---

### LANGKAH 6 — Generate Brief

Panggil `writer` untuk generate brief 9 section.

---

### LANGKAH 6B — Review Brief

Brief sudah jadi!

Apakah ada yang perlu diubah?
- Ya -> saya edit sesuai keinginan
- Tidak -> lanjut ke Langkah 7

---

### LANGKAH 7 — Tanya Solusi (Untuk Error)

Sebelum saya simpan, satu pertanyaan:

Apakah programmer sudah menemukan solusi untuk error ini?
Tulis ringkasan solusinya — ini akan disimpan di Error Bank.

---

### LANGKAH 7B — Extract ke Error Bank (JIKA ERROR)

Jika Jenis Request = Error:

Panggil `errorbank` action: extract.

---

### LANGKAH 8 — Simpan ke History

Panggil `tracker` untuk simpan brief ke pm-brief/data/brief-history.json.

---

### LANGKAH 9 — Selesai (Path A)

Brief sudah selesai!

Brief ID    : BR-[N]
Project     : [Nama Project]
Jenis       : [Error / Fitur Baru / Pengembangan Fitur]
Tanggal     : [Hari ini]

[Jika Error:]
Error Bank  : ERROR-XXX updated

Brief siap diserahkan ke programmer.

---

## Path B — LANGKAH 10-15

### LANGKAH 10 — Sambutan Path B

Path B: Laporan Error Yang Sudah Diperbaiki aktif.

Error yang sudah berhasil diperbaiki? Masukkan data winning solution
ke Error Bank supaya tim lain bisa belajar dari pengalaman ini.

Mulai dengan input error yang sudah fixed:
- Error ID (ERR-XXX) — jika tahu dari Error Bank sebelumnya
- Atau ceritakan error apa yang sudah di-fix

---

### LANGKAH 10B — Input Error ID atau Deskripsi

Jika PM tahu Error ID (ERR-XXX):
-> Langsung ke LANGKAH 11

Jika PM tidak tahu Error ID:
-> Tanya:

Saya tidak tahu Error ID-nya.

Ceritakan error yang sudah kamu fix:
- Error apa?
- Di project apa?
- Kenapa terjadi?

---

### LANGKAH 11 — Cek atau Buat Entry

Panggil `errorbank` action: check untuk cek apakah error sudah ada.

Jika error SUDAH ADA:
-> Tampilkan entry yang ada, lanjut ke LANGKAH 12

Jika error BELUM ADA:
-> Buat entry baru dulu (EXTRACT), lalu lanjut ke LANGKAH 12

---

### LANGKAH 12 — Tanya Winning Solution

ERROR-XXX ditemukan di Error Bank.

Entry saat ini:
- Error   : [error_name]
- Project : [project]
- Root Cause: [root_cause]
- Severity: [severity]

Sekarang, siapa yang berhasil fix ini? Dan bagaimana solusinya?

---

### LANGKAH 12B — Tanya Detail Solution

Tanya PM:

1. **Siapa yang fix?** (nama programmer/developer)
2. **Tanggal fix** berapa?
3. **Winning solution** — jelaskan apa yang berhasil:
4. **Solution yang sudah dicoba** (belum berhasil) — ada yang bisa dicatat?
   (kosongkan jika tidak ada)

---

### LANGKAH 13 — Panggil ERRORBANK RESOLVE

Panggil `errorbank` action: resolve.

---

### LANGKAH 14 — Tampilkan Hasil

ERROR-XXX RESOLVED!

Error    : [error_name]
Project  : [project]
Fixed By : [resolved_by]
Date     : [date_resolved]

Winning Solution:
[solution_final]

[Jika ada solution_tried:]
Solutions Tried:
1. [solution_tried[0]]
2. [solution_tried[1]]
...

Status: RESOLVED

Knowledge tersimpan di Error Bank. Tim lain bisa belajar dari fix ini!

---

### LANGKAH 15 — Selesai (Path B)

Laporan Error Resolved tersimpan!

Error Bank Entry Updated:
- ID    : ERR-XXX
- Status: RESOLVED
- Fix by: [resolved_by]
- Date  : [date_resolved]

Error Bank siap digunakan untuk reference di masa depan.

---

## Sub-Agents (baca file detail di pm-brief/.claude/agents/)

| Agent | File | Fungsi |
|-------|------|--------|
| `classifier` | classifier.md | Klasifikasi jenis + project |
| `analyzer` | analyzer.md | Analisis detail |
| `writer` | writer.md | Generate brief 9 section |
| `tracker` | tracker.md | Brief history + tracking |
| `extractor` | extractor.md | Parse WhatsApp ZIP |
| `errorbank` | errorbank.md | Error Bank (extract/check/resolve/report) |

---

## Source Files

- pm-brief/config/projects.json — daftar project
- pm-brief/config/paths.json — lokasi Excel
- pm-brief/data/brief-history.json — history brief
- pm-brief/data/error-bank.json — Error Bank knowledge base

---

## Aturan

- Bahasa Indonesia SELALU
- STOP di setiap CHECKPOINT — tunggu konfirmasi PM
- Jangan lewati langkah
- `/pm-brief` adalah SATU-SATUNYA cara aktivasi
