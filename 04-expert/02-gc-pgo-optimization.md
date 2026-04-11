# GC, PGO, dan Optimisasi

## Tujuan
- Memahami biaya GC (CPU vs memory tradeoff)
- Memahami `GOGC` dan memory limit
- Menerapkan PGO untuk optimisasi berbasis profil

## GC quick notes
- Heap growth memengaruhi frekuensi GC.
- Naikkan `GOGC` -> biasanya CPU overhead GC turun, tapi memory naik.
- Turunkan `GOGC` -> memory turun, CPU GC naik.

## PGO workflow sederhana
1. Build dan jalankan aplikasi.
2. Kumpulkan profile CPU representatif.
3. Build ulang dengan profile.
4. Bandingkan baseline vs hasil PGO.

## Perintah contoh

```powershell
go build -pgo=auto ./...
go build -pgo=off ./...
```

## Prinsip optimisasi
- Ukur dulu, optimasi kemudian.
- Targetkan hotspot nyata, bukan asumsi.

