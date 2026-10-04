# Nara Watch

Nara Watch adalah micro-SaaS monitoring uptime server dan website instan berbasis bot Telegram, dirancang dengan fokus konsumsi daya dan RAM ultra-rendah (<30MB) untuk lingkungan VPS mandiri.

## Arsitektur & Teknologi
- **Engine**: Python 3.14 / 3.12 dengan `asyncio`
- **HTTP Client**: `httpx` (asynchronous non-blocking ping dengan pelacakan latency presisi milidetik)
- **Database**: SQLite3 (`nara_watch.db`) dengan schema relational ringan (`users`, `monitors`, `events`)
- **Antarmuka Utama**: Telegram Bot API (long polling via getUpdates, zero port open required)
- **Konsumsi Memori**: ~26MB RSS RAM

## Fitur Utama
1. **Deteksi Gangguan & Pemulihan (State Machine)**:
   - Notifikasi seketika jika target DOWN (Connection Timeout, DNS Error, status 500/502/503).
   - Notifikasi pemulihan saat layanan kembali UP disertai rincian latency.
2. **Kendali Penuh via Telegram Bot**:
   - `/add <url> [nama]`: Daftarkan URL website atau API target.
   - `/list`: Daftar target aktif beserta indikator status (🟢 UP / 🔴 DOWN) dan response time.
   - `/check <url>`: Pengujian on-demand instan tanpa harus mendaftarkan target.
   - `/delete <id>`: Menghapus target monitor.
   - `/status`: Ringkasan kesehatan seluruh sistem.

## Lokasi Proyek
- Jalur: `/home/ubuntu/nara-watch`
- Eksekusi: `.venv/bin/python main.py`

Kembali ke: [[Proyek]] | [[Home]]
