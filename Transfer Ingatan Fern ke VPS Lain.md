# Cara Transfer Ingatan Fern ke Hermes Bot di VPS Lain

Panduan singkat untuk memindahkan identitas dan ingatan asisten Fern dari satu VPS ke VPS lainnya (sama-sama menggunakan Hermes).

## File yang Perlu Disalin

| File/Folder | Isi | Jalur |
|---|---|---|
| `SOUL.md` | Kepribadian Fern, aturan chat kasual, standar komunikasi | `~/.hermes/SOUL.md` |
| `memories/USER.md` | Profil pengguna Rama (nama, profesi, gaya komunikasi) | `~/.hermes/memories/USER.md` |
| `memories/MEMORY.md` | Catatan teknis permanen (infra VPS, config kunci) | `~/.hermes/memories/MEMORY.md` |

## Cara Pemindahan Cepat (Arsip TAR)

Jalankan perintah ini di VPS asal untuk membuat arsip:
```bash
tar -czvf ~/hermes_memory_export.tar.gz -C ~/.hermes SOUL.md memories/
```

Arsip berukuran ~3 KB ini berisi seluruh identitas dan ingatan Fern. Upload ke VPS tujuan, lalu ekstrak:
```bash
tar -xzvf hermes_memory_export.tar.gz -C ~/.hermes/
```

Setelah ini, bot Hermes di VPS tujuan langsung memiliki karakter dan ingatan yang sama persis.

## Catatan Tambahan
- Dokumentasi teknis yang lebih panjang tersedia di repositori GitHub Obsidian: [hermes-obsidian](https://github.com/Ramadani-coding/hermes-obsidian).
- Arsip ini tidak mencakup token bot Telegram (harus dikonfigurasi ulang di VPS tujuan via `.env` atau `config.yaml`).
- Skills perlu diinstall ulang di VPS baru menggunakan `hermes skills install`.

Kembali ke: [[Pengetahuan]]