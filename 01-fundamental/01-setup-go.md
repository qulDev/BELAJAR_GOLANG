# Setup Go dan Workflow Dasar

## Tujuan
- Instal Go
- Verifikasi toolchain
- Menjalankan program pertama
- Memahami alur `go mod init`, `go run`, `go test`

## Checklist setup

1. Install Go dari rilis resmi.
2. Cek versi:

```powershell
go version
```

3. Buat project awal:

```powershell
mkdir hello-go
cd hello-go
go mod init example/hello-go
```

4. Buat `main.go`:

```go
package main

import "fmt"

func main() {
	fmt.Println("Halo, Go!")
}
```

5. Jalankan:

```powershell
go run .
```

## Catatan penting
- `go.mod` menyimpan identitas module dan dependency.
- `go mod tidy` dipakai untuk sinkronisasi dependency.
- Biasakan pakai `gofmt` dan `go test` sejak awal.

