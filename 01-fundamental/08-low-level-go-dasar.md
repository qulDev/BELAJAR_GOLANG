# Low-Level Go Dasar untuk Pemula

Topik low-level sering terdengar menakutkan, padahal fondasinya bisa dipelajari pelan-pelan.  
Di materi ini kita tidak membahas optimasi ekstrem dulu, tapi membangun intuisi yang benar tentang memori, pointer, string, dan keamanan data agar kamu siap naik ke level advanced.

## Tujuan belajar
Setelah menyelesaikan artikel ini, kamu diharapkan bisa:
- Memahami **model memori sederhana di Go**.
- Menjelaskan **stack vs heap** secara praktis.
- Memahami intuisi **escape analysis** dan membaca contoh output compiler.
- Membedakan **value vs pointer semantics** dalam konteks performa dan mutasi data.
- Memahami perbedaan **byte vs rune** beserta implikasi performa/kebenaran.
- Menghubungkan konsep ini ke **race detector** dan **Go memory model** sebagai jembatan ke materi advanced.

## Mental model
Bayangkan memori program seperti dua area kerja:
- **Stack**: meja kerja cepat untuk data lokal yang umurnya pendek.
- **Heap**: gudang untuk data yang perlu hidup lebih lama/diakses lintas scope.

Go compiler mencoba menaruh data di stack kalau aman. Kalau tidak aman (mis. datanya “kabur” keluar fungsi), data dipindah ke heap. Heap dipantau GC (garbage collector).

```text
Goroutine
  |
  +-- Stack frame main()
  |      - variabel lokal sederhana
  |
  +-- Stack frame fungsiA()
         - variabel lokal sementara

Heap (shared area)
  - objek yang lifetime-nya lebih panjang
  - objek yang di-return sebagai pointer
  - objek yang dipegang closure/goroutine lain
```

1. **Analogi non-teknis:** meja kerja (stack) vs gudang penyimpanan (heap).
2. **Pemetaan teknis:** stack cepat dan dekat alur fungsi; heap lebih fleksibel tapi dikelola GC.
3. **Batas analogi:** keputusan final stack/heap ditentukan compiler (escape analysis), bukan “dipilih manual” tiap kali.

## Konsep inti (step-by-step)
### Step 1 — Model memori sederhana di Go
- **Apa:** variabel menempati lokasi memori selama lifetime tertentu.
- **Kenapa penting:** membantu reasoning saat data berubah/tidak berubah.
- **Kapan dipakai:** debugging bug nilai tidak sesuai, tuning performa dasar.
- **Catatan jebakan:** jangan mengira semua pointer pasti di heap.

### Step 2 — Stack vs heap (konsep praktis)
- **Apa:** stack untuk frame fungsi aktif; heap untuk objek yang butuh lifetime lebih panjang.
- **Kenapa penting:** alokasi heap berpotensi menambah tekanan GC.
- **Kapan dipakai:** saat mendesain struktur data/algoritma yang sering dipanggil.
- **Catatan jebakan:** “heap selalu buruk” adalah mitos; fokus ke measurement.

### Step 3 — Escape analysis (intuisi + contoh)
- **Apa:** analisis compiler apakah nilai “escape” dari scope lokal.
- **Kenapa penting:** menjelaskan kenapa objek tertentu pindah ke heap.
- **Kapan dipakai:** saat profiling menunjukkan alokasi tinggi.
- **Catatan jebakan:** output escape analysis bisa berubah antar versi compiler.

### Step 4 — Value vs pointer semantics
- **Apa:** value mengirim salinan; pointer mengirim alamat nilai asli.
- **Kenapa penting:** berdampak ke mutasi state dan biaya copy.
- **Kapan dipakai:** API function/method, struct besar, state bersama.
- **Catatan jebakan:** pointer tidak otomatis lebih cepat di semua kasus.

### Step 5 — Byte vs rune (ringkas) + implikasi performa/kebenaran
- **Apa:** string Go disimpan sebagai byte UTF-8; `rune` merepresentasikan code point Unicode.
- **Kenapa penting:** `len(s)` menghitung byte, bukan jumlah karakter manusia.
- **Kapan dipakai:** validasi teks, pemotongan string, pemrosesan multilingual.
- **Catatan jebakan:** iterasi byte bisa merusak karakter multibyte.

### Step 6 — Jembatan ke race detector dan memory model advanced
- **Apa:** data race terjadi saat akses bersamaan ke lokasi memori sama tanpa sinkronisasi benar.
- **Kenapa penting:** bug race sering tidak deterministik dan sulit direproduksi.
- **Kapan dipakai:** semua kode concurrency.
- **Catatan jebakan:** program “terlihat normal” belum tentu race-free.

## Contoh kode bertahap
### Tahap 1 — Value vs pointer semantics
```go
package main

import "fmt"

type Counter struct {
	N int
}

func incByValue(c Counter) {
	c.N++
}

func incByPointer(c *Counter) {
	c.N++
}

func main() {
	c := Counter{N: 10}

	incByValue(c)
	fmt.Println("setelah value:", c.N) // 10

	incByPointer(&c)
	fmt.Println("setelah pointer:", c.N) // 11
}
```

Penjelasan:
- Value semantics menjaga isolasi data.
- Pointer semantics memungkinkan mutasi langsung ke data asli.

### Tahap 2 — Melihat escape analysis
```go
package main

import "fmt"

type User struct {
	Name string
}

func userValue(name string) User {
	u := User{Name: name}
	return u
}

func userPointer(name string) *User {
	u := User{Name: name}
	return &u
}

func main() {
	v := userValue("Rina")
	p := userPointer("Budi")
	fmt.Println(v.Name, p.Name)
}
```

