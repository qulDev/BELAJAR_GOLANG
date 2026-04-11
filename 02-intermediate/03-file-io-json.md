# File I/O dan JSON

## Tujuan
- Membaca/menulis file
- Melakukan marshal/unmarshal JSON
- Menangani error I/O dengan benar

## Contoh JSON

```go
type User struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}
```

```go
u := User{Name: "Ayu", Age: 24}
b, err := json.Marshal(u)
if err != nil {
	return err
}
fmt.Println(string(b))
```

## Contoh baca file

```go
data, err := os.ReadFile("config.json")
if err != nil {
	return err
}
```

## Praktik baik
- Selalu cek error I/O.
- Gunakan struct tags JSON yang konsisten.
- Untuk data besar, gunakan `json.Decoder`/`json.Encoder` agar streaming-friendly.

