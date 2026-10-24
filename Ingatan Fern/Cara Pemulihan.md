# Cara Memulihkan Ingatan dan Karakter Fern

Folder ini menyimpan salinan asli identitas inti, profil Rama, dan memori teknis jangka panjang asisten Fern.

Jika terjadi masalah pada server atau ingin memulihkan Fern di mesin / VPS baru yang sudah terinstall Hermes Agent, cukup salin file-file ini ke direktori konfigurasi Hermes.

## 1. File Inti
- `SOUL.md`: Kepribadian, karakter kuudere/pragmatis, aturan gaya chat tanpa em-dash, dan protokol kerja Fern.
- `USER.md`: Profil Rama, preferensi personal, proyek aktif, dan kebiasaan.
- `MEMORY.md`: Pengetahuan teknis permanen (arsitektur VPS, ports, database Supabase, dsb).

## 2. Perintah Restore Cepat di VPS Baru
Jalankan perintah ini di terminal server baru:
```bash
# Pastikan folder memories ada
mkdir -p ~/.hermes/memories

# Salin ingatan dan jiwa Fern
cp "Ingatan Fern/SOUL.md" ~/.hermes/SOUL.md
cp "Ingatan Fern/USER.md" ~/.hermes/memories/USER.md
cp "Ingatan Fern/MEMORY.md" ~/.hermes/memories/MEMORY.md
```

Setelah file disalin, restart Hermes atau jalankan session baru. Fern akan langsung mengenali Rama dan melanjutkan pekerjaannya seperti biasa.

Panduan khusus perangkat hemat daya (STB Armbian) dapat dilihat di: [[Panduan Migrasi dan Pemulihan Fern di STB Armbian]].
