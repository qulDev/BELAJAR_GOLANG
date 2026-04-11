# Testing Dasar di Go

## Tujuan
- Menulis unit test dengan package `testing`
- Memakai table-driven tests
- Memahami subtests dan coverage dasar

## Contoh table-driven test

```go
package mathx

import "testing"

func Add(a, b int) int { return a + b }

func TestAdd(t *testing.T) {
	cases := []struct {
		a, b int
		want int
	}{
		{1, 2, 3},
		{0, 0, 0},
		{-1, 1, 0},
	}

	for _, tc := range cases {
		got := Add(tc.a, tc.b)
		if got != tc.want {
			t.Fatalf("Add(%d, %d) = %d, want %d", tc.a, tc.b, got, tc.want)
		}
	}
}
```

## Perintah penting

```powershell
go test ./...
go test -v ./...
go test -cover ./...
```

## Praktik baik
- Error message test harus jelas dan actionable.
- Jangan abaikan `err` return value.
- Pisahkan setup test bila mulai kompleks.

