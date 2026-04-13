# Pointer, Struct, dan Method Dasar

Kalau kamu baru masuk ke Go, kombinasi `struct`, pointer (`*`), dan method receiver sering terasa “misterius”.  
Di artikel ini kita pecah pelan-pelan: bagaimana data dikemas, bagaimana perubahan data terjadi, dan kenapa receiver method penting untuk desain kode yang rapi.

## Tujuan belajar
Setelah selesai, kamu diharapkan bisa:
- Memahami fungsi `struct` sebagai pengelompokan data yang terkait.
- Memahami perbedaan value (copy) vs pointer (alamat data asli).
- Menjelaskan kapan pakai method dengan value receiver vs pointer receiver.
- Menulis kode kecil yang memodifikasi state dengan aman dan jelas.

## Mental model
Bayangkan kamu punya **kartu profil**:
- `struct` = satu kartu lengkap (nama, umur, saldo, dst).
- value copy = fotokopi kartu; ubah fotokopi tidak mengubah kartu asli.
- pointer = catatan lokasi lemari kartu asli; lewat alamat ini kamu bisa ubah kartu asli.

```text
u (value)                      pu (pointer)
+----------------------+       +---------------------+
| Name: "Rina"         |       | alamat: 0x14000...  | ----+
| Age: 19              |       +---------------------+     |
+----------------------+                                     |
                                                             v
                                                   +----------------------+
                                                   | Name: "Rina"         |
                                                   | Age: 19              |
                                                   +----------------------+
```

1. **Analogi non-teknis:** fotokopi dokumen vs dokumen asli.
2. **Pemetaan teknis:** value = salinan data, pointer = alamat ke data.
3. **Batas analogi:** kamu tidak mengatur alamat memori manual; Go runtime mengelola itu.

## Konsep inti (step-by-step)
### Step 1 — `struct` menyatukan data
- **Apa:** tipe custom dengan beberapa field.
- **Kenapa penting:** data terkait tidak tercecer di variabel terpisah.
- **Kapan dipakai:** saat memodelkan entitas (User, Product, Order, dst).
- **Catatan jebakan:** nama field kapital berarti exported (bisa diakses package lain).

### Step 2 — Pointer untuk mengubah data asli
- **Apa:** `*T` menyimpan alamat dari nilai bertipe `T`.
- **Kenapa penting:** menghindari copy besar dan memungkinkan mutasi langsung.
- **Kapan dipakai:** update state, fungsi/method mutasi, struct cukup besar.
- **Catatan jebakan:** pointer `nil` jika di-dereference akan panic.

### Step 3 — Method receiver menentukan semantik
- **Apa:** method bisa receiver value (`func (u User)`) atau pointer (`func (u *User)`).
- **Kenapa penting:** menentukan apakah method mengubah state asli.
- **Kapan dipakai:** pointer receiver untuk mutasi; value receiver untuk read-only sederhana.
- **Catatan jebakan:** receiver campur-campur tanpa alasan bikin API membingungkan.

## Contoh kode bertahap
### Tahap 1 — Struct minimum
```go
package main

import "fmt"

type User struct {
	Name string
	Age  int
}

func main() {
	u := User{Name: "Rina", Age: 19}
	fmt.Printf("%s berumur %d tahun\n", u.Name, u.Age)
}
```

Penjelasan singkat:
- `User` mengelompokkan data jadi satu objek.
- Literal `User{...}` adalah cara paling umum untuk inisialisasi.

### Tahap 2 — Value vs pointer saat mutasi
```go
package main

import "fmt"

type User struct {
	Name string
	Age  int
}

func tambahUmurByValue(u User) {
	u.Age++
}

func tambahUmurByPointer(u *User) {
	u.Age++
}

func main() {
	u := User{Name: "Rina", Age: 19}

	tambahUmurByValue(u)
	fmt.Println("setelah by value:", u.Age) // 19 (tidak berubah)

	tambahUmurByPointer(&u)
	fmt.Println("setelah by pointer:", u.Age) // 20 (berubah)
}
```

Perubahan dibanding tahap 1:
- Fungsi by-value menerima salinan.
- Fungsi by-pointer menerima alamat data asli (`&u`).

