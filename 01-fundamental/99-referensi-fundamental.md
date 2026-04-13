# 99 - Referensi Fundamental Go (Resmi + Lanjutan)

Kumpulan referensi ini dibuat sebagai **indeks cepat** untuk semua materi fundamental.
Fokusnya: link resmi Go + sedikit lanjutan yang paling berguna saat praktik.

## Referensi resmi + lanjutan
Semua link di bawah ini sudah dikurasi, dikelompokkan per topik, dan diprioritaskan dari sumber resmi Go (`go.dev`/`pkg.go.dev`).

## Cara pakai singkat
1. Pilih topik yang sedang kamu pelajari.
2. Baca 1-2 link dulu (jangan semuanya sekaligus).
3. Balik lagi ke artikel fundamental, lalu praktikkan.

---

## Setup & workflow dasar ([01-setup-go.md](./01-setup-go.md))
- **[Install Go](https://go.dev/doc/install)** — panduan instalasi resmi per OS.
  _Kapan baca:_ setup pertama kali atau upgrade versi.
- **[Tutorial: Getting Started](https://go.dev/doc/tutorial/getting-started)** — alur project pertama.
  _Kapan baca:_ saat baru mulai pakai CLI Go.
- **[Go Modules Reference](https://go.dev/ref/mod)** — aturan `go.mod`, dependency, dan versioning.
  _Kapan baca:_ saat `go mod tidy`/dependency terasa membingungkan.
- **[Command `go`](https://pkg.go.dev/cmd/go)** — dokumentasi perintah `go`.
  _Kapan baca:_ ketika lupa flag/perintah yang tepat.

## Sintaks & tipe data ([02-syntax-dan-tipe-data.md](./02-syntax-dan-tipe-data.md))
- **[A Tour of Go: Basic Types](https://go.dev/tour/basics/11)** — penguatan konsep tipe dasar.
  _Kapan baca:_ sesudah membaca materi sintaks untuk latihan cepat.
- **[Go Spec: Variable declarations](https://go.dev/ref/spec#Variable_declarations)** — aturan formal variabel.
  _Kapan baca:_ saat bingung `var` vs `:=`.
- **[Go Spec: Constant declarations](https://go.dev/ref/spec#Constant_declarations)** — aturan formal konstanta.
  _Kapan baca:_ saat perlu kepastian batas penggunaan `const`.
- **[Package `strconv`](https://pkg.go.dev/strconv)** — konversi string/angka.
  _Kapan baca:_ saat parsing input user.

## Control flow ([03-control-flow.md](./03-control-flow.md))
- **[A Tour of Go: Flow Control](https://go.dev/tour/flowcontrol/1)** — latihan `if`, `switch`, `for`.
  _Kapan baca:_ saat ingin evaluasi pemahaman dasar control flow.
- **[Go Spec: `if` statements](https://go.dev/ref/spec#If_statements)** — detail `if`.
  _Kapan baca:_ saat ragu soal short statement/scope.
- **[Go Spec: `switch` statements](https://go.dev/ref/spec#Switch_statements)** — detail `switch`.
  _Kapan baca:_ saat butuh percabangan multi-kasus yang tepat.
- **[Go Spec: `for` statements](https://go.dev/ref/spec#For_statements)** — detail loop di Go.
  _Kapan baca:_ saat menemukan perilaku iterasi yang tidak terduga.

## Slice & map ([04-slices-maps-deep-dive.md](./04-slices-maps-deep-dive.md))
- **[Go Blog: Go Slices: usage and internals](https://go.dev/blog/slices-intro)** — mental model slice.
  _Kapan baca:_ saat perilaku `append`/`cap` membingungkan.
- **[Go Blog: Go maps in action](https://go.dev/blog/maps)** — pattern penggunaan map.
  _Kapan baca:_ saat membangun lookup/counter/set.
- **[Package `slices`](https://pkg.go.dev/slices)** — utility slice modern.
  _Kapan baca:_ saat butuh sort/compare/clone slice.
- **[Package `maps`](https://pkg.go.dev/maps)** — utility map modern.
  _Kapan baca:_ saat butuh clone/equal map idiomatik.

## Pointer, struct, method ([05-pointer-struct-method-dasar.md](./05-pointer-struct-method-dasar.md))
- **[A Tour of Go: Pointers](https://go.dev/tour/moretypes/1)** — dasar pointer.
  _Kapan baca:_ saat masih tertukar value vs reference.
- **[A Tour of Go: Structs](https://go.dev/tour/moretypes/2)** — desain data dengan struct.
  _Kapan baca:_ saat mulai memodelkan entity.
- **[A Tour of Go: Methods](https://go.dev/tour/methods/1)** — method pada tipe kustom.
  _Kapan baca:_ saat belajar receiver value/pointer.
- **[Go Spec: Method declarations](https://go.dev/ref/spec#Method_declarations)** — aturan formal method.
  _Kapan baca:_ saat butuh jawaban presisi soal method set.

## Error handling ([06-error-handling-dasar.md](./06-error-handling-dasar.md))
- **[Go Blog: Error handling and Go](https://go.dev/blog/error-handling-and-go)** — filosofi error di Go.
  _Kapan baca:_ saat mulai menulis alur error yang konsisten.
- **[Go Blog: Working with Errors in Go 1.13](https://go.dev/blog/go1.13-errors)** — wrapping & inspection error.
  _Kapan baca:_ saat implementasi `%w`, `errors.Is`, `errors.As`.
- **[Package `errors`](https://pkg.go.dev/errors)** — API error standar.
  _Kapan baca:_ saat implementasi error handling harian.
- **[Go Spec: Handling panics](https://go.dev/ref/spec#Handling_panics)** — aturan panic/recover.
  _Kapan baca:_ saat memastikan penggunaan panic tetap tepat.

## Interface & composition ([07-interface-dasar-composition.md](./07-interface-dasar-composition.md))
- **[A Tour of Go: Interfaces](https://go.dev/tour/methods/9)** — interface secara praktis.
  _Kapan baca:_ saat mulai membuat abstraction sederhana.
- **[Effective Go: Interfaces and other types](https://go.dev/doc/effective_go#interfaces_and_types)** — guideline desain interface.
  _Kapan baca:_ saat ingin menjaga interface tetap kecil dan jelas.
- **[Package `io`](https://pkg.go.dev/io)** — contoh interface kecil paling penting di stdlib.
  _Kapan baca:_ saat belajar composition di dunia nyata.
- **[Go Blog: The Laws of Reflection](https://go.dev/blog/laws-of-reflection)** — hubungan interface dan reflection.
  _Kapan baca:_ setelah paham interface dasar.

## Fondasi low-level ([08-low-level-go-dasar.md](./08-low-level-go-dasar.md))
- **[The Go Memory Model](https://go.dev/ref/mem)** — aturan formal sinkronisasi memori.
  _Kapan baca:_ saat masuk concurrent programming lebih serius.
- **[Data Race Detector](https://go.dev/doc/articles/race_detector)** — alat deteksi race.
  _Kapan baca:_ tiap kali ada shared state antar goroutine.
- **[Go FAQ](https://go.dev/doc/faq)** — jawaban resmi soal stack/heap/alokasi.
  _Kapan baca:_ saat bertanya “kenapa perilakunya begini?”.
- **[Go Blog: Strings, bytes, runes and characters in Go](https://go.dev/blog/strings)** — fondasi pemrosesan teks.
  _Kapan baca:_ saat mengolah Unicode dan butuh model yang tepat.
- **[Package `unicode/utf8`](https://pkg.go.dev/unicode/utf8)** — API utilitas UTF-8.
  _Kapan baca:_ saat validasi atau iterasi rune dengan aman.

---

## Referensi lintas topik
- **[Go Specification](https://go.dev/ref/spec)** — sumber aturan bahasa paling otoritatif.
- **[Effective Go](https://go.dev/doc/effective_go)** — idiom penulisan Go yang maintainable.
- **[Standard library index](https://pkg.go.dev/std)** — katalog package bawaan Go.

## Navigasi fundamental
- [Kembali ke Start Here](./00-start-here.md)
- **Sebelumnya:** [08-low-level-go-dasar.md](./08-low-level-go-dasar.md)
- **Selanjutnya (penutup fase fundamental):** [10-latihan-fundamental-dan-checklist.md](./10-latihan-fundamental-dan-checklist.md)
