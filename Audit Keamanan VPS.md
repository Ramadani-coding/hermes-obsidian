# Audit Keamanan VPS

Tanggal Audit: 2026-09-29
Auditor: [[Fern]]
Target: VPS Ubuntu Linux

## Ringkasan Temuan
Status keamanan server saat ini tergolong **rawan** dan butuh perbaikan segera.

## 1. Titik Lemah Kritis (High Risk)
- **Firewall Lokal (UFW) Nonaktif:**
  Status UFW `inactive`. Semua port yang terbuka pada `0.0.0.0` terekspos langsung ke jaringan luar.
- **SSH Password & Root Login Terbuka:**
  - `PermitRootLogin yes`
  - `PasswordAuthentication yes`
  - Dalam 2 jam terakhir tercatat 77+ percobaan brute force password yang gagal dari IP asing.
  - Belum terpasang proteksi anti brute force seperti `fail2ban`.
- **Port LLM Service (20128) Terbuka ke Publik:**
  Service model local (`next-server` nara-combo) listen di `0.0.0.0:20128`, bukan `127.0.0.1`.

## 2. Aspek yang Sudah Aman (Good)
- **Hermes Dashboard (Port 9119):**
  Listen pada `127.0.0.1` dan hanya dapat diakses melalui Cloudflare Tunnel terproteksi.
- **Pembaruan Sistem:**
  Tidak ada pending patch security kritis di apt saat ini.

## Rekomendasi Tindakan
1. Pasang dan aktifkan `fail2ban` untuk memblokir IP penyerang otomatis.
2. Batasi listen port 20128 ke `127.0.0.1` saja bila hanya dipakai lokal oleh Hermes.
3. Matikan login SSH root (`PermitRootLogin no`) dan pertimbangkan beralih ke SSH Key auth saja.
4. Aktifkan UFW dengan membuka port yang benar-benar esensial (misal port 22).

Tautan Terkait:
- [[Pengetahuan]]
- [[Home]]
