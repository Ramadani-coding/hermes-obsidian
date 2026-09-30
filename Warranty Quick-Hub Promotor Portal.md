# Warranty Quick-Hub Promotor Portal

Dokumentasi arsitektur, konfigurasi, dan pengoperasian portal cepat aktivasi & cek garansi ekosistem Apple untuk promotor kartu Syihab.

## Ringkasan Spesifikasi
- **URL Publik:** `https://link.herama.my.id`
- **Lokasi Kode:** `/home/ubuntu/warranty-hub`
- **Port Lokal:** `9050` (Dual-stack IPv4/IPv6 `[::]:9050`)
- **Backend:** Standalone Python HTTP Server (`server.py` stdlib `http.server` + SQLite)
- **Database:** SQLite `/home/ubuntu/warranty-hub/warranty.db`
- **Systemd Service:** `warranty-hub.service` (Auto-start saat boot & auto-restart)
- **Penggunaan Memori:** Ultra-ringan (~10 MB RAM)

## Fitur Utama PWA
1. **Khusus Ekosistem Apple & Legalitas:** 9 portal utama (Check Coverage, Apple Support, iCloud Lock, Status Perbaikan, Locator ASP, Apple ID, IMEI Kemenperin, IMEI Bea Cukai, System Status).
2. **Kategori & Pencarian Kilat:** Filter chip per kategori, pencarian real-time via input text.
3. **PWA Standalone:** Fitur Web App Manifest (`manifest.json`) dan Service Worker (`sw.js`) untuk pemasangan ke homescreen HP promotor.
4. **CRUD Modal:** Manajemen link (tambah, edit, hapus) via UI responsive mobile.
5. **Click Counter:** Otomatis menghitung frekuensi penggunaan link.

## Konfigurasi Systemd (`/etc/systemd/system/warranty-hub.service`)
```ini
[Unit]
Description=Warranty Quick-Hub Promotor Portal
After=network.target

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/home/ubuntu/warranty-hub
ExecStart=/usr/bin/python3 server.py
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```

## Cloudflare Tunnel Ingress
Ditambahkan ke Cloudflare Tunnel via `cloudflared`:
- Hostname: `link.herama.my.id`
- Service: `http://127.0.0.1:9050`

Kembali ke: [[Pengetahuan]] | [[Home]]
