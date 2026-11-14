# Mod Bahasa Indonesia Thief Simulator (Unity StreamingAssets SoLocalization)

Modifikasi teks antarmuka, dialog, misi, tutorial, dan barang di game *Thief Simulator* (PC Steam) ke Bahasa Indonesia lengkap (1.664 baris teks) tanpa merusak biner game.

## Arsitektur Lokalisasi Unity SoLocalization

Berbeda dengan game Unreal Engine yang membungkus teks dalam arsip `.pak` dan biner `.locres`, game *Thief Simulator* (Unity Engine) menyimpan seluruh string multibahasa secara terbuka di direktori:

```
thief_Data\StreamingAssets\SoLocalization\
```

Struktur file:
- Setiap baris berformat CSV dengan pemisah koma: `GUID,name,URL_ENCODED_TEXT,comment`
- Bagian teks dikodekan menggunakan URL percent-encoding (spasi dikonversi menjadi `%20`).
- Tidak menggunakan penanda placeholder rumit sehingga sangat aman diterjemahkan.

## Alur Penerjemahan & Kompilasi

1. **Ekstraksi & Parsing String**:
   - Membedah 1.664 baris teks bahasa Spanyol (`es`) via skrip Python `/root/projects/thief_id/translator.py`.
   - Menggunakan `urllib.parse.unquote` untuk mengekstrak teks mentah.

2. **Penerjemahan Multithreaded via 9Router**:
   - Memanfaatkan 5 akun Antigravity OAuth melalui model `ag/gemini-3.8-flash` dengan `ThreadPoolExecutor(max_workers=5)`.
   - Batching 50 kalimat per request dengan checkpointing `cache.json` untuk ketahanan proses.

3. **Injeksi & Packaging**:
   - Mengonversi teks terjemahan kembali ke URL percent-encoding via `urllib.parse.quote(safe='')`.
   - Menghasilkan file `es` (untuk menggantikan bahasa Spanyol di menu opsi) dan `id`.
   - Paket siap unduh tersedia di `https://nara.herama.my.id/Thief_Simulator_Mod_Indo.zip`.

## Cara Pemasangan

1. Unduh paket zip mod.
2. Salin file `es` ke folder `thief_Data\StreamingAssets\SoLocalization\`.
3. Buka game, pilih bahasa **Español** di menu Settings, seluruh teks langsung berbahasa Indonesia.
