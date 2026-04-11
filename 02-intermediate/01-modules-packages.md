# Modules, Packages, dan Workspace

## Tujuan
- Memahami batas package dan module
- Memahami dependency management
- Mengenal `go work` untuk multi-module

## Inti konsep
- **Package**: unit kode di satu folder.
- **Module**: kumpulan package yang dikelola oleh `go.mod`.

## Perintah penting

```powershell
go mod init example/myapp
go mod tidy
go list ./...
go work init ./module-a ./module-b
```

## Praktik baik
- Gunakan nama package singkat dan bermakna.
- Simpan API publik yang stabil di package non-`internal`.
- Gunakan folder `internal/` untuk detail implementasi yang tidak untuk dipakai dari luar module.

