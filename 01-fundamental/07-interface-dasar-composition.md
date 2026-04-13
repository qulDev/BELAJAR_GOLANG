# Interface Dasar dan Composition Sederhana

Setelah paham struct, pointer, dan error, langkah penting berikutnya adalah memahami **abstraksi perilaku** lewat interface, lalu menyusun objek dengan **composition** (bukan warisan/inheritance klasik).  
Keduanya membuat kode Go lebih fleksibel, mudah dites, dan mudah diubah.

## Tujuan belajar
Setelah artikel ini, kamu diharapkan bisa:
- Memahami interface sebagai kontrak perilaku, bukan wadah data.
- Menulis interface kecil yang mudah dipakai ulang.
- Menerapkan composition untuk menyusun kemampuan objek.
- Menghindari jebakan umum: interface terlalu besar, atau dipakai terlalu cepat.

## Mental model
Bayangkan interface sebagai **colokan listrik standar**:
- Kamu tidak peduli merek alatnya.
- Selama colokannya cocok, alat bisa dipakai.

Di Go:
- interface mendefinisikan **apa yang bisa dilakukan** (method),
- tipe konkret menentukan **bagaimana cara melakukannya**.

```text
[Kode pemakai] --> butuh: Notifier
                     |
          +----------+----------+
          |                     |
   EmailNotifier         SlackNotifier
   (implementasi A)      (implementasi B)
```

1. **Analogi non-teknis:** stopkontak dan colokan standar.
2. **Pemetaan teknis:** interface = kontrak method, struct = implementasi.
3. **Batas analogi:** interface tidak memaksa field/data tertentu, hanya method.

## Konsep inti (step-by-step)
### Step 1 — Interface mendefinisikan perilaku
- **Apa:** kumpulan signature method.
- **Kenapa penting:** caller tidak tergantung detail implementasi.
- **Kapan dipakai:** saat ada lebih dari satu implementasi yang mungkin.
- **Catatan jebakan:** jangan buat interface besar kalau belum perlu.

### Step 2 — Small interface lebih mudah dikelola
- **Apa:** interface 1–3 method yang fokus.
- **Kenapa penting:** lebih mudah diuji (mock/stub sederhana).
- **Kapan dipakai:** dependency service, adapter I/O, plugin sederhana.
- **Catatan jebakan:** “god interface” (terlalu gemuk) bikin coupling tinggi.

### Step 3 — Composition untuk menyusun kemampuan
- **Apa:** gabung komponen lewat field/embedding.
- **Kenapa penting:** lebih eksplisit daripada inheritance.
- **Kapan dipakai:** menambah kemampuan tanpa hierarki kelas rumit.
- **Catatan jebakan:** embedding berlebihan bisa membingungkan asal method.

## Contoh kode bertahap
### Tahap 1 — Interface paling minimum
```go
package main

import "fmt"

type Speaker interface {
	Speak() string
}

type Cat struct{}
type Dog struct{}

func (Cat) Speak() string { return "meong" }
func (Dog) Speak() string { return "guk guk" }

func bunyikan(s Speaker) {
	fmt.Println(s.Speak())
}

func main() {
	bunyikan(Cat{})
	bunyikan(Dog{})
}
```

Penjelasan:
- `bunyikan` tidak peduli tipe konkret selama punya `Speak() string`.

### Tahap 2 — Interface untuk dependency yang bisa ditukar
```go
package main

import "fmt"

type Notifier interface {
	Send(msg string) error
}

type EmailNotifier struct{}

func (EmailNotifier) Send(msg string) error {
	fmt.Println("[EMAIL]", msg)
	return nil
}

type OrderService struct {
	notifier Notifier
}

func NewOrderService(n Notifier) *OrderService {
	return &OrderService{notifier: n}
}

func (s *OrderService) Checkout(orderID string) error {
	// proses checkout disederhanakan
	return s.notifier.Send("Order " + orderID + " sukses")
}

func main() {
	svc := NewOrderService(EmailNotifier{})
	_ = svc.Checkout("ORD-001")
}
```

Perubahan dibanding tahap 1:
- Interface dipakai untuk dependency nyata (`Notifier`).
- Implementasi notifier bisa diganti tanpa ubah `OrderService`.

