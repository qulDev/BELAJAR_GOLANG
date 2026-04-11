# Sintaks dan Tipe Data Dasar

## Tujuan
- Memahami deklarasi variabel/konstanta
- Mengenal tipe dasar Go
- Memahami zero value

## Contoh singkat

```go
package main

import "fmt"

func main() {
	var nama string = "Gopher"
	umur := 2
	const bahasa = "Go"
	var aktif bool

	fmt.Println(nama, umur, bahasa, aktif) // aktif default: false
}
```

## Tipe dasar yang wajib dikuasai
- `bool`
- `string`
- `int`, `int64`
- `float64`
- `byte`, `rune`

## Deep note
- Go adalah bahasa statically typed.
- Inference (`:=`) tetap menghasilkan tipe statis di compile time.
- Zero value mengurangi kebutuhan inisialisasi manual.

