# Setup Go dan Workflow Dasar (Tanpa Bingung)

Kalau kamu baru mulai Go, tantangan pertama biasanya bukan sintaks, tapi setup awal: install, bikin project, lalu perintah mana yang dipakai duluan. Artikel ini fokus ke alur praktis yang benar dari nol. Setelah selesai, kamu akan punya project Go pertama yang rapi dan siap dipakai belajar lebih lanjut.

---

## Untuk siapa materi ini
- Pemula total yang baru pertama kali menyentuh Go.
- Pemula yang sudah install Go tapi belum paham alur `go mod`, `go run`, dan `go test`.
- Pembaca yang ingin membangun kebiasaan workflow Go yang benar sejak awal.

---

## Prasyarat
- Laptop/PC dengan akses internet.
- Bisa membuka terminal (PowerShell/CMD/Terminal).
- Sudah membaca [Start Here](./00-start-here.md).

---

## Tujuan belajar
Setelah menyelesaikan artikel ini, kamu diharapkan bisa:
- Menginstal Go dan memverifikasi toolchain berjalan normal.
- Membuat project baru berbasis module (`go.mod`).
- Menjalankan program (`go run`), test (`go test`), dan merapikan dependency (`go mod tidy`).

---

## Mental model
Anggap project Go seperti dapur kecil:

```text
Kode (.go) -> go run/go build -> Program jadi
         \-> go test -> Cek perilaku fungsi
go.mod -> "identitas project + daftar dependency"
```

1. **Analogi non-teknis:** kamu memasak dari resep.
2. **Pemetaan analogi ke istilah teknis:** file `.go` = resep, `go.mod` = daftar bahan + identitas dapur, `go run` = masak cepat, `go build` = hasil siap dibawa.
3. **Batas analogi:** compiler Go akan menolak kode salah sebelum program jalan, jadi tidak semua "resep" bisa langsung dieksekusi.

---

## Konsep inti bertahap (step-by-step)
### Step 1 — Install Go dan verifikasi
- **Apa:** pasang Go dari situs resmi, lalu cek `go version`.
- **Kenapa penting:** memastikan compiler dan toolchain sudah ada di PATH.
- **Kapan dipakai:** sekali saat setup awal, lalu saat update versi Go.
- **Catatan jebakan:** kalau perintah `go` tidak dikenali, biasanya PATH belum benar atau terminal belum di-restart.

### Step 2 — Mulai project dengan Go module
- **Apa:** buat folder baru dan jalankan `go mod init nama/module`.
- **Kenapa penting:** module adalah fondasi dependency management modern di Go.
- **Kapan dipakai:** setiap memulai project baru.
- **Catatan jebakan:** nama module jangan pakai spasi/karakter aneh karena akan menyulitkan saat project berkembang.

### Step 3 — Jalankan workflow harian minimum
- **Apa:** gunakan `go run .`, `go test ./...`, dan `go mod tidy`.
- **Kenapa penting:** ini alur minimum supaya coding tetap cepat dan terstruktur.
- **Kapan dipakai:** setiap menambah/mengubah fitur.
- **Catatan jebakan:** `go test` hanya akan menjalankan file berakhiran `_test.go`.

---

## Contoh kode bertahap
### Tahap 1 — Versi minimum (program pertama)
```powershell
mkdir hello-go
cd hello-go
go mod init example/hello-go
```

`main.go`
```go
package main

import "fmt"

func main() {
	fmt.Println("Halo, Go!")
}
```

Jalankan:
```powershell
go run .
```

Penjelasan singkat output/perilaku:
- Terminal menampilkan `Halo, Go!`.
- `go run .` akan compile sementara lalu langsung mengeksekusi.

### Tahap 2 — Tambah kebutuhan nyata (fungsi sapaan)
`main.go`
```go
package main

import "fmt"

func greeting(name string) string {
	if name == "" {
		name = "Gopher"
	}
	return fmt.Sprintf("Halo, %s!", name)
}

func main() {
	fmt.Println(greeting("Ayu"))
	fmt.Println(greeting(""))
}
```

Perubahan dibanding tahap 1:
- Logika dipindah ke fungsi `greeting`.
- Ada fallback saat input kosong.

