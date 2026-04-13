# Error Handling Dasar: `error`, `panic`, `recover`

Salah satu kekuatan Go adalah cara menangani error yang eksplisit.  
Kita tidak “menyembunyikan” error: kita lihat, cek, lalu putuskan aksi. Di artikel ini kamu akan belajar alur dasar error handling yang praktis dan aman untuk pemula.

## Tujuan belajar
Setelah membaca materi ini, kamu diharapkan bisa:
- Menangani error dengan pola `if err != nil`.
- Menambah konteks error menggunakan wrapping (`%w`).
- Memahami kapan `panic` layak dipakai dan kapan tidak.
- Mengenal `recover` sebagai pagar terakhir agar program tidak langsung mati.

## Mental model
Pikirkan tiga level sinyal masalah:
- `error` = lampu kuning: ada masalah, tapi masih bisa ditangani di alur normal.
- `panic` = rem darurat: kondisi fatal/tidak masuk akal terjadi.
- `recover` = airbag: mencegah crash total di boundary tertentu (misalnya worker/handler).

```text
fungsi A -> fungsi B -> fungsi C
                 |
             error return
                 v
            caller memutuskan:
            retry / fallback / stop

panic terjadi -> stack unwind -> defer jalan -> recover (opsional)
```

1. **Analogi non-teknis:** lampu indikator mobil (error) vs rem tangan darurat (panic).
2. **Pemetaan teknis:** error adalah nilai biasa, panic adalah mekanisme runtime.
3. **Batas analogi:** tidak semua error harus dipanic-kan; mayoritas cukup return error.

## Konsep inti (step-by-step)
### Step 1 — Error adalah nilai balik
- **Apa:** fungsi mengembalikan `(hasil, error)`.
- **Kenapa penting:** alur kegagalan terlihat jelas.
- **Kapan dipakai:** hampir semua operasi yang bisa gagal (I/O, parsing, network, validasi).
- **Catatan jebakan:** mengabaikan error (`_`) sering jadi bug tersembunyi.

### Step 2 — Tambahkan konteks dengan wrapping
- **Apa:** `fmt.Errorf("konteks: %w", err)`.
- **Kenapa penting:** stack call manual jadi lebih terbaca.
- **Kapan dipakai:** saat meneruskan error ke layer atas.
- **Catatan jebakan:** pakai `%v` akan kehilangan rantai error untuk `errors.Is/As`.

### Step 3 — `panic` dan `recover` untuk kondisi khusus
- **Apa:** `panic` menghentikan flow normal; `recover` menangkap panic di `defer`.
- **Kenapa penting:** berguna untuk kondisi fatal atau boundary perlindungan.
- **Kapan dipakai:** bug internal serius, invariant rusak, inisialisasi kritis gagal.
- **Catatan jebakan:** jangan pakai panic untuk validasi input biasa.

## Contoh kode bertahap
### Tahap 1 — Menangani error dasar
```go
package main

import (
	"errors"
	"fmt"
)

func bagi(a, b float64) (float64, error) {
	if b == 0 {
		return 0, errors.New("pembagi tidak boleh 0")
	}
	return a / b, nil
}

func main() {
	hasil, err := bagi(10, 0)
	if err != nil {
		fmt.Println("error:", err)
		return
	}
	fmt.Println("hasil:", hasil)
}
```

Penjelasan:
- Fungsi tidak panic untuk kasus normal yang bisa diprediksi.
- Caller memutuskan apa yang harus dilakukan saat ada error.

### Tahap 2 — Wrapping + pengecekan jenis error
```go
package main

import (
	"errors"
	"fmt"
)

var ErrUserNotFound = errors.New("user tidak ditemukan")

func findUser(id int) (string, error) {
	if id != 1 {
		return "", ErrUserNotFound
	}
	return "Rina", nil
}

func loadProfile(id int) (string, error) {
	name, err := findUser(id)
	if err != nil {
		return "", fmt.Errorf("load profile id=%d: %w", id, err)
	}
	return "Profil user: " + name, nil
}

func main() {
	p, err := loadProfile(2)
	if err != nil {
		if errors.Is(err, ErrUserNotFound) {
			fmt.Println("tampilkan halaman 404 user")
			return
		}
		fmt.Println("error umum:", err)
		return
	}
	fmt.Println(p)
}
```

Perubahan dibanding tahap 1:
- Ada sentinel error (`ErrUserNotFound`).
- Error diberi konteks tambahan tanpa kehilangan identitas aslinya.

### Tahap 3 — `panic` + `recover` di boundary
```go
package main

import "fmt"

func mustParseConfig(cfg string) {
	if cfg == "" {
		panic("config kosong: tidak bisa lanjut")
	}
}

func runApp(cfg string) (err error) {
	defer func() {
		if r := recover(); r != nil {
			err = fmt.Errorf("aplikasi dihentikan dengan aman: %v", r)
		}
	}()

	mustParseConfig(cfg)
	fmt.Println("aplikasi berjalan")
	return nil
}

func main() {
	if err := runApp(""); err != nil {
		fmt.Println("error:", err)
	}
}
```

Checklist pemahaman:
- [ ] Saya paham `recover` hanya bekerja di `defer`.
- [ ] Saya paham `recover` menangkap panic di goroutine yang sama.
- [ ] Saya paham `panic` bukan pengganti `return error`.

## Common mistakes
### Mistake 1 — Mengabaikan error
- **Gejala:** program “jalan”, tapi hasil aneh atau data rusak.
- **Penyebab:** error dibuang (`_`) atau tidak dicek.
- **Perbaikan:** selalu cek `if err != nil` di titik yang relevan.

### Mistake 2 — Semua masalah diperlakukan sebagai panic
- **Gejala:** aplikasi sering crash untuk kasus input user biasa.
- **Penyebab:** panic dipakai untuk flow normal.
- **Perbaikan:** gunakan `return error` untuk kondisi yang bisa diprediksi.

### Mistake 3 — Wrapping salah format
- **Gejala:** `errors.Is` tidak bisa mengenali root error.
- **Penyebab:** pakai `%v` alih-alih `%w`.
- **Perbaikan:** gunakan `%w` saat meneruskan error.

## Latihan
### Basic
Buat fungsi `toInt(s string) (int, error)` yang:
- Mengembalikan error bila string bukan angka.
- Menampilkan pesan ramah pengguna di `main`.

### Menengah
Buat alur 2 layer:
- `readConfig(path string)`,
- `initApp(path string)`.

Jika file config gagal dibaca, bungkus error dengan konteks path, lalu cek dengan `errors.Is`.

### Challenge
Buat worker sederhana yang:
- Memproses daftar tugas (`[]string`),
- Setiap tugas bisa mengembalikan `error`,
- Worker punya `defer-recover` agar panic pada satu tugas tidak menghentikan seluruh batch.

## Ringkasan
- `error` adalah mekanisme utama untuk kegagalan yang bisa diprediksi.
- Wrapping (`%w`) membuat debugging lebih jelas tanpa kehilangan root cause.
- `panic` dipakai hemat untuk kondisi fatal/invariant rusak.
- `recover` adalah pagar terakhir, bukan mekanisme kontrol flow harian.

## Referensi resmi + lanjutan
- **[Go Blog: Error handling and Go](https://go.dev/blog/error-handling-and-go)** — fondasi filosofi error idiomatik di Go.
  _Kapan baca:_ saat ingin paham kenapa Go tidak pakai exception tradisional.
- **[Go Blog: Working with Errors in Go 1.13](https://go.dev/blog/go1.13-errors)** — wrapping/unwrapping dan `errors.Is/As`.
  _Kapan baca:_ ketika mulai membangun rantai error yang bisa ditelusuri.
- **[Package `errors`](https://pkg.go.dev/errors)** — API resmi `New`, `Is`, `As`, `Join`.
  _Kapan baca:_ saat implementasi error handling di kode nyata.
- **[Package `fmt#Errorf`](https://pkg.go.dev/fmt#Errorf)** — detail `%w` untuk wrapping error.
  _Kapan baca:_ ketika ingin menambah konteks error tanpa kehilangan sumber.
- **[Go Spec: Handling panics](https://go.dev/ref/spec#Handling_panics)** — aturan formal `panic`/`recover`.
  _Kapan baca:_ saat butuh batas jelas kapan panic/recover valid dipakai.
- **[Indeks referensi fundamental](./99-referensi-fundamental.md)** — kumpulan referensi topik fundamental.
  _Kapan baca:_ saat ingin membandingkan error handling dengan topik sebelumnya.

## Navigasi fundamental
- [Kembali ke Start Here](./00-start-here.md)
- **Sebelumnya:** [05-pointer-struct-method-dasar.md](./05-pointer-struct-method-dasar.md)
- **Selanjutnya:** [07-interface-dasar-composition.md](./07-interface-dasar-composition.md)
