# Goroutine, Channels, dan Select

## Tujuan
- Menjalankan pekerjaan concurrent dengan aman
- Memahami channel buffering vs non-buffering
- Menggunakan `select` untuk multiplexer event

## Contoh pola worker sederhana

```go
jobs := make(chan int)
results := make(chan int)

go func() {
	for j := range jobs {
		results <- j * 2
	}
}()

jobs <- 10
close(jobs)
fmt.Println(<-results) // 20
```

## Deep note
- Gunakan channel untuk komunikasi sinkron antar goroutine.
- Hindari goroutine leak: semua goroutine harus punya jalur selesai yang jelas.
- `select` membantu menangani timeout/cancel/event multi-channel.

