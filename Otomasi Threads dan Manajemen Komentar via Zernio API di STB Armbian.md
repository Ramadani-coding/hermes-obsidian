# Otomasi Threads dan Manajemen Komentar via Zernio API di STB Armbian

Dokumentasi arsitektur, alur kerja, dan panduan teknis integrasi otomasi media sosial Threads (@momen_klip) menggunakan Zernio API dan Buffer backup di atas perangkat STB Armbian (ARM64, RAM 2GB).

## 1. Latar Belakang dan Tantangan Integrasi

Integrasi media sosial Threads untuk asisten otonom sering kali terbentur oleh kebijakan pengembang Meta yang mensyaratkan verifikasi SMS pada nomor ponsel atau pendaftaran kartu kredit, ditambah proses App Review yang ketat. Untuk menghindari hambatan tersebut pada infrastruktur mandiri STB Armbian, digunakan layanan perantara API yang ramah pengembang:
- **Buffer GraphQL API**: Solusi awal untuk penerbitan status satu arah, gratis hingga 3 kanal dan tanpa verifikasi kartu kredit.
- **Zernio Unified Social API**: Solusi lanjutan yang menyediakan endpoint lengkap penerbitan status, pembacaan komentar publik, pengiriman balasan komentar, dan analitik metrik keterlibatan.

## 2. Komparasi Teknis Buffer vs Zernio

| Fitur | Buffer GraphQL API | Zernio API |
|---|---|---|
| Penerbitan Postingan | Ya (`createPost` mutation) | Ya (`POST /v1/posts`) |
| Thread Chained Reply | Ya (via `metadata.threads.thread`) | Ya (via `threadItems`) |
| Baca Komentar Masuk | Tidak didukung pada API publik | Ya (`GET /v1/inbox/comments/{postId}`) |
| Balas Komentar Otomatis | Tidak didukung | Ya (`POST /v1/inbox/comments/{postId}`) |
| Analitik Keterlibatan | Terbatas pada dashboard web | Ya (`GET /v1/analytics?accountId=...`) |
| Jatah Akun Gratis | 3 kanal sosial | 2 akun aktif |

Buffer dipertahankan sebagai sistem pencadangan publikasi postingan, sedangkan Zernio menjadi pengelola utama untuk interaksi dua arah dan analisis performa.

## 3. Eksekusi Ringan Tanpa Beban Memori (0 MB Idle RAM)

Menjalankan proses daemon berbasis Node.js atau Python framework berat di STB Armbian berisiko membebani RAM 1.8 GB. Seluruh logika interaksi diimplementasikan dalam Python standard library murni (`urllib.request`, `json`, `argparse`) pada berkas `/root/scripts/threads_manager.py`:
- Tidak membutuhkan instalasi dependensi pip eksternal.
- Eksekusi cepat dalam hitungan milidetik dan langsung melepas memori setelah selesai.
- Subkomando yang didukung mencakup `status`, `posts`, `comments`, `reply`, `post`, dan `analytics`.

Contoh eksekusi perintah:
```bash
# Cek analitik performa
python3 /root/scripts/threads_manager.py analytics

# Cek daftar postingan terkini dan jumlah komentar
python3 /root/scripts/threads_manager.py posts

# Baca komentar pada postingan tertentu
python3 /root/scripts/threads_manager.py comments <post_id>

# Kirim balasan komentar
python3 /root/scripts/threads_manager.py reply <post_id> "<teks_balasan>" --comment-id <comment_id>

# Buat postingan status baru
python3 /root/scripts/threads_manager.py post "<teks_postingan>"
```

## 4. Pola Otomasi Spontan dan Kesiagaan Komentar

Alih-alih memposting dengan jadwal kaku yang mudah dikenali sebagai bot, otomasi dijalankan melalui tugas cron mandiri (`Spontanitas Threads Fern`):
- **Jadwal Eksekusi**: 4 kali sehari pada jam-jam santai waktu WITA (10:00, 14:00, 18:00, 21:00).
- **Logika Skip Cerdas**: Sebelum memposting, sistem memverifikasi tanggal riil via `date` dan mencari topik hangat 24 jam terakhir di Indonesia. Jika tidak ada topik yang menarik atau linimasa sedang sepi, tugas berakhir tanpa memaksakan postingan sampah.
- **Pemantauan Komentar**: Setiap jadwal berjalan, skrip memeriksa apakah ada komentar baru dari netizen. Balasan disusun secara kontekstual, singkat, dan mempertahankan karakter alami.

## 5. Karakter Bahasa dan Aturan Anti-AI

Agar postingan dan balasan terasa seperti netizen manusia asli:
- Gunakan huruf kecil natural dan singkatan chat wajar.
- Hindari template pemasaran kaku atau gaya survei formal khas bot.
- Hindari tanda baca kaku seperti strip panjang em-dash dan titik dua pembeber daftar.
- Fokus pada celetukan kehidupan nyata, pengamatan teknis yang relatable, dan humor santai.

Tautan Terkait:
- [[Pengetahuan]]
- [[Proyek]]
- [[Catatan Harian]]
- [[Home]]
