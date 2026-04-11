# Memory Model dan Race Detector

## Tujuan
- Memahami kenapa data race berbahaya
- Memahami kebutuhan sinkronisasi (`mutex`, `channel`, `atomic`)
- Mampu menjalankan race detector

## Prinsip inti
- Data race terjadi saat dua goroutine mengakses lokasi memori sama dan minimal satu write tanpa sinkronisasi yang benar.
- Program yang race-free punya perilaku lebih deterministik (DRF-SC).

## Cara cek race

```powershell
go test -race ./...
go run -race .
```

## Kapan pakai apa
- **Mutex**: proteksi state bersama.
- **Channel**: koordinasi alur komunikasi.
- **Atomic**: operasi low-level spesifik counter/flag.

## Anti-pattern umum
- Menulis ke map dari beberapa goroutine tanpa proteksi.
- Mengandalkan busy-wait loop tanpa sinkronisasi.

