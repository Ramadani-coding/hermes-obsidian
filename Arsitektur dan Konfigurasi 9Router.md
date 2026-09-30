# Arsitektur dan Konfigurasi 9Router

## Konsep Dasar
9Router adalah proxy LLM yang memiliki dua lapisan abstraksi utama dalam me-routing request, yaitu tingkat penyedia (akun) dan tingkat model (combo).

### 1. Rotasi Antar-Akun (Multi-Account Load Balancing)
Jika Anda mendaftarkan beberapa akun ke dalam satu penyedia (misal: 3 akun Google OAuth yang berbeda di bawah penyedia *Antigravity*), 9Router secara otomatis akan menerapkan strategi **Round-Robin di balik layar**. Ini berarti setiap request API akan digilir ke akun-akun tersebut agar beban (dan batas kuota/limit) tersebar merata, tanpa campur tangan pengguna di konfigurasi *combo*.

### 2. Rotasi Antar-Model (Strategi Combo)
Pada manajemen Combo, Anda menentukan himpunan model yang digunakan.
- **Mode Round Robin:** Menggilir penggunaan model pada setiap request. Jika combo diisi dengan model yang memiliki tingkat kecerdasan berbeda (contoh: mencampur Opus, Sonnet, dan Flash), kualitas respons asisten AI akan tidak konsisten dan melompat-lompat setiap kali merespons pesan.
- **Mode Fallback (Direkomendasikan):** Menggunakan model sesuai urutan prioritas dari atas ke bawah. Model terbaik (contoh: `claude-sonnet-4-6`) diletakkan di urutan pertama, sedangkan model yang lebih cepat/ringan diletakkan di bawahnya sebagai cadangan (darurat) apabila model utama terkena limit.

### Kesimpulan Rekomendasi
Gunakan **Mode Fallback** pada pengaturan grup model (combo). 9Router tetap akan melakukan load-balancing antar akun Google di balik layar secara otomatis, tetapi kualitas dan karakter obrolan tetap konsisten stabil di tingkat tertinggi.

Kembali ke: [[Pengetahuan]]