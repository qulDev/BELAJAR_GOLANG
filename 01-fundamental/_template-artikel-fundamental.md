# Template Artikel Fundamental (Reusable)

Gunakan file ini sebagai kerangka standar saat menulis/menulis ulang materi fundamental agar konsisten, praktis, dan bertahap.

## Panduan penulisan singkat
- **Tone:** ramah pemula, jelas, tidak menggurui, fokus membantu pembaca “paham + bisa praktik”.
- **Style:** format blog teknis; jelaskan dari intuisi dulu, lalu detail teknis; hindari paragraf panjang.
- **Depth:** tetap beginner-friendly, tapi cukup dalam untuk progres teknis (jelaskan *apa, kenapa, kapan dipakai, jebakan*).
- **Konvensi format:** gunakan heading terstruktur, bullet list ringkas, kode runnable, dan penamaan section konsisten dengan template ini.

---

## Judul
# [Nama Topik Fundamental yang Spesifik]

Pembuka 2–4 kalimat:
- masalah apa yang sering dialami pemula,
- kenapa topik ini penting,
- apa hasil praktis setelah selesai membaca.

---

## Untuk siapa materi ini
- [Pemula total yang baru mulai Go]
- [Pemula yang sudah lihat sintaks tapi belum paham konsep]
- [Pembaca yang ingin transisi dari “ikut tutorial” ke “bisa reasoning sendiri”]

---

## Prasyarat
- [Contoh: Go sudah terpasang]
- [Contoh: paham dasar command line]
- [Contoh: sudah membaca materi sebelumnya: `xx-nama-file.md`]

> Jika prasyarat belum terpenuhi, beri tautan langsung ke materi prasyarat.

---

## Tujuan belajar
Setelah menyelesaikan artikel ini, pembaca diharapkan bisa:
- [Tujuan terukur 1]
- [Tujuan terukur 2]
- [Tujuan terukur 3]

---

## Mental model (dengan instruksi menyisipkan ilustrasi)
Jelaskan konsep dengan analogi sederhana dulu, lalu kaitkan ke implementasi teknis.

> **Instruksi ilustrasi:** sisipkan 1 ilustrasi/diagram sederhana di section ini (boleh ASCII, Mermaid, atau gambar statis) yang menunjukkan alur inti konsep.  
> Contoh format: “input -> proses -> output”, “memory lama vs memory baru”, atau “request -> handler -> response”.

Tambahkan 3 poin:
1. [Analogi non-teknis]
2. [Pemetaan analogi ke istilah teknis]
3. [Batas analogi: bagian mana yang tidak sepenuhnya sama]

---

## Konsep inti (step-by-step)
### Step 1 — [Konsep paling dasar]
- **Apa:** [...]
- **Kenapa penting:** [...]
- **Kapan dipakai:** [...]
- **Catatan jebakan:** [...]

### Step 2 — [Konsep lanjutan yang masih terkait]
- **Apa:** [...]
- **Kenapa penting:** [...]
- **Kapan dipakai:** [...]
- **Catatan jebakan:** [...]

### Step 3 — [Hubungkan ke kasus nyata]
- **Apa:** [...]
- **Kenapa penting:** [...]
- **Kapan dipakai:** [...]
- **Catatan jebakan:** [...]

---

## Contoh kode bertahap
### Tahap 1 — Versi minimum (paling sederhana)
```go
// tulis contoh runnable minimum
```
Penjelasan singkat output/perilaku:
- [...]

### Tahap 2 — Tambah 1 kebutuhan nyata
```go
// perluas contoh tahap 1
```
Perubahan dibanding tahap 1:
- [...]

### Tahap 3 — Versi lebih idiomatik/siap dipakai
```go
// versi final yang lebih production-minded untuk level fundamental
```
Checklist pemahaman:
- [ ] Saya paham kenapa versi ini lebih aman/rapi
- [ ] Saya bisa modifikasi contoh untuk kasus sendiri

---

## Common mistakes (minimal 2)
### Mistake 1 — [Nama kesalahan]
- **Gejala:** [...]
- **Penyebab:** [...]
- **Perbaikan:** [...]

### Mistake 2 — [Nama kesalahan]
- **Gejala:** [...]
- **Penyebab:** [...]
- **Perbaikan:** [...]

> Opsional: tambahkan Mistake 3 jika topik rawan disalahpahami.

---

## Latihan (basic/menengah/challenge)
### Basic
- [Latihan kecil untuk memastikan konsep dasar]
- [Expected output / kriteria lulus]

### Menengah
- [Latihan modifikasi dari contoh utama]
- [Constraint sederhana agar pembaca berpikir]

### Challenge
- [Mini problem yang menggabungkan beberapa konsep]
- [Hint tanpa spoiler penuh]

---

## Ringkasan
- [Inti konsep 1]
- [Inti konsep 2]
- [Inti konsep 3]
- [Next action: apa yang sebaiknya langsung dicoba pembaca]

---

## Referensi resmi + lanjutan
- Dokumentasi resmi: [link resmi 1]
- Dokumentasi resmi: [link resmi 2]
- Bacaan lanjutan: [artikel/video/repo latihan]

> Prioritaskan referensi resmi dulu, lalu sumber lanjutan berkualitas.

---

## Navigasi (sebelumnya/selanjutnya/start-here)
- [Kembali ke Start Here](./00-start-here.md)
- **Sebelumnya:** [xx-materi-sebelumnya.md](./xx-materi-sebelumnya.md)
- **Selanjutnya:** [xx-materi-selanjutnya.md](./xx-materi-selanjutnya.md)