### Tahap 3 — Composition interface + struct embedding
```go
package main

import (
	"bytes"
	"fmt"
)

type Reader interface {
	Read(p []byte) (n int, err error)
}

type Writer interface {
	Write(p []byte) (n int, err error)
}

type ReadWriter interface {
	Reader
	Writer
}

type Audit struct{}

func (Audit) Log(msg string) {
	fmt.Println("[AUDIT]", msg)
}

type BufferService struct {
	Audit // composition via embedding
	buf   bytes.Buffer
}

func (s *BufferService) Read(p []byte) (int, error)  { return s.buf.Read(p) }
func (s *BufferService) Write(p []byte) (int, error) { return s.buf.Write(p) }

func copyOnce(rw ReadWriter, data string) {
	_, _ = rw.Write([]byte(data))
	tmp := make([]byte, len(data))
	_, _ = rw.Read(tmp)
	fmt.Println("hasil:", string(tmp))
}

func main() {
	s := &BufferService{}
	s.Log("mulai copy")
	copyOnce(s, "belajar-go")
}
```

Checklist pemahaman:
- [ ] Saya paham interface bisa digabungkan dari interface kecil.
- [ ] Saya paham composition (`Audit` di-embed) menambah kemampuan tanpa inheritance.
- [ ] Saya paham caller cukup tahu kontrak (`ReadWriter`), bukan detail objek.

## Common mistakes
### Mistake 1 — Membuat interface terlalu cepat
- **Gejala:** banyak interface tapi hanya satu implementasi dan tidak pernah ditukar.
- **Penyebab:** over-abstraction di awal.
- **Perbaikan:** mulai dari concrete type dulu; buat interface saat ada kebutuhan nyata.

### Mistake 2 — Interface terlalu gemuk
- **Gejala:** implementasi jadi berat karena harus memenuhi banyak method.
- **Penyebab:** satu interface menampung terlalu banyak tanggung jawab.
- **Perbaikan:** pecah jadi interface kecil yang fokus.

### Mistake 3 — Bingung dengan `nil` interface
- **Gejala:** `if x == nil` tidak bekerja seperti yang diharapkan.
- **Penyebab:** nilai konkret di dalam interface nil, tapi tipe interfacenya tetap ada.
- **Perbaikan:** pahami bahwa interface menyimpan `(type, value)`; keduanya harus nil agar `x == nil`.

## Latihan
### Basic
Buat interface `Printer` dengan method `Print() string`.
- Implementasi `InvoicePrinter` dan `ReceiptPrinter`.
- Buat fungsi `render(p Printer)` yang mencetak output.

### Menengah
Buat `PaymentService` yang menerima dependency interface `PaymentGateway`:
- Method `Charge(amount int) error`.
- Buat dua implementasi: `MockGateway` (selalu sukses) dan `FailingGateway` (selalu error).

### Challenge
Bangun mini aplikasi log:
- Interface kecil `Formatter` dan `Writer`.
- Struct `Logger` yang compose keduanya.
- Tambahkan 2 formatter (plain, JSON sederhana) dan 2 writer (console, memory buffer).

## Ringkasan
- Interface di Go mendeskripsikan perilaku, bukan struktur data.
- Small interface + dependency injection membuat kode fleksibel dan testable.
- Composition adalah cara utama menyusun kemampuan objek di Go.
- Hindari over-abstraction: buat interface saat ada kebutuhan.

## Referensi resmi + lanjutan
- **[A Tour of Go: Interfaces](https://go.dev/tour/methods/9)** — pengantar interface secara bertahap.
  _Kapan baca:_ saat butuh contoh sederhana implementasi implicit interface.
- **[Effective Go: Interfaces and other types](https://go.dev/doc/effective_go#interfaces_and_types)** — prinsip small interface dan desain API.
  _Kapan baca:_ saat mulai melakukan abstraction di project sendiri.
- **[Package `io`](https://pkg.go.dev/io)** — contoh nyata interface kecil yang sangat dipakai.
  _Kapan baca:_ saat belajar composition lewat `Reader`, `Writer`, dll.
- **[Go Blog: The Laws of Reflection](https://go.dev/blog/laws-of-reflection)** — melihat hubungan interface dan reflection.
  _Kapan baca:_ setelah paham interface dasar dan ingin tahu layer internal.
- **[Indeks referensi fundamental](./99-referensi-fundamental.md)** — referensi resmi lintas topik.
  _Kapan baca:_ saat perlu jembatan ke materi low-level atau error handling.

## Navigasi fundamental
- [Kembali ke Start Here](./00-start-here.md)
- **Sebelumnya:** [06-error-handling-dasar.md](./06-error-handling-dasar.md)
- **Selanjutnya:** [08-low-level-go-dasar.md](./08-low-level-go-dasar.md)