### Tahap 3 — Method receiver yang lebih idiomatik
```go
package main

import "fmt"

type Wallet struct {
	Owner   string
	Balance int
}

func (w Wallet) Snapshot() string {
	return fmt.Sprintf("%s punya saldo %d", w.Owner, w.Balance)
}

func (w *Wallet) Deposit(amount int) {
	if amount <= 0 {
		return
	}
	w.Balance += amount
}

func main() {
	w := Wallet{Owner: "Budi", Balance: 100}
	fmt.Println(w.Snapshot())

	w.Deposit(50)
	fmt.Println(w.Snapshot())
}
```

Checklist pemahaman:
- [ ] Saya paham kenapa `Deposit` menggunakan pointer receiver.
- [ ] Saya paham method baca-only seperti `Snapshot` tidak wajib pointer receiver.
- [ ] Saya bisa memutuskan kapan perlu `&obj`.

## Common mistakes
### Mistake 1 — Mengira semua fungsi bisa mengubah nilai asli
- **Gejala:** field tidak berubah setelah fungsi dipanggil.
- **Penyebab:** argumen dikirim by value (copy).
- **Perbaikan:** kirim pointer (`&obj`) kalau butuh mutasi.

### Mistake 2 — Dereference pointer `nil`
- **Gejala:** panic `invalid memory address or nil pointer dereference`.
- **Penyebab:** pointer belum menunjuk objek valid.
- **Perbaikan:** inisialisasi objek dulu atau cek `if p == nil`.

### Mistake 3 — Receiver tidak konsisten
- **Gejala:** sebagian method value receiver, sebagian pointer receiver tanpa alasan jelas.
- **Penyebab:** desain API tidak konsisten.
- **Perbaikan:** untuk tipe yang sering dimutasi, gunakan pointer receiver secara konsisten.

## Latihan
### Basic
1. Buat `struct Product { Name string; Price int }`.
2. Tulis dua fungsi diskon:
   - `DiskonByValue(p Product)`
   - `DiskonByPointer(p *Product)`
3. Bandingkan hasil akhir `Price`.

### Menengah
Buat `struct Counter` dengan method:
- `Increment()` (pointer receiver),
- `Value()` (value receiver).

Tambahkan log sederhana agar tiap increment terlihat.

### Challenge
Buat `struct BankAccount` dengan method:
- `Deposit(amount int) error`
- `Withdraw(amount int) error`

Constraint:
- Saldo tidak boleh negatif.
- Jumlah transaksi harus tercatat di field `TxCount`.

## Ringkasan
- `struct` membuat data lebih terstruktur.
- Value = salinan; pointer = referensi ke data asli.
- Pointer receiver dipakai saat method perlu mutasi state.
- Konsistensi receiver meningkatkan keterbacaan dan maintainability.

## Referensi resmi + lanjutan
- **[A Tour of Go: Pointers](https://go.dev/tour/moretypes/1)** — dasar pointer secara visual.
  _Kapan baca:_ saat masih bingung bedanya copy value vs referensi data.
- **[A Tour of Go: Structs](https://go.dev/tour/moretypes/2)** — struktur data dasar di Go.
  _Kapan baca:_ ketika mulai memodelkan data ke dalam tipe sendiri.
- **[A Tour of Go: Methods](https://go.dev/tour/methods/1)** — cara menempelkan perilaku ke tipe.
  _Kapan baca:_ saat belajar receiver value vs pointer.
- **[Go Spec: Method declarations](https://go.dev/ref/spec#Method_declarations)** — aturan formal deklarasi method.
  _Kapan baca:_ ketika butuh kepastian aturan receiver/type method set.
- **[Effective Go](https://go.dev/doc/effective_go)** — idiom desain API kecil dan clean.
  _Kapan baca:_ setelah contoh dasar jalan, untuk merapikan style kode.
- **[Indeks referensi fundamental](./99-referensi-fundamental.md)** — katalog link resmi per topik.
  _Kapan baca:_ saat ingin lanjut pendalaman ke topik fundamental lain.

## Navigasi fundamental
- [Kembali ke Start Here](./00-start-here.md)
- **Sebelumnya:** [04-slices-maps-deep-dive.md](./04-slices-maps-deep-dive.md)
- **Selanjutnya:** [06-error-handling-dasar.md](./06-error-handling-dasar.md)
