# Fuzzing dan Integration Coverage

## Tujuan
- Menulis fuzz test native Go
- Memahami corpus, failing input, minimization
- Mengumpulkan coverage untuk skenario integrasi

## Fuzzing ringkas

```go
func FuzzReverse(f *testing.F) {
	f.Add("hello")
	f.Fuzz(func(t *testing.T, in string) {
		out := Reverse(in)
		if Reverse(out) != in {
			t.Fatalf("double reverse mismatch")
		}
	})
}
```

```powershell
go test -fuzz=Fuzz -run=^$
```

## Integration coverage ringkas
- Build aplikasi dengan `-cover`.
- Jalankan dengan `GOCOVERDIR`.
- Analisis hasil via `go tool covdata`.

## Praktik baik
- Simpan failing input sebagai regression test.
- Gunakan beban kerja realistis untuk coverage integrasi.

