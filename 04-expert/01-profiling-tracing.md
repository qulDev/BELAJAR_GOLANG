# Profiling dan Tracing

## Tujuan
- Mengidentifikasi bottleneck CPU/memori
- Memahami perbedaan profiling vs tracing
- Mampu membaca output pprof secara praktis

## Profil penting
- CPU profile
- Heap profile
- Goroutine profile
- Block/mutex profile

## Perintah umum

```powershell
go test -cpuprofile cpu.out -memprofile mem.out ./...
go tool pprof cpu.out
go tool pprof mem.out
```

## Tracing
- Gunakan tracing saat ingin melihat timeline event runtime (scheduler, syscall, GC) dan latensi lintas operasi.

## Foto referensi (contoh visual analisis)
- Flame graph dan pprof graph bisa dipakai untuk membantu pembaca melihat hotspot performa.

