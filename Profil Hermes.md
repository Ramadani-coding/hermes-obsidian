# Profil di Hermes Agent

Penjelasan mengenai konsep profil (multi-instance/environment) di Hermes.

## Apa itu Profil?
Profil di Hermes adalah fitur untuk membuat lingkungan kerja (instance) yang terisolasi di dalam satu server/instalasi yang sama. Mirip seperti fitur multi-user atau multi-profile di browser Chrome.

Secara bawaan, Hermes menggunakan profil `default` (tersimpan di `~/.hermes/`). Saat membuat profil baru, datanya akan disimpan di `~/.hermes/profiles/<nama-profil>/`.

## Komponen yang Terisolasi per Profil
Setiap profil memiliki data independen yang tidak saling tercampur:
1. **Konfigurasi & Kredensial:** Berkas `config.yaml` dan `.env` terpisah.
2. **Identitas & Memori:** Berkas `SOUL.md` (persona), `MEMORY.md`, dan `USER.md` sendiri.
3. **Skills & Plugins:** Kumpulan skill dan ekstensi yang aktif bisa berbeda.
4. **Riwayat Percakapan:** Database sesi (`state.db`) dan token auth terpisah.
5. **Jadwal Otomatisasi:** Cron job dan background task masing-masing.

## Contoh Penggunaan
- Memisahkan asisten kerjaan (misal bot urusan promotor/kantor) dengan asisten personal (Fern).
- Menguji model AI, persona baru, atau plugin tanpa merusak setelan utama yang sudah stabil.

## Perbedaan Profil vs Sub-Agent
Banyak yang mengira keduanya sama, padahal tujuannya berbeda:
- **Profil:** Lingkungan permanen yang benar-benar terpisah (ibarat beda user akun di Windows atau beda profile di Chrome). Konfigurasi, ingatan, kredensial, dan kepribadiannya beda sama sekali.
- **Sub-Agent:** Pekerja lepas sementara yang dipanggil oleh agen utama (`delegate_task`) untuk menyelesaikan tugas tertentu secara paralel di latar belakang. Sub-agent berjalan di dalam profil yang sama, bekerja beberapa menit, mengembalikan hasil analisis ke agen utama, lalu otomatis nonaktif.

## Perintah Dasar CLI
- Melihat daftar profil: `hermes profile list`
- Membuat profil baru: `hermes profile create <nama>`
- Menjalankan Hermes di profil tertentu: `hermes -p <nama>`

Tautan Terkait:
- [[Pengetahuan]]
- [[Home]]
