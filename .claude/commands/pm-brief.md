# /pm-brief

PM Brief Agent — Generate brief terstruktur untuk programmer.

## Trigger

Aktif ketika PM mengetik `/pm-brief` di Claude Code.

## Alur

1. PM ketik `/pm-brief`
2. Agent sambut + minta brief
3. Agent baca Excel dari `config/paths.json`
4. Agent generate brief document (8 section)
5. PM edit atau langsung pakai

## Konfigurasi

Lokasi Excel diatur di `config/paths.json` — edit file itu jika lokasi berubah.

## Format Output

Brief document berisi 8 section:
1. Ringkasan
2. Background & Konteks
3. Yang Diminta (Requirements)
4. Error / Masalah Teknis
5. Error Database Referensi
6. Acceptance Criteria
7. Catatan & Catatan Tambahan
8. Referensi
