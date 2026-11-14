# Riset Loker Harian dan Otomasi Draft Lamaran

Sistem pemantauan dan pembuatan draf lamaran kerja otomatis berbasis jadwal (cronjob) untuk Ahmad Ramadani di STB Armbian.

## Jadwal & Aturan Eksekusi

- **Jadwal**: Setiap hari pukul `08:00 WITA` (cron: `0 8 * * *`, timezone: `Asia/Makassar`).
- **Target Wilayah**: Khusus Kota Banjarbaru dan Kota Banjarmasin. Lowongan di luar wilayah ini diabaikan.
- **Kategori Posisi Relevan**:
  - Teknisi IT / Komputer / Jaringan / Hardware
  - Admin Data / Kantor / Operasional / Produksi
  - Store Crew / Frontliner Toko Gadget & Retail
  - Sales Promotor / Edukasi Produk
- **Standar Keamanan & Anti-Penipuan**:
  - Memverifikasi keberadaan lokasi fisik toko/kantor yang riil di Google Maps.
  - Memastikan tidak ada pungutan biaya, calo tiket, atau indikasi penipuan.
  - **Wajib Ada Email Resmi**: Lowongan yang hanya menyediakan nomor WhatsApp atau mewajibkan walk-in tanpa jalur email otomatis dilewati (*skip*).

## Otomasi Alur Kerja

1. **Riset & Deduplikasi**: Mencari lowongan baru yang belum pernah dilamar pada hari-hari sebelumnya.
2. **Injeksi Draf Gmail**: Jika ditemukan lowongan valid, sistem otomatis membuat draf surat lamaran profesional via `/root/scripts/email_manager.py draft` lengkap dengan lampiran canonical CV (`/root/documents/cv/CV_Ahmad_Ramadani.pdf`).
3. **Notifikasi Telegram**: Mengirimkan rangkuman lowongan yang ditemukan langsung ke chat Telegram Rama, mengonfirmasi bahwa draf sudah siap diperiksa dan dikirim.