Jalankan:
```powershell
go build -gcflags="-m" .
```

Biasanya kamu akan melihat petunjuk seperti:
- `moved to heap: u` pada fungsi yang return pointer.

Perubahan dibanding tahap 1:
- Kita tidak hanya melihat perilaku runtime, tapi juga keputusan alokasi dari compiler.

### Tahap 3 — Byte vs rune + jembatan race detector
```go
package main

import (
	"fmt"
	"sync"
	"unicode/utf8"
)

func main() {
	s := "Go語"
	fmt.Println("bytes:", len(s))                   // 5
	fmt.Println("runes:", utf8.RuneCountInString(s)) // 3

	counter := 0
	var mu sync.Mutex
	var wg sync.WaitGroup

	for i := 0; i < 1000; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			mu.Lock()
			counter++
			mu.Unlock()
		}()
	}

	wg.Wait()
	fmt.Println("counter:", counter) // 1000
}
```

Eksperimen:
1. Hapus `mu.Lock()/mu.Unlock()`.
2. Jalankan `go run -race .`.
3. Amati laporan data race dari race detector.

Checklist pemahaman:
- [ ] Saya paham kapan harus menghitung rune, bukan byte.
- [ ] Saya paham race detector membantu menemukan akses memori bersamaan yang berbahaya.
- [ ] Saya paham pointer/value choice harus dilihat bersama kebutuhan mutasi + biaya alokasi.

## Common mistakes
### Mistake 1 — Menganggap pointer selalu lebih cepat
- **Gejala:** refactor besar ke pointer tapi performa tidak membaik.
- **Penyebab:** mengabaikan overhead alokasi heap dan GC.
- **Perbaikan:** ukur dengan benchmark/profiling, jangan asumsi.

### Mistake 2 — Menganggap `len(string)` = jumlah karakter
- **Gejala:** validasi panjang nama berbahasa non-ASCII jadi salah.
- **Penyebab:** `len` menghitung byte UTF-8.
- **Perbaikan:** pakai `utf8.RuneCountInString` saat butuh jumlah rune.

### Mistake 3 — Merasa aman karena “tidak pernah crash”
- **Gejala:** bug acak di concurrency sulit direproduksi.
- **Penyebab:** data race tidak selalu langsung memunculkan panic.
- **Perbaikan:** jalankan `go test -race ./...` atau `go run -race .` secara rutin.

## Latihan
### Basic
Ubah kode tahap 1:
- Tambah field `History []int` pada `Counter`.
- Bandingkan hasil saat append dilakukan via value vs pointer.

### Menengah
Tulis dua fungsi:
- `countBytes(s string) int`
- `countRunes(s string) int`

Uji dengan string: `"Halo"`, `"Gö"`, `"日本語"`.  
Catat kapan hasil byte dan rune berbeda.

### Challenge
Buat program mini concurrent counter:
- Versi A: tanpa sinkronisasi.
- Versi B: pakai `sync.Mutex`.

Langkah:
1. Jalankan kedua versi dengan `-race`.
2. Bandingkan hasil dan laporan.
3. Jelaskan kaitannya dengan memory model “happens-before” (secara intuisi dulu).

## Ringkasan
- Go punya model memori yang bisa dipahami dari konsep lifetime data.
- Stack vs heap penting, tapi keputusan nyata ada di compiler lewat escape analysis.
- Value vs pointer bukan soal “mana paling cepat”, tapi soal semantik mutasi + biaya.
- Byte vs rune krusial untuk kebenaran saat memproses teks Unicode.
- Race detector adalah jembatan praktis sebelum masuk memory model advanced secara formal.

## Referensi resmi + lanjutan
- **[The Go Memory Model](https://go.dev/ref/mem)** — aturan formal visibilitas data antar goroutine.
  _Kapan baca:_ setelah paham intuisi race detector dan ingin fondasi formalnya.
- **[Data Race Detector](https://go.dev/doc/articles/race_detector)** — cara praktis mendeteksi race di kode.
  _Kapan baca:_ setiap kali menulis concurrent code yang menyentuh shared state.
- **[Go FAQ](https://go.dev/doc/faq)** — jawaban resmi soal alokasi, stack/heap, dan performa umum.
  _Kapan baca:_ saat muncul pertanyaan “kenapa Go melakukan ini?”.
- **[Go Blog: Strings, bytes, runes and characters in Go](https://go.dev/blog/strings)** — fondasi teks Unicode di Go.
  _Kapan baca:_ ketika mengolah string non-ASCII dan butuh model mental yang benar.
- **[Package `unicode/utf8`](https://pkg.go.dev/unicode/utf8)** — API resmi validasi dan iterasi rune.
  _Kapan baca:_ saat butuh implementasi robust untuk pemrosesan teks.
- **[Indeks referensi fundamental](./99-referensi-fundamental.md)** — ringkasan referensi semua topik fundamental.
  _Kapan baca:_ saat menyiapkan transisi ke materi intermediate.

## Navigasi fundamental
- [Kembali ke Start Here](./00-start-here.md)
- **Sebelumnya:** [07-interface-dasar-composition.md](./07-interface-dasar-composition.md)
- **Selanjutnya:** [99-referensi-fundamental.md](./99-referensi-fundamental.md)
- **Penutup fase fundamental:** [10-latihan-fundamental-dan-checklist.md](./10-latihan-fundamental-dan-checklist.md)
