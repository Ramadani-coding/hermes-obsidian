# Setup Dashboard Hermes dan Cloudflare Tunnel

Panduan teknis konfigurasi Web Dashboard Hermes agar bisa diakses publik secara aman via Cloudflare Tunnel.

## 1. Topologi & Arsitektur
- Service Lokal: Hermes Dashboard berjalan di 127.0.0.1:9119.
- Reverse Proxy: Cloudflare Tunnel (cloudflared systemd service).
- Domain Publik: https://hermes.herama.my.id.
- Autentikasi: dashboard.basic_auth (username dan scrypt password hash).

## 2. Masalah yang Terjadi dan Solusinya
- DNS Rebinding & Host Mismatch:
  Hermes menolak request dari domain publik jika dashboard.public_url belum dideklarasikan.
  Solusi: Daftarkan domain publik dengan perintah hermes config set dashboard.public_url "https://hermes.herama.my.id".
- WebSocket Origin Mismatch (Terminal Disconnect):
  Terminal web (/api/pty) menolak koneksi WebSocket jika header Origin dari browser tidak cocok dengan host yang diizinkan.
  Solusi: Mendaftarkan dashboard.public_url otomatis memasukkan domain ke daftar trusted_public_hosts sehingga koneksi WebSocket diizinkan resmi.
- Kewajiban Autentikasi Publik:
  Membuka dashboard ke publik mewajibkan adanya kunci keamanan. Digunakan modul bawaan dashboard.basic_auth dengan username dan password terenkripsi hash scrypt.

## 3. Layanan Terkait
- Dashboard Hermes: Dijalankan dengan hermes dashboard --no-open.
- Cloudflare Service: Dikelola otomatis oleh systemd (cloudflared.service).

Tautan Terkait:
- [[Pengetahuan]]
- [[Home]]
