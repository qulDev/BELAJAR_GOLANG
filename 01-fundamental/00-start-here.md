# Start Here: Fundamental Go untuk Pemula Total

Selamat datang di level fundamental. Bagian ini disusun seperti seri blog: kamu belajar dari nol, bertahap, sampai masuk ke dasar-dasar low-level Go.

## Cara Pakai Bagian Ini (Untuk Pemula Absolut)
1. Ikuti urutan materi sesuai nomor (jangan lompat dulu).
2. Ketik ulang contoh kode, lalu jalankan sendiri (`go run .` / `go test ./...`).
3. Fokus paham konsep inti sebelum lanjut ke deep dive.
4. Kalau bingung, ulang artikel sebelumnya lalu lanjut lagi.
5. Simpan catatan error yang kamu temui; itu bagian penting dari proses belajar.

## Peta Belajar Fundamental

| Urutan | Topik | File | Status |
|---|---|---|---|
| 1 | Setup Go dan workflow dasar | [01-setup-go.md](./01-setup-go.md) | ✅ Tersedia |
| 2 | Sintaks dan tipe data dasar | [02-syntax-dan-tipe-data.md](./02-syntax-dan-tipe-data.md) | ✅ Tersedia |
| 3 | Control flow (`if`, `switch`, `for`, `range`) | [03-control-flow.md](./03-control-flow.md) | ✅ Tersedia |
| 4 | Deep dive slices dan maps | [04-slices-maps-deep-dive.md](./04-slices-maps-deep-dive.md) | ✅ Tersedia |
| 5 | Pointer, struct, dan method dasar | [05-pointer-struct-method-dasar.md](./05-pointer-struct-method-dasar.md) | ✅ Tersedia |
| 6 | Error handling dasar (`error`, `panic`, `recover`) | [06-error-handling-dasar.md](./06-error-handling-dasar.md) | ✅ Tersedia |
| 7 | Interface dasar dan composition sederhana | [07-interface-dasar-composition.md](./07-interface-dasar-composition.md) | ✅ Tersedia |
| 8 | **Dasar low-level Go**: model memori sederhana, stack vs heap, escape analysis ringan, byte vs rune | [08-low-level-go-dasar.md](./08-low-level-go-dasar.md) | ✅ Tersedia |
| 9 | Indeks referensi resmi + lanjutan fundamental | [99-referensi-fundamental.md](./99-referensi-fundamental.md) | ✅ Tersedia |
| 10 | Latihan bertingkat + mini-project + checklist kelulusan (penutup fase fundamental) | [10-latihan-fundamental-dan-checklist.md](./10-latihan-fundamental-dan-checklist.md) | ✅ Tersedia |

> Level ini memang mencakup topik low-level dalam versi ramah pemula, jadi kamu tidak berhenti di sintaks saja.
>
> Tutup fase fundamental di langkah 10 sebelum lanjut ke intermediate.

## Mulai dari Sini

➡️ Lanjut ke materi pertama: [01-setup-go.md](./01-setup-go.md)

Butuh kumpulan link resmi per topik? Buka: [99-referensi-fundamental.md](./99-referensi-fundamental.md)

## Referensi resmi + lanjutan
- **[Dokumentasi resmi Go](https://go.dev/doc/)** — pintu utama semua panduan resmi Go.
  _Kapan baca:_ saat butuh rujukan cepat untuk topik apa pun.
- **[A Tour of Go](https://go.dev/tour/welcome/1)** — latihan interaktif dari dasar.
  _Kapan baca:_ setelah selesai satu artikel dan ingin langsung praktik kecil.
- **[Effective Go](https://go.dev/doc/effective_go)** — idiom dan kebiasaan menulis Go yang rapi.
  _Kapan baca:_ setelah paham dasar sintaks, sebelum lanjut ke level intermediate.
- **[Dokumentasi package standard library](https://pkg.go.dev/std)** — daftar package bawaan Go.
  _Kapan baca:_ saat bertanya “fitur ini sudah ada di package standar belum?”.
