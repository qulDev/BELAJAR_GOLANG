# Sintaks dan Tipe Data Dasar Go (Biar Tidak Asal Hafal)

Setelah setup selesai, langkah berikutnya adalah memahami cara Go "melihat" data. Banyak pemula bisa menulis kode, tapi belum paham kenapa beberapa operasi ditolak compiler. Di artikel ini, kita bangun fondasi sintaks dan tipe data secara bertahap supaya kamu bisa reasoning, bukan sekadar meniru.

---

## Untuk siapa materi ini
- Pemula yang baru selesai setup Go.
- Pembaca yang sudah pernah lihat `var` dan `:=`, tapi belum paham bedanya.
- Pembaca yang sering bingung soal konversi tipe dan zero value.

---

## Prasyarat
- Go sudah terpasang dan bisa menjalankan `go run .`.
- Paham struktur dasar file `main.go`.
- Sudah membaca [01-setup-go.md](./01-setup-go.md).

---

## Tujuan belajar
Setelah menyelesaikan artikel ini, kamu diharapkan bisa:
- Mendeklarasikan variabel dan konstanta dengan cara yang tepat.
- Memilih tipe data dasar yang sesuai kebutuhan.
- Menghindari error umum terkait mismatch tipe data.

---

## Mental model
Bayangkan variabel sebagai **kotak berlabel**:

```text
Nilai masuk -> kotak (variabel + tipe) -> operasi sesuai aturan tipe -> output
```

1. **Analogi non-teknis:** seperti gudang dengan kotak bertuliskan "angka", "teks", atau "boolean".
2. **Pemetaan analogi ke istilah teknis:** label kotak = tipe data (`int`, `string`, dst), isi kotak = value, aturan bongkar-muat = aturan compiler.
3. **Batas analogi:** di Go, label tipe sangat ketat; kamu tidak bisa mencampur isi kotak beda tipe tanpa konversi eksplisit.

---

## Konsep inti bertahap (step-by-step)
### Step 1 — Deklarasi variabel dan konstanta
- **Apa:** `var` untuk variabel, `const` untuk nilai konstan, `:=` untuk deklarasi singkat di dalam fungsi.
- **Kenapa penting:** membuat kode jelas, aman, dan mudah dirawat.
- **Kapan dipakai:** setiap menyimpan data sementara atau nilai tetap.
- **Catatan jebakan:** `:=` tidak bisa dipakai di luar fungsi.

### Step 2 — Tipe data dasar dan zero value
- **Apa:** tipe yang sering dipakai: `bool`, `string`, `int`, `float64`, `byte`, `rune`.
- **Kenapa penting:** tipe menentukan operasi yang valid dan bentuk memori data.
- **Kapan dipakai:** sejak baris pertama saat memilih bentuk data.
- **Catatan jebakan:** variabel yang tidak diinisialisasi punya zero value (`0`, `""`, `false`, dll).

### Step 3 — Konversi tipe dan representasi teks
- **Apa:** konversi eksplisit antar tipe numerik, paham beda `byte` dan `rune`.
- **Kenapa penting:** mencegah bug perhitungan dan bug karakter non-ASCII.
- **Kapan dipakai:** saat memproses input dari user/API/file.
- **Catatan jebakan:** `len(string)` menghitung byte, bukan jumlah karakter Unicode.

---

## Contoh kode bertahap
### Tahap 1 — Versi minimum (deklarasi dasar)
```go
package main

import "fmt"

func main() {
	var nama string = "Gopher"
	umur := 2
	const bahasa = "Go"

	var aktif bool   // zero value: false
	var skor int     // zero value: 0
	var catatan string // zero value: ""

	fmt.Println(nama, umur, bahasa)
	fmt.Println("aktif:", aktif, "skor:", skor, "catatan:", catatan)
}
```

Penjelasan singkat output/perilaku:
- `aktif`, `skor`, dan `catatan` otomatis punya nilai default.
- `:=` membantu deklarasi cepat saat tipe bisa diinfer.

### Tahap 2 — Tambah kebutuhan nyata (konversi input string ke angka)
```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	inputUmur := "21"
	umur, err := strconv.Atoi(inputUmur)
	if err != nil {
		fmt.Println("Input umur tidak valid")
		return
	}

	var tinggiCM float64 = 168.5
	tinggiM := tinggiCM / 100

	fmt.Printf("Umur: %d tahun, tinggi: %.2f m\n", umur, tinggiM)
}
```

