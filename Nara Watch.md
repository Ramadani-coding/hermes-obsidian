# Nara Watch

Nara Watch adalah micro-SaaS monitoring uptime server dan website instan berbasis bot Telegram, dirancang dengan fokus konsumsi daya dan RAM ultra-rendah (<30MB) untuk lingkungan VPS mandiri.

## Arsitektur & Teknologi
- **Engine**: Python 3.14 / 3.12 dengan `asyncio`
- **HTTP Client**: `httpx` (asynchronous non-blocking ping dengan pelacakan latency presisi milidetik)
- **Database**: SQLite3 (`nara_watch.db`) dengan schema relational ringan (`users`, `monitors`, `events`)
- **Antarmuka Utama**: Telegram Bot API (long polling via getUpdates, interactive inline keyboard buttons, zero port open required)
- **Konsumsi Memori**: ~26MB RSS RAM

## Fitur Utama
1. **Deteksi Gangguan & Pemulihan (State Machine)**:
   - Notifikasi seketika jika target DOWN (Connection Timeout, DNS Error, status 500/502/503).
   - Notifikasi pemulihan saat layanan kembali UP disertai rincian latency.
2. **Pemantauan Masa Aktif SSL Otomatis**:
   - Pengecekan TLS handshake tanpa membebani server target.
   - Deteksi sisa hari masa aktif sertifikat SSL, tanggal kedaluwarsa, dan penerbit (CA).
   - Peringatan otomatis jika sertifikat mendekati kedaluwarsa (<7 hari & <3 hari).
3. **Monitoring Resource Host VPS**:
   - Visual progress bar CPU load average, RAM usage (`/proc/meminfo`), dan disk root (`shutil.disk_usage`).
   - Uptime host serta status service systemd utama (`mission-control`, `warranty-hub`, `nara-watch`, `cloudflared`) dan 9Router.
   - Peringatan darurat Telegram otomatis jika penggunaan RAM atau Disk menyentuh ambang batas >=90% (cooldown 15 menit).
4. **Kendali Penuh via Telegram Bot**:
   - `/add <url> [nama]`: Daftarkan URL website atau API target.
   - `/list`: Daftar target aktif beserta indikator status, response time, dan badge sisa hari SSL dengan tombol interaktif (⚡ Cek instan, 🗑️ Hapus aman, 🖥️ Resource VPS).
   - `/server` atau `/vps`: Ringkasan langsung beban server host dan status layanan penting.
   - `/ssl`: Tampilan ringkas status masa aktif sertifikat SSL seluruh website.
   - `/check <url>`: Pengujian on-demand instan (HTTP status + SSL info) tanpa harus mendaftarkan target.
   - `/delete <id>`: Menghapus target monitor.
   - `/status`: Ringkasan kesehatan seluruh sistem.

## Lokasi Proyek & Status
- Jalur: `/home/ubuntu/nara-watch`
- Service: `systemd` unit `nara-watch.service` (auto-restart & start on boot)
- Bot Telegram: `@narawatchbot`
- Memory Footprint: ~21MB RSS RAM

Kembali ke: [[Proyek]] | [[Home]]
