# Audit Keamanan VPS

Tanggal Audit: 2026-09-29 (Diperbarui: 2026-10-02)
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

## 2. Verifikasi Operasional SSHGuard & Log Audit (2026-10-02)
- **Pemantauan Real-Time Serangan:**
  - Terdeteksi 499 catatan aktivitas serangan dari 161 IP scanner unik yang mencoba brute-force port SSH `2222`.
  - SSHGuard berhasil memitigasi seluruh serangan secara otomatis dengan mengeksekusi 18 kali tindakan pemblokiran bertingkat via tabel `nftables`.
- **Daftar IP Agresif Terblacklist:**
  - 7 IP unik berhasil dijatuhi sanksi blokir: `104.64.202.190`, `115.190.159.225`, `146.190.99.29`, `147.139.167.207`, `195.178.110.218`, `39.109.116.214`, dan `77.90.185.20`.
  - IP paling agresif (`146.190.99.29`) dikenakan eskalasi durasi blokir hingga 16 menit bertingkat.
- **Kesimpulan Proteksi:** Kombinasi port non-standar `2222`, pemblokiran dinamis SSHGuard, serta masking origin IP lewat Cloudflare Tunnel terbukti sangat efektif menahan penetrasi otomatis.

## 3. Catatan & Rekomendasi Lanjutan
1. Pertimbangkan mematikan `PasswordAuthentication no` pada SSH jika sudah sepenuhnya beralih ke SSH Key auth.
2. Pastikan port `20128` (9Router) hanya diakses untuk kebutuhan pengelolaan router.

Tautan Terkait:
- [[Proyek]]
- [[Pengetahuan]]
- [[Home]]