Perubahan dibanding tahap 1:
- Ada parsing string ke `int`.
- Mulai kenal pola error handling saat konversi.

### Tahap 3 — Versi lebih idiomatik (tipe numerik + rune)
```go
package main

import "fmt"

func hitungTagihan(harga int, diskonPersen float64) float64 {
	potongan := float64(harga) * (diskonPersen / 100)
	return float64(harga) - potongan
}

func main() {
	nama := "Béla"
	fmt.Println("Jumlah byte nama:", len(nama))
	for i, r := range nama {
		fmt.Printf("index %d -> rune %q\n", i, r)
	}

	total := hitungTagihan(200000, 12.5)
	fmt.Printf("Total bayar: Rp%.0f\n", total)
}
```

Checklist pemahaman:
- [ ] Saya paham kapan memakai `var`, `const`, dan `:=`.
- [ ] Saya tahu kenapa konversi tipe di Go harus eksplisit.

---

## Common mistakes (minimal 2)
### Mistake 1 — Memakai `:=` di luar fungsi
- **Gejala:** error compile terkait `non-declaration statement outside function body`.
- **Penyebab:** `:=` hanya valid di dalam fungsi.
- **Perbaikan:** gunakan `var` untuk deklarasi di level package.

### Mistake 2 — Mencampur `int` dan `float64` tanpa konversi
- **Gejala:** error `mismatched types`.
- **Penyebab:** Go tidak melakukan konversi numerik otomatis.
- **Perbaikan:** konversi eksplisit (misalnya `float64(x)`).

### Mistake 3 — Mengira `len("é") == 1`
- **Gejala:** hasil panjang string terasa "salah" untuk karakter Unicode.
- **Penyebab:** `len` pada string menghitung byte, bukan rune.
- **Perbaikan:** gunakan `for range` atau konversi ke `[]rune` saat butuh hitung karakter.

---

## Latihan (basic/menengah/challenge)
### Basic
- Buat variabel `nama`, `umur`, `aktif` dengan tipe yang tepat.
- Cetak semuanya dalam satu kalimat.
- **Kriteria lulus:** program berjalan tanpa warning/error compile.

### Menengah
- Simpan umur dalam bentuk string (misal `"19"`), lalu konversi ke `int`.
- Jika konversi gagal, tampilkan pesan error dan hentikan program.
- **Constraint:** wajib pakai `strconv.Atoi`.

### Challenge
- Buat fungsi `hitungTotal(harga int, pajakPersen float64) float64`.
- Tambahkan output nama user yang mengandung karakter Unicode.
- **Hint:** cek perbedaan output `len(nama)` vs iterasi `range`.

---

## Ringkasan
- Go adalah bahasa statically typed: tipe data memegang peran utama.
- `var`, `const`, dan `:=` punya konteks penggunaan berbeda.
- Zero value membantu, tapi kamu tetap harus paham konversi tipe secara eksplisit.
- Next action: lanjut ke control flow untuk mengolah data berdasarkan kondisi dan perulangan.

---

## Referensi resmi + lanjutan
- **[A Tour of Go: Basic types](https://go.dev/tour/basics/11)** — latihan cepat memahami tipe dasar.
  _Kapan baca:_ saat butuh pemanasan sebelum menulis kode sendiri.
- **[Go Spec: Variable declarations](https://go.dev/ref/spec#Variable_declarations)** — aturan resmi deklarasi variabel.
  _Kapan baca:_ ketika bingung perilaku `var` vs `:=`.
- **[Go Spec: Constant declarations](https://go.dev/ref/spec#Constant_declarations)** — aturan resmi konstanta.
  _Kapan baca:_ saat ragu kapan pakai `const` dan batasannya.
- **[Package `strconv`](https://pkg.go.dev/strconv)** — konversi string ↔ angka secara aman.
  _Kapan baca:_ saat parsing input user atau format output.
- **[Package `unicode/utf8`](https://pkg.go.dev/unicode/utf8)** — referensi byte vs rune.
  _Kapan baca:_ saat mengolah teks Unicode dan hasil `len` terasa “aneh”.
- **[Indeks referensi fundamental](./99-referensi-fundamental.md)** — ringkasan lintas topik.
  _Kapan baca:_ saat perlu referensi topik lain dengan cepat.

---

## Navigasi fundamental
- [Kembali ke Start Here](./00-start-here.md)
- **Sebelumnya:** [01-setup-go.md](./01-setup-go.md)
- **Selanjutnya:** [03-control-flow.md](./03-control-flow.md)

