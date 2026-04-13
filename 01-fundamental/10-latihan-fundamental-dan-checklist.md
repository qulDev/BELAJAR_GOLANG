# 10 - Latihan Fundamental dan Checklist Kelulusan

Dokumen ini adalah **penutup fase fundamental**. Fokusnya: memastikan kamu bukan cuma paham teori, tapi juga bisa mengubah materi 01-09 menjadi program yang berjalan rapi.

## Cara pakai dokumen ini
1. Kerjakan berurutan: **Basic → Menengah → Challenge**.
2. Catat hasil tiap latihan (berhasil/gagal, error yang muncul, dan perbaikan).
3. Kerjakan mini-project setelah level Basic selesai.
4. Gunakan rubrik + checklist untuk menentukan status kelulusan.

## Peta topik 01-09 (acuan latihan)
| Topik | Fokus | Referensi |
|---|---|---|
| 01 | Setup Go dan workflow CLI dasar | [01-setup-go.md](./01-setup-go.md) |
| 02 | Sintaks, variabel, tipe data, parsing input | [02-syntax-dan-tipe-data.md](./02-syntax-dan-tipe-data.md) |
| 03 | Control flow (`if`, `switch`, `for`, `range`) | [03-control-flow.md](./03-control-flow.md) |
| 04 | Slice & map untuk manipulasi koleksi data | [04-slices-maps-deep-dive.md](./04-slices-maps-deep-dive.md) |
| 05 | Pointer, struct, method, dan mutasi state | [05-pointer-struct-method-dasar.md](./05-pointer-struct-method-dasar.md) |
| 06 | Error handling (`error`, wrapping, panic/recover seperlunya) | [06-error-handling-dasar.md](./06-error-handling-dasar.md) |
| 07 | Interface kecil dan composition | [07-interface-dasar-composition.md](./07-interface-dasar-composition.md) |
| 08 | Fondasi low-level: stack/heap, escape, byte vs rune, race dasar | [08-low-level-go-dasar.md](./08-low-level-go-dasar.md) |
| 09 | Referensi resmi + penguatan mandiri lintas topik | [99-referensi-fundamental.md](./99-referensi-fundamental.md) |

## Latihan bertingkat

### Level Basic (wajib)
| ID | Topik | Latihan | Kriteria selesai (terukur) |
|---|---|---|---|
| B01 | 01 | Buat CLI `hello <nama>` dengan `os.Args`. | `go run . hello Budi` menampilkan `Halo, Budi!` tanpa panic. |
| B02 | 02 | Buat command `calc <a> <op> <b>` (`+,-,*,/`). | Operasi benar untuk 5 kasus uji (termasuk pembagian nol ditangani error). |
| B03 | 03 | Buat command `grade <nilai>` dengan `if/switch`. | Nilai 0-100 dipetakan ke grade konsisten dan input invalid ditolak. |
| B04 | 04 | Buat command `count-word <kalimat>` pakai map. | Output berisi frekuensi kata yang benar untuk minimal 3 kalimat uji. |
| B05 | 05 | Definisikan `struct Task` + method `MarkDone()`. | Status task berubah hanya lewat method, bukan ubah field acak di `main`. |
| B06 | 06 | Tambahkan validasi ID/judul dengan error wrapping (`%w`). | Error user-friendly muncul untuk 3 input invalid berbeda. |
| B07 | 07 | Buat interface `Storage` (`Save`, `Load`) + implementasi memory. | Fitur simpan/muat data jalan lewat interface, bukan tipe konkret langsung. |
| B08 | 08 | Validasi panjang judul berdasarkan rune (`utf8.RuneCountInString`). | Input Unicode (contoh `Gö`, `日本語`) dihitung benar, bukan berdasarkan byte. |
| B09 | 09 | Baca 2 referensi resmi dari topik 09 lalu terapkan 1 perbaikan kode. | Ada 1 perubahan nyata (mis. validasi/error/struktur) dan dicatat alasannya. |

### Level Menengah (disarankan kuat)
| ID | Topik | Latihan | Kriteria selesai (terukur) |
|---|---|---|---|
| M01 | 03,05,06,09 | Bangun parser command yang rapi (`add`, `list`, `done`, `delete`, `help`). | Semua command wajib berjalan dan command tidak dikenal menampilkan bantuan. |
| M02 | 04,05,06,07 | Tambahkan penyimpanan file `tasks.json` selain memory storage. | Data tetap ada setelah program ditutup dan dibuka lagi. |
| M03 | 07,08 | Simulasikan operasi paralel sederhana (add/list) lalu cek dengan `-race`. | Tidak ada laporan race pada skenario uji yang kamu jalankan. |
| M04 | 02,06,09 | Rapikan pesan error per kategori (`input`, `io`, `internal`). | Minimal 3 kategori error tampil konsisten di CLI. |

### Level Challenge (opsional, tapi sangat bagus)
| ID | Topik | Latihan | Kriteria selesai (terukur) |
|---|---|---|---|
| C01 | 01-09 | Tambahkan flag `--status`, `--limit`, `--sort` di command `list`. | Output terfilter/sorted sesuai flag untuk minimal 5 skenario. |
| C02 | 05,07,09 | Buat dua backend storage (`memory`, `file`) yang bisa dipilih saat runtime. | Pemilihan backend tidak mengubah logic bisnis inti. |
| C03 | 06,08,09 | Tambahkan mode debug (`--debug`) untuk log error detail. | User normal tetap dapat pesan singkat, mode debug tampilkan detail root cause. |

## Mini-project fundamental: CLI Task Tracker

### Tujuan
Membangun CLI sederhana untuk manajemen task harian sebagai bukti penguasaan materi fundamental.

