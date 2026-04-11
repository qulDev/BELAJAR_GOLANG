# Context dan Cancellation

## Tujuan
- Mengelola timeout, deadline, dan cancel signal
- Mempropagasi context antar layer aplikasi

## Aturan penting
- `context.Context` jadi parameter pertama fungsi lintas boundary.
- Jangan simpan context di struct untuk dipakai jangka panjang.
- Panggil `cancel()` untuk melepas resource ketika selesai.

## Contoh

```go
ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
defer cancel()

select {
case <-time.After(3 * time.Second):
	fmt.Println("done")
case <-ctx.Done():
	fmt.Println("timeout/canceled:", ctx.Err())
}
```

## Praktik baik
- Gunakan context untuk request-scoped concern (deadline, cancel, tracing metadata).
- Jangan pakai context sebagai tempat data bisnis umum.

