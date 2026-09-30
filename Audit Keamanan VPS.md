# Audit Keamanan VPS

Tanggal Audit: 2026-09-29 (Diperbarui: 2026-09-30)
Auditor: [[Fern]]
Target: VPS Ubuntu Linux

## Ringkasan Temuan & Status Saat Ini
Status keamanan server saat ini **sudah jauh lebih aman (Terproteksi)** setelah dilakukan tindakan hardening.

## 1. Konfigurasi Keamanan yang Sudah Diterapkan (Aman)
- **Firewall Sistem (`firewalld`):**
  - Service `firewalld` aktif dan berjalan (`active (running)`).
  - Port yang terbuka dibatasi secara ketat hanya pada port `2222/tcp` (SSH) dan `20128/tcp` (9Router Dashboard).
- **Proteksi SSH & Anti Brute-Force:**
  - Port SSH diubah dari port standar 22 ke port `2222/tcp`.
  - `PermitRootLogin no` sudah aktif (login langsung sebagai root ditolak).
  - `sshguard` dipasang dan aktif memantau log `sshd` untuk memblokir IP penyerang otomatis via `nftables`.
- **Hermes Dashboard (Port 9119):**
  - Listen pada `127.0.0.1` dan hanya dapat diakses melalui Cloudflare Tunnel terproteksi (`hermes.herama.my.id`).
- **Autentikasi Git & Obsidian:**
  - SSH Deploy Key (`id_ed25519_obsidian`) berhasil dipasang untuk sinkronisasi otomatis vault Obsidian ke repository GitHub `Ramadani-coding/hermes-obsidian`.

## 2. Catatan & Rekomendasi Lanjutan
1. Pertimbangkan mematikan `PasswordAuthentication no` pada SSH jika sudah sepenuhnya beralih ke SSH Key auth.
2. Pastikan port `20128` (9Router) hanya diakses untuk kebutuhan pengelolaan router.

Tautan Terkait:
- [[Proyek]]
- [[Pengetahuan]]
- [[Home]]