# Control Flow: if, switch, for, range

## Tujuan
- Mampu menulis percabangan dan perulangan idiomatik Go
- Memahami `range` di slice, map, string

## Contoh

```go
package main

import "fmt"

func main() {
	nilai := 78
	if nilai >= 75 {
		fmt.Println("Lulus")
	} else {
		fmt.Println("Belum lulus")
	}

	hari := "Senin"
	switch hari {
	case "Sabtu", "Minggu":
		fmt.Println("Weekend")
	default:
		fmt.Println("Hari kerja")
	}

	total := 0
	for i := 1; i <= 5; i++ {
		total += i
	}
	fmt.Println(total)
}
```

## Deep note
- Go hanya punya satu keyword loop: `for`.
- `switch` default tidak fallthrough otomatis.
- `range` pada string berjalan per rune (UTF-8 aware), bukan byte mentah.

