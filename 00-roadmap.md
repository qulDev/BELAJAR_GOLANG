# Roadmap Belajar Go

![Go Gopher BW](assets/images/go-gopher-mascot-bw.png)

Roadmap ini dipakai sebagai jalur eksekusi belajar. Jika kamu pemula absolut, tuntaskan fase fundamental dulu sebelum lanjut.

## Fase 1 — Fundamental (jalur utama pemula)

### Tujuan fase
Membangun fondasi Go dari nol: setup, sintaks inti, control flow, slices/maps, lalu masuk perlahan ke dasar low-level yang relevan untuk pemula.

### Jalur aksi (ikuti urutan, jangan lompat)
1. [01-fundamental/00-start-here.md](01-fundamental/00-start-here.md)
2. [01-fundamental/01-setup-go.md](01-fundamental/01-setup-go.md)
3. [01-fundamental/02-syntax-dan-tipe-data.md](01-fundamental/02-syntax-dan-tipe-data.md)
4. [01-fundamental/03-control-flow.md](01-fundamental/03-control-flow.md)
5. [01-fundamental/04-slices-maps-deep-dive.md](01-fundamental/04-slices-maps-deep-dive.md)
6. [01-fundamental/05-pointer-struct-method-dasar.md](01-fundamental/05-pointer-struct-method-dasar.md)
7. [01-fundamental/06-error-handling-dasar.md](01-fundamental/06-error-handling-dasar.md)
8. [01-fundamental/07-interface-dasar-composition.md](01-fundamental/07-interface-dasar-composition.md)
9. [01-fundamental/08-low-level-go-dasar.md](01-fundamental/08-low-level-go-dasar.md)
10. [01-fundamental/99-referensi-fundamental.md](01-fundamental/99-referensi-fundamental.md)
11. [01-fundamental/10-latihan-fundamental-dan-checklist.md](01-fundamental/10-latihan-fundamental-dan-checklist.md)

### Kriteria selesai fase fundamental
Kamu dinyatakan selesai jika sudah bisa:
- menyiapkan environment Go dan menjalankan program sendiri;
- menjelaskan tipe data, control flow, slices/maps, pointer, error handling, interface/composition, dan low-level dasar lewat contoh sederhana;
- menyelesaikan latihan kecil tiap materi tanpa copy-paste penuh;
- menuntaskan [10-latihan-fundamental-dan-checklist.md](01-fundamental/10-latihan-fundamental-dan-checklist.md) sebagai penutup fase fundamental.

## Fase 2 — Intermediate (setelah checklist fundamental selesai)

Masuk ke fase ini setelah langkah 11 di fundamental selesai:
1. [02-intermediate/01-modules-packages.md](02-intermediate/01-modules-packages.md)
2. [02-intermediate/02-testing-dasar.md](02-intermediate/02-testing-dasar.md)
3. [02-intermediate/03-file-io-json.md](02-intermediate/03-file-io-json.md)

## Fase 3 — Advanced (ringkas)
- Goroutine, channels, select
- Context dan cancellation
- Memory model + race detector

## Fase 4 — Expert (ringkas)
- Profiling dan tracing
- GC tuning dan PGO
- Fuzzing + integration coverage

## Fase 5 — Practice / Capstone (ringkas)
- Bangun mini REST API
- Terapkan test, race detection, dan profiling
- Review hasil dengan checklist

## Strategi belajar yang disarankan

1. **Belajar konsep** (baca materi).
2. **Implementasi kecil** (latihan 15-30 menit).
3. **Refleksi** (catat error/insight).
4. **Ulangi** sampai pola berpikirnya terbentuk.

