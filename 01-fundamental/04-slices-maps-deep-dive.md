# Deep Dive Slice dan Map: Biar Tidak Ketipu Perilaku Data

Slice dan map adalah dua struktur data yang paling sering kamu pakai di Go. Banyak bug pemula muncul di sini: data berubah "sendiri", urutan map acak, atau panic karena map belum diinisialisasi. Artikel ini membahas konsepnya pelan-pelan, tapi tetap cukup dalam agar kamu paham apa yang terjadi di balik layar.

---

## Untuk siapa materi ini
- Pemula yang sudah paham control flow dan tipe data dasar.
- Pembaca yang pernah bingung kenapa mengubah slice A bisa memengaruhi slice B.
- Pembaca yang ingin memakai map dengan aman dan benar.

---

## Prasyarat
- Sudah bisa menulis `for` dan `range`.
- Paham deklarasi variabel dan zero value.
- Sudah membaca [03-control-flow.md](./03-control-flow.md).

---

## Tujuan belajar
Setelah menyelesaikan artikel ini, kamu diharapkan bisa:
- Menjelaskan hubungan array backing, `len`, dan `cap` pada slice.
- Menghindari bug aliasing dengan teknik copy yang tepat.
- Memakai map untuk operasi insert/read/delete dengan pola aman.

---

## Mental model
Untuk slice, bayangkan "jendela" ke array backing:

```text
Backing array: [10 20 30 40 50]
Slice a := arr[1:4] -> [20 30 40] (len=3, cap=4)
```

Untuk map, bayangkan kamus:

```text
key -> value
"apel" -> 3
"jeruk" -> 5
```

1. **Analogi non-teknis:** slice = potongan pandangan ke rak yang sama; map = kamus kata-ke-arti.
2. **Pemetaan analogi ke istilah teknis:** slice menyimpan pointer ke backing array + `len` + `cap`; map menyimpan pasangan key-value berbasis hash.
3. **Batas analogi:** urutan map tidak stabil, dan `append` pada slice bisa pindah ke backing array baru.

---

## Konsep inti bertahap (step-by-step)
### Step 1 — Slice: `len`, `cap`, dan aliasing
- **Apa:** slice adalah descriptor (pointer, len, cap), bukan array itu sendiri.
- **Kenapa penting:** kamu jadi paham kenapa dua slice bisa saling memengaruhi.
- **Kapan dipakai:** saat slicing (`a[low:high]`) dan operasi `append`.
- **Catatan jebakan:** mengubah elemen lewat satu slice bisa mengubah data yang terlihat dari slice lain.

### Step 2 — Isolasi data dengan copy
- **Apa:** buat salinan slice jika butuh data independen.
- **Kenapa penting:** menghindari bug side effect yang sulit dilacak.
- **Kapan dipakai:** saat kirim data antar fungsi/layer tanpa ingin data asli berubah.
- **Catatan jebakan:** assignment biasa (`b := a`) hanya menyalin descriptor, bukan isi backing array.

### Step 3 — Map: operasi dasar + pola aman
- **Apa:** map menyimpan pasangan key-value, akses cepat via key.
- **Kenapa penting:** cocok untuk lookup, counter, dan indeks data.
- **Kapan dipakai:** counting, grouping, cache sederhana.
- **Catatan jebakan:** tulis ke `nil map` akan panic; iterasi map tidak terurut; akses concurrent butuh sinkronisasi.

---

## Contoh kode bertahap
### Tahap 1 — Versi minimum (slice berbagi backing array)
```go
package main

import "fmt"

func main() {
	angka := []int{10, 20, 30, 40}
	a := angka[:2]   // [10 20]
	b := angka[1:3]  // [20 30]

	b[0] = 999

	fmt.Println("angka:", angka)
	fmt.Println("a    :", a)
	fmt.Println("b    :", b)
	fmt.Println("len(a):", len(a), "cap(a):", cap(a))
}
```

Penjelasan singkat output/perilaku:
- Mengubah `b[0]` memengaruhi `angka` dan terlihat juga dari `a`.
- Ini terjadi karena `a` dan `b` masih berbagi backing array yang sama.

### Tahap 2 — Tambah kebutuhan nyata (copy untuk isolasi)
```go
package main

import "fmt"

func main() {
	asal := []int{1, 2, 3}
	salinan := append([]int(nil), asal...) // copy aman

	asal = append(asal, 4)
	asal[0] = 100

	fmt.Println("asal   :", asal)
	fmt.Println("salinan:", salinan)
}
```

Perubahan dibanding tahap 1:
- `salinan` tidak ikut berubah saat `asal` dimodifikasi.
- Teknik ini penting saat kamu butuh immutability sederhana.

### Tahap 3 — Versi lebih idiomatik (map untuk menghitung frekuensi)
```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	kata := []string{"go", "map", "go", "slice", "go", "map"}
	hitung := make(map[string]int)

	for _, k := range kata {
		hitung[k]++
	}

	keys := make([]string, 0, len(hitung))
	for k := range hitung {
		keys = append(keys, k)
	}
	sort.Strings(keys)

	for _, k := range keys {
		fmt.Printf("%s -> %d\n", k, hitung[k])
	}
}
```

Checklist pemahaman:
- [ ] Saya paham kapan slice berbagi backing array.
- [ ] Saya tahu cara menyalin slice agar perubahan tidak bocor.
- [ ] Saya paham map harus diinisialisasi sebelum ditulis.

---

## Common mistakes (minimal 2)
### Mistake 1 — Menulis ke `nil map`
- **Gejala:** panic `assignment to entry in nil map`.
- **Penyebab:** map dideklarasikan tapi belum diinisialisasi.
- **Perbaikan:** buat map dengan `make(map[K]V)` sebelum operasi write.

### Mistake 2 — Mengira urutan iterasi map selalu sama
- **Gejala:** output urutan key berubah-ubah antar run.
- **Penyebab:** spesifikasi Go memang tidak menjamin urutan map.
- **Perbaikan:** kumpulkan key ke slice lalu urutkan jika butuh output stabil.

### Mistake 3 — Tidak sadar dua slice saling berbagi data
- **Gejala:** update di satu tempat mengubah data di tempat lain.
- **Penyebab:** slicing/assignment menyalin descriptor, bukan backing array.
- **Perbaikan:** lakukan copy (`copy` atau `append([]T(nil), src...)`) saat perlu isolasi.

---

## Latihan (basic/menengah/challenge)
### Basic
- Buat slice `[]int{1,2,3,4,5}`, ambil sub-slice, lalu ubah satu elemen.
- Amati apakah slice asli ikut berubah.
- **Kriteria lulus:** kamu bisa menjelaskan kenapa perubahan terjadi.

### Menengah
- Buat fungsi yang menerima slice angka dan mengembalikan salinan independen.
- Buktikan bahwa perubahan pada slice asli tidak memengaruhi hasil salinan.
- **Constraint:** tidak boleh mengembalikan referensi ke backing array lama.

### Challenge
- Dari slice kata, buat map frekuensi sekaligus hasilkan output terurut berdasarkan key.
- **Hint:** hitung dengan map, lalu urutkan key memakai `sort.Strings`.

---

## Ringkasan
- Slice adalah view ke backing array; `len` dan `cap` penting untuk memahami perilakunya.
- Copy slice diperlukan saat kamu ingin data independen.
- Map sangat efektif untuk lookup/counter, tapi tidak punya urutan iterasi tetap.
- Next action: lanjut ke pointer, struct, dan method dasar agar data & perilaku program makin terstruktur.

---

## Referensi resmi + lanjutan
- **[Go Blog: Go Slices: usage and internals](https://go.dev/blog/slices-intro)** — fondasi mental model slice + backing array.
  _Kapan baca:_ saat perilaku `append`/`cap` masih terasa membingungkan.
- **[Go Blog: Go maps in action](https://go.dev/blog/maps)** — praktik map untuk lookup, set, dan counter.
  _Kapan baca:_ saat mulai sering memakai map di kode harian.
- **[Go Spec: Slice types](https://go.dev/ref/spec#Slice_types)** — definisi formal slice.
  _Kapan baca:_ ketika perlu jawaban presisi soal slice semantics.
- **[Go Spec: Map types](https://go.dev/ref/spec#Map_types)** — definisi formal map.
  _Kapan baca:_ saat perlu memastikan perilaku key/value map.
- **[Package `slices`](https://pkg.go.dev/slices)** — helper resmi untuk operasi slice modern.
  _Kapan baca:_ saat butuh sort, compare, atau utility slice tanpa reinvent.
- **[Package `maps`](https://pkg.go.dev/maps)** — helper resmi operasi map.
  _Kapan baca:_ saat perlu clone/equal map dengan cara idiomatik.
- **[Indeks referensi fundamental](./99-referensi-fundamental.md)** — ringkasan referensi lintas topik.
  _Kapan baca:_ ketika ingin melihat hubungan topik fundamental lainnya.

---

## Navigasi fundamental
- [Kembali ke Start Here](./00-start-here.md)
- **Sebelumnya:** [03-control-flow.md](./03-control-flow.md)
- **Selanjutnya:** [05-pointer-struct-method-dasar.md](./05-pointer-struct-method-dasar.md)

