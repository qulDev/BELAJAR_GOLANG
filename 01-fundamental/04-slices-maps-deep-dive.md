# Deep Dive Slices dan Maps

## Tujuan
- Paham perbedaan array vs slice
- Paham kapasitas (`cap`) dan risiko aliasing slice
- Paham karakteristik map dan pola aman penggunaannya

## Slice mental model
- Slice = view ke array backing.
- Operasi `append` bisa:
  1. menulis ke backing array lama, atau
  2. membuat backing array baru.

## Contoh aliasing

```go
a := []int{1, 2, 3, 4}
b := a[:2]
b[0] = 99
// a ikut berubah karena b masih berbagi backing array
```

## Map quick notes
- Map tidak thread-safe untuk write/read concurrent tanpa sinkronisasi.
- Iterasi map tidak menjamin urutan.
- Key map harus comparable.

## Praktik baik
- Copy slice saat perlu isolasi data.
- Gunakan `make(map[K]V)` sebelum write.
- Untuk akses concurrent map, gunakan `sync.Mutex`/`sync.RWMutex` atau `sync.Map` sesuai kebutuhan.

