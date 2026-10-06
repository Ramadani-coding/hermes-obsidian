# Audit Keamanan VPS

Tanggal Audit: 2026-09-29 (Diperbarui: 2026-10-06)
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

## 3. Verifikasi Operasional SSHGuard & Log Audit Lanjutan (2026-10-06)
- **Telemetri Serangan Kumulatif:**
  - Terdeteksi total 2.011 aktivitas serangan brute-force dari 566 IP penyerang unik yang mencoba memindai port SSH `2222`.
  - SSHGuard berhasil mengeksekusi 85 tindakan pemblokiran bertingkat secara otomatis ke tabel `nftables` pada 23 IP agresif.
- **Aktivitas Harian (2026-10-06):**
  - Tercatat 38 IP unik mencoba probing koneksi SSH ke port 2222 dalam kurun waktu 24 jam terakhir.
  - Blacklist aktif saat audit: IP `77.91.71.91` dan `167.71.47.103` aktif di-drop pada chain blacklist `nftables` dengan durasi blokir eskalasi hingga 2 jam.
  - Top 5 IP penyerang terbanyak: `139.199.80.137` (146 kali), `77.91.71.91` (80 kali), `195.178.110.218` (46 kali), `77.239.124.161` (33 kali), dan `51.158.205.203` (30 kali).
- **Evaluasi Efisiensi Sumber Daya:**
  - `sshguard` hanya mengonsumsi memori ~4,1 MB RAM dengan akumulasi beban CPU 1,3 detik selama uptime 2 hari, membuktikan efisiensi arsitektur C native dibanding IDS berbasis Python (seperti Fail2ban) pada VPS RAM 2 GB.
  - Seluruh upaya intrusi berhasil ditangkal pada level layer jaringan tanpa ada kebocoran akses.

## 4. Catatan & Rekomendasi Lanjutan
1. Pertimbangkan mematikan `PasswordAuthentication no` pada SSH jika sudah sepenuhnya beralih ke SSH Key auth.
2. Pastikan port `20128` (9Router) hanya diakses untuk kebutuhan pengelolaan router.

Tautan Terkait:
- [[Proyek]]
- [[Pengetahuan]]
- [[Home]]