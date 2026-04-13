# Control Flow Go: if, switch, for, range (Praktis untuk Pemula)

Setelah paham variabel dan tipe data, sekarang saatnya mengatur alur program: kapan kode jalan, kapan berhenti, dan kapan mengulang. Topik ini adalah "otak keputusan" dari programmu. Kalau bagian ini kuat, kamu akan lebih mudah membangun program yang benar, bukan sekadar bisa compile.

---

## Untuk siapa materi ini
- Pemula yang sudah paham sintaks dasar Go.
- Pembaca yang masih bingung kapan pakai `if` vs `switch`.
- Pembaca yang ingin menulis loop Go tanpa jebakan umum.

---

## Prasyarat
- Sudah bisa menjalankan program Go sederhana.
- Paham variabel, konstanta, dan tipe data dasar.
- Sudah membaca [02-syntax-dan-tipe-data.md](./02-syntax-dan-tipe-data.md).

---

## Tujuan belajar
Setelah menyelesaikan artikel ini, kamu diharapkan bisa:
- Menulis percabangan `if/else` dan `switch` dengan tepat.
- Menggunakan `for` sebagai loop utama Go (termasuk gaya `while`).
- Memakai `range` pada slice, map, dan string tanpa asumsi yang salah.

---

## Mental model
Bayangkan flow program seperti lampu lalu lintas:

```text
Input -> Keputusan (if/switch) -> Aksi berulang (for/range) -> Output
```

1. **Analogi non-teknis:** kamu memilih jalur perjalanan berdasarkan kondisi jalan.
2. **Pemetaan analogi ke istilah teknis:** keputusan = `if/switch`, pengulangan = `for/range`, berhenti/lanjut = `break/continue`.
3. **Batas analogi:** program tidak "menebak"; kondisi harus ditulis eksplisit agar hasil konsisten.

---

## Konsep inti bertahap (step-by-step)
### Step 1 — Percabangan dengan `if/else`
- **Apa:** jalankan blok tertentu jika kondisi bernilai `true`.
- **Kenapa penting:** hampir semua validasi dan keputusan pakai `if`.
- **Kapan dipakai:** cek nilai, validasi input, cek error.
- **Catatan jebakan:** kondisi `if` harus boolean; Go tidak menerima angka sebagai boolean.

### Step 2 — Multi-branch dengan `switch`
- **Apa:** memilih satu dari banyak kemungkinan tanpa rantai `if` panjang.
- **Kenapa penting:** kode lebih rapi dan mudah dibaca.
- **Kapan dipakai:** klasifikasi status, command, kategori.
- **Catatan jebakan:** `switch` Go tidak fallthrough otomatis.

### Step 3 — Perulangan dengan `for` dan `range`
- **Apa:** `for` adalah satu-satunya keyword loop di Go; `range` membantu iterasi collection/string.
- **Kenapa penting:** data processing hampir selalu melibatkan loop.
- **Kapan dipakai:** iterasi array/slice/map/string, retry, scan data.
- **Catatan jebakan:** urutan iterasi map tidak dijamin; `range` string berjalan per rune, bukan byte.

---

## Contoh kode bertahap
### Tahap 1 — Versi minimum (`if` + `for`)
```go
package main

import "fmt"

func main() {
	nilai := 78
	if nilai >= 75 {
		fmt.Println("Lulus")
	} else {
		fmt.Println("Remedial")
	}

	total := 0
	for i := 1; i <= 5; i++ {
		total += i
	}
	fmt.Println("Total 1..5 =", total)
}
```

Penjelasan singkat output/perilaku:
- `if` menentukan status kelulusan.
- `for` menjumlahkan angka 1 sampai 5.

### Tahap 2 — Tambah kebutuhan nyata (`switch` untuk klasifikasi)
```go
package main

import (
	"fmt"
	"strings"
)

func kategoriHari(hari string) string {
	switch strings.ToLower(hari) {
	case "sabtu", "minggu":
		return "Akhir pekan"
	default:
		return "Hari kerja"
	}
}

func main() {
	for _, hari := range []string{"Senin", "Minggu"} {
		fmt.Printf("%s -> %s\n", hari, kategoriHari(hari))
	}
}
```

Perubahan dibanding tahap 1:
- Menambahkan `switch` agar percabangan multi-kasus lebih rapi.
- `strings.ToLower` dipakai supaya input lebih toleran.

### Tahap 3 — Versi lebih idiomatik (`continue`, `break`, `range` string)
```go
package main

import "fmt"

func main() {
	angka := []int{2, -1, 4, 0, 7}
	totalGenap := 0

	for _, n := range angka {
		if n < 0 {
			continue // skip angka negatif
		}
		if n == 0 {
			break // 0 dianggap tanda berhenti
		}
		if n%2 == 0 {
			totalGenap += n
		}
	}
	fmt.Println("Total genap sebelum 0:", totalGenap)

	kata := "Go🚀"
	for i, r := range kata {
		fmt.Printf("index=%d rune=%q\n", i, r)
	}
}
```

Checklist pemahaman:
- [ ] Saya paham kapan memakai `if` dan kapan `switch`.
- [ ] Saya bisa menjelaskan fungsi `break` dan `continue` di loop.

---

## Common mistakes (minimal 2)
### Mistake 1 — Mengira `switch` Go otomatis lanjut ke `case` berikutnya
- **Gejala:** hasil tidak sesuai ekspektasi "jatuh" ke banyak case.
- **Penyebab:** asumsi dari bahasa lain yang default-nya fallthrough.
- **Perbaikan:** ingat bahwa Go berhenti di case yang cocok, kecuali kamu tulis `fallthrough` eksplisit.

### Mistake 2 — Mengubah nilai variabel `range` dan berharap data asli ikut berubah
- **Gejala:** elemen slice/map asli tidak berubah.
- **Penyebab:** variabel `v` pada `for _, v := range ...` adalah salinan nilai.
- **Perbaikan:** ubah lewat index (`slice[i] = ...`) jika ingin memodifikasi data asli.

### Mistake 3 — Loop tidak pernah selesai
- **Gejala:** program hang karena loop tak berujung.
- **Penyebab:** kondisi loop tidak pernah berubah jadi `false`.
- **Perbaikan:** pastikan ada update state di setiap iterasi atau pakai syarat berhenti yang jelas.

---

## Latihan (basic/menengah/challenge)
### Basic
- Buat program cek angka: jika `>= 0` tampilkan `positif`, selain itu `negatif`.
- Ulangi untuk 5 angka menggunakan loop.
- **Kriteria lulus:** output sesuai logika untuk semua angka.

### Menengah
- Buat fungsi `kategoriNilai(n int) string` dengan `switch`.
- Kategori: `A` (>=90), `B` (>=80), `C` (>=70), selain itu `D`.
- **Constraint:** jangan pakai chain `if` panjang.

### Challenge
- Diberi slice angka campur (negatif, nol, positif), jumlahkan hanya angka ganjil positif sampai sebelum ketemu nol.
- **Hint:** gabungkan `if`, `%`, `continue`, dan `break`.

---

## Ringkasan
- `if/else` dipakai untuk keputusan sederhana dan validasi.
- `switch` cocok untuk percabangan multi-kasus yang lebih rapi.
- `for` adalah loop utama Go, `range` mempermudah iterasi data.
- Next action: lanjut ke slice dan map untuk memahami struktur data yang paling sering dipakai di Go.

---

## Referensi resmi + lanjutan
- **[A Tour of Go: Flow control](https://go.dev/tour/flowcontrol/1)** — latihan interaktif `if`, `switch`, dan `for`.
  _Kapan baca:_ saat ingin cek pemahaman dengan contoh kecil.
- **[Go Spec: `if` statements](https://go.dev/ref/spec#If_statements)** — aturan resmi percabangan `if`.
  _Kapan baca:_ ketika ragu soal scope variabel di `if short statement`.
- **[Go Spec: `switch` statements](https://go.dev/ref/spec#Switch_statements)** — detail `switch expression` dan `type switch`.
  _Kapan baca:_ saat butuh percabangan multi-kondisi yang rapi.
- **[Go Spec: `for` statements](https://go.dev/ref/spec#For_statements)** — aturan loop utama di Go.
  _Kapan baca:_ saat bingung variasi `for`/`range` dan perilaku iterasi.
- **[Go Blog: Defer, Panic, and Recover](https://go.dev/blog/defer-panic-and-recover)** — kontrol alur saat cleanup/error fatal.
  _Kapan baca:_ setelah paham control flow dasar, sebelum masuk error handling.
- **[Indeks referensi fundamental](./99-referensi-fundamental.md)** — kumpulan link topik fundamental.
  _Kapan baca:_ saat ingin pindah topik tanpa kehilangan referensi resmi.

---

## Navigasi fundamental
- [Kembali ke Start Here](./00-start-here.md)
- **Sebelumnya:** [02-syntax-dan-tipe-data.md](./02-syntax-dan-tipe-data.md)
- **Selanjutnya:** [04-slices-maps-deep-dive.md](./04-slices-maps-deep-dive.md)