### Requirement fungsional (wajib)
1. `add <judul>`: menambah task baru.
2. `list`: menampilkan semua task dengan format `ID | Status | Judul`.
3. `done <id>`: menandai task selesai.
4. `delete <id>`: menghapus task.
5. `help`: menampilkan panduan command.
6. Data tersimpan di `tasks.json` agar tetap ada setelah restart aplikasi.

### Requirement teknis (wajib)
- Gunakan **Go standard library** (tanpa dependency pihak ketiga).
- Gunakan `struct` + method receiver untuk entitas task.
- Gunakan interface kecil (`Storage`) untuk abstraksi penyimpanan.
- Tangani error dengan jelas (hindari `panic` untuk alur normal).
- Validasi judul task: minimum 3 rune, maksimum 60 rune.
- Kode tetap terbaca: fungsi tidak terlalu panjang dan tanggung jawab jelas.

### Skenario uji manual minimum
Jalankan dan pastikan hasil sesuai:
1. `go run . help`
2. `go run . add "Belajar pointer"`
3. `go run . add "Membaca referensi resmi"`
4. `go run . list`
5. `go run . done 1`
6. `go run . list`
7. `go run . delete 2`
8. Tutup aplikasi, jalankan lagi `go run . list` (cek persistensi data)

## Rubrik dan checklist kelulusan

### Rubrik penilaian (100 poin)
| Area | Bobot | Indikator |
|---|---:|---|
| Fitur wajib mini-project | 35 | Semua command inti berjalan sesuai requirement. |
| Penerapan konsep topik 01-09 | 20 | Konsep dipakai tepat (control flow, map/slice, struct/method, interface, referensi resmi). |
| Error handling dan validasi input | 20 | Error informatif, tidak panic untuk kesalahan user. |
| Kualitas struktur kode | 15 | Fungsi/method jelas, coupling rendah, naming konsisten. |
| Kualitas pengalaman CLI | 10 | Output mudah dibaca, command help jelas, perilaku konsisten. |

**Batas lulus:** total skor **≥ 80** dan semua item kritikal di bawah tercentang.

### Checklist kritikal (harus semua ✅)
- [ ] `add`, `list`, `done`, `delete`, `help` berjalan sesuai requirement.
- [ ] Input invalid (ID non-angka, judul kosong, command tidak dikenal) tidak membuat panic.
- [ ] Data tersimpan dan bisa dimuat ulang dari `tasks.json`.
- [ ] Validasi panjang judul memakai rune, bukan byte.
- [ ] Error utama menampilkan konteks yang jelas untuk user.
- [ ] Minimal satu skenario dijalankan dengan race detector (`go run -race . list` atau setara) tanpa race.
- [ ] Ada catatan singkat hasil belajar (apa yang sulit, apa yang diperbaiki).

### Checklist tambahan (untuk nilai lebih)
- [ ] Sudah menyelesaikan minimal 2 latihan level Menengah.
- [ ] Sudah menyelesaikan minimal 1 latihan level Challenge.
- [ ] Sudah menerapkan perbaikan dari referensi topik 09 secara sadar.

## Common pitfalls saat mengerjakan latihan
1. **Langsung lompat challenge** sebelum basic stabil → hasilnya sering rapuh.
2. **Semua logic ditaruh di `main`** → sulit debug dan sulit dikembangkan.
3. **Memakai `panic` untuk error input user** → pengalaman CLI buruk.
4. **Menganggap panjang string = byte** → validasi judul Unicode sering salah.
5. **Tidak cek race saat mulai pakai goroutine** → bug acak sulit direproduksi.
6. **Tidak membaca referensi resmi** → solusi jadi “kebetulan jalan”, bukan paham.

## Rekomendasi langkah lanjut ke intermediate
1. Pastikan mini-project lulus rubrik + checklist.
2. Refactor ringan: rapikan struktur package agar siap scale.
3. Lanjut ke **[02-intermediate/01-modules-packages.md](../02-intermediate/01-modules-packages.md)**.
4. Di level intermediate, fokuskan upgrade pada:
   - organisasi package/modul,
   - testing otomatis,
   - file I/O dan JSON lebih sistematis.

## Referensi resmi + lanjutan
- **[Command `go`](https://pkg.go.dev/cmd/go)** — referensi resmi semua subcommand CLI Go.
  _Kapan baca:_ saat bingung perintah build/test/run yang paling tepat untuk latihanmu.
- **[Tutorial: Create a module](https://go.dev/doc/tutorial/create-module)** — pola project Go yang rapi sejak awal.
  _Kapan baca:_ saat mini-project mulai membesar dan butuh struktur lebih jelas.
- **[Package `testing`](https://pkg.go.dev/testing)** — fondasi menulis tes otomatis.
  _Kapan baca:_ setelah skenario uji manual stabil dan ingin naik kualitas verifikasi.
- **[Data Race Detector](https://go.dev/doc/articles/race_detector)** — panduan resmi mendeteksi data race.
  _Kapan baca:_ saat kamu mulai menambah goroutine atau shared state.
- **[Effective Go](https://go.dev/doc/effective_go)** — praktik idiomatik agar kode tetap bersih dan mudah dirawat.
  _Kapan baca:_ saat refactor mini-project sebelum naik ke intermediate.
- **[Indeks referensi fundamental](./99-referensi-fundamental.md)** — kumpulan link resmi topik 01-09.
  _Kapan baca:_ saat butuh pendalaman cepat per topik tanpa cari ulang link.

## Navigasi fundamental
- [Kembali ke Start Here](./00-start-here.md)
- **Sebelumnya:** [99-referensi-fundamental.md](./99-referensi-fundamental.md)
- **Selanjutnya (setelah lulus checklist):** [02-intermediate/01-modules-packages.md](../02-intermediate/01-modules-packages.md)