### Tahap 3 — Versi lebih idiomatik (siap dites)
`main.go`
```go
package main

import "fmt"

func greeting(name string) string {
	if name == "" {
		name = "Gopher"
	}
	return fmt.Sprintf("Halo, %s!", name)
}

func main() {
	fmt.Println(greeting("Dunia"))
}
```

`main_test.go`
```go
package main

import "testing"

func TestGreeting(t *testing.T) {
	got := greeting("Budi")
	want := "Halo, Budi!"
	if got != want {
		t.Fatalf("got %q, want %q", got, want)
	}
}
```

Jalankan:
```powershell
go test ./...
```

Checklist pemahaman:
- [ ] Saya paham beda `go run .` dan `go test ./...`.
- [ ] Saya bisa membuat project Go baru dari nol.

---

## Common mistakes (minimal 2)
### Mistake 1 — Menjalankan perintah di folder yang salah
- **Gejala:** error `go.mod file not found`.
- **Penyebab:** command dijalankan bukan dari root module.
- **Perbaikan:** pindah ke folder yang berisi `go.mod` lalu jalankan ulang.

### Mistake 2 — Nama module tidak rapi dari awal
- **Gejala:** kebingungan saat import package sendiri di project yang mulai besar.
- **Penyebab:** module path asal tulis, misalnya tidak konsisten atau mengandung format yang buruk.
- **Perbaikan:** pakai nama module yang jelas dan konsisten (contoh latihan: `example/nama-project`).

### Mistake 3 — Tidak pernah menjalankan `go mod tidy`
- **Gejala:** isi `go.mod`/`go.sum` tidak sinkron dengan import terbaru.
- **Penyebab:** dependency ditambah/hapus tapi module tidak dirapikan.
- **Perbaikan:** jalankan `go mod tidy` setelah perubahan dependency.

---

## Latihan (basic/menengah/challenge)
### Basic
- Buat module baru bernama `example/latihan-setup`.
- Tulis program yang menampilkan `Belajar Go itu seru`.
- **Kriteria lulus:** `go run .` berjalan tanpa error.

### Menengah
- Buat fungsi `greeting(name string) string`.
- Jika `name` kosong, pakai default `"Teman Go"`.
- **Constraint:** logika default harus berada di fungsi, bukan di `main`.

### Challenge
- Tambahkan test untuk 2 kasus: nama terisi dan nama kosong.
- Jalankan `go test ./...` sampai pass.
- **Hint:** gunakan table-driven test agar tidak duplikatif.

---

## Ringkasan
- Setup Go yang benar dimulai dari install + verifikasi `go version`.
- Setiap project baru sebaiknya langsung pakai module (`go mod init`).
- Workflow minimum yang perlu dibiasakan: `go run .`, `go test ./...`, `go mod tidy`.
- Next action: lanjut ke sintaks dan tipe data supaya bisa menulis logika program.

---

## Referensi resmi + lanjutan
- **[Dokumentasi instalasi Go](https://go.dev/doc/install)** — panduan install per OS dari sumber resmi.
  _Kapan baca:_ saat setup awal atau upgrade versi Go.
- **[Tutorial resmi: Getting Started](https://go.dev/doc/tutorial/getting-started)** — alur lengkap membuat project Go pertama.
  _Kapan baca:_ kalau baru pertama pakai `go run`, `go mod init`, dan `go build`.
- **[Referensi Go Modules](https://go.dev/ref/mod)** — spesifikasi perilaku `go.mod` dan dependency.
  _Kapan baca:_ saat bingung soal versi module, replace, atau tidy.
- **[A Tour of Go](https://go.dev/tour/welcome/1)** — playground interaktif untuk pemanasan konsep dasar.
  _Kapan baca:_ setelah setup selesai agar tangan cepat terbiasa syntax Go.
- **[Indeks referensi fundamental](./99-referensi-fundamental.md)** — ringkasan link resmi per topik.
  _Kapan baca:_ saat ingin lompat ke referensi topik lain tanpa cari manual.

---

## Navigasi fundamental
- [Kembali ke Start Here](./00-start-here.md)
- **Sebelumnya:** [00-start-here.md](./00-start-here.md)
- **Selanjutnya:** [02-syntax-dan-tipe-data.md](./02-syntax-dan-tipe-data.md)

