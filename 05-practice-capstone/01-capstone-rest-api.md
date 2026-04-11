# Capstone: REST API Go (Production-minded)

![Go Gopher App Engine](https://upload.wikimedia.org/wikipedia/commons/d/df/Go_gopher_app_engine_color.jpg)

## Goal
Bangun REST API sederhana (mis. katalog produk/album) dengan standar engineering:
- Struktur project rapi
- Unit test + race check
- Basic profiling
- Dokumentasi endpoint

## Fase capstone

1. **Scaffold**
   - Module init
   - Route dasar (`GET`, `POST`, `GET by id`)

2. **Data layer**
   - In-memory dulu, lanjut DB bila siap

3. **Quality**
   - Table-driven tests
   - `go test -race`

4. **Reliability**
   - Context timeout pada request
   - Error handling jelas

5. **Performance check**
   - Tambahkan profiling sederhana

## Checklist kelulusan

- [ ] Endpoint utama berjalan sesuai kontrak
- [ ] Test lulus
- [ ] Race check bersih
- [ ] Error response konsisten
- [ ] Dokumentasi endpoint ada

