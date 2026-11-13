# Proyek

Daftar proyek dan tugas yang sedang aktif.

## Prioritas Tinggi (Selesai / Aktif)
- [x] **[[Nara Chronicle]] (Editorial AI & Human Journal)**
  - Web jurnal terbuka editorial esai perkembangan AI, model baru, dan riwayat interaksi Fern & Rama.
  - Stack: Next.js 15, Tailwind CSS, layout Marginalia paper-tone (`#faf9f5`), anti-slop rules, mobile-responsive navigation.
  - Lokasi: `/home/ubuntu/nara-chronicle`
  - Status: Selesai dibangun. Terhubung ke Cloud Supabase PostgreSQL via Prisma ORM, fitur interaktif real-time (like/unlike RPC, guestbook komentar, view counter deduplikasi, reading progress bar 60fps, badge baru) aktif, serta otomasi cronjob publikasi berita AI harian (08:30 WITA) berjalan mandiri.
- [x] **[[Nara Watch]] (Server & Website Uptime Monitor)**
  - Micro-SaaS monitoring uptime server & website instan via Bot Telegram (<30MB RAM usage).
  - Bot Telegram: `@narawatchbot`
  - Stack: Python Asyncio, HTTPX, SQLite3, Telegram Bot API, managed via systemd `nara-watch.service`.
  - Lokasi: `/home/ubuntu/nara-watch`
  - Status: Selesai dibangun dan aktif berjalan (~21MB RAM). Mendukung auto-ping, deteksi DOWN/RECOVERED, dan interaksi command bot.
- [ ] **[[Nara Topup]] (Top-up Game Instan)**
  - Platform top-up game instan otomatis tanpa registrasi akun (guest checkout, Midtrans QRIS, Supabase PostgreSQL, Prisma ORM, Next.js 15).
  - Lokasi: `/home/ubuntu/nara-topup`
  - Status: PRD matang, database Supabase terhubung, OpenCode MCP Context7 & Supabase aktif, frontend katalog etalase responsif selesai dibangun.
- [x] **[[Nara Premium]] (Nara Apps v2)**
  - Platform e-commerce otomatis penjualan akun premium (Midtrans QRIS + GoPay QRIS gateway + Supplier API).
  - Web: https://premium.herama.my.id
  - Repo: https://github.com/Ramadani-coding/nara-appsV2
  - Status: Berhasil dipindahkan dan aktif berjalan bare-metal di STB Armbian (`/root/projects/nara-appsV2`), backend port 5001 dikelola PM2, frontend Nginx port 80, Redis port 6379, dan Cloudflare Tunnel aktif. Referensi: [[Setup Arsitektur Nara Apps dan Cloudflare Tunnel di STB Armbian]].
- [x] **Catatan Fern (Web Jurnal Statis STB Armbian)**
  - Web jurnal lapangan dan refleksi harian berbahasa alami tanpa beban RAM runtime (`0 MB` idle).
  - Web: https://nara.herama.my.id
  - Stack: Kompilasi Markdown Obsidian Vault ke HTML statis oleh skrip Python stdlib (`/root/scripts/build_journal.py`) dan disajikan Nginx port 3050/80 (`/var/www/journal`) via Cloudflare Tunnel.
- [x] **Otomasi Threads Fern (`@momen_klip`)**
  - Sistem publikasi status spontan, pembacaan inbox komentar, balasan otomatis, dan pemantauan analitik Threads.
  - Stack: Python stdlib CLI (`/root/scripts/threads_manager.py` via Zernio API dan `/root/scripts/threads_buffer.py` via Buffer GraphQL API), berjalan lewat cronjob spontanitas 4x sehari waktu WITA. Referensi: [[Otomasi Threads dan Manajemen Komentar via Zernio API di STB Armbian]].
- [x] **Integrasi YouTube Channel Manager (`@rammadan_code`)**
  - Pengelolaan channel YouTube Rama (playlist, update judul/deskripsi video, statistik) via YouTube Data API v3 OAuth 2.0.
  - Stack: Python stdlib CLI (`/root/scripts/youtube_manager.py`), auto-refresh token, 0 MB memory idle. Referensi: [[Integrasi YouTube Channel Manager via OAuth 2.0 di STB Armbian]].
- [x] **Ruang (Hermes 3D & 2D Virtual Office & Mission Control)**
  - [x] Replatforming dari mission-control Python ke `yugienugraha/ruang` (Node.js Express + React 19 + Three.js / React Three Fiber)
  - [x] Kantor visual 3D interaktif 2 lantai (Lantai 1 ruang kerja, meeting, pantry, gerobak bakso/kopi keliling; Lantai 2 kamar tidur istirahat agen idle)
  - [x] Mode fallback 2D untuk browser ringan / mobile
  - [x] Managed via systemd user service (`ruang.service` aktif di port 3001, linger enabled)
  - [x] Fitur lengkap: Status agen real-time via `hermes` CLI, Kanban board, Calendar cronjob, Token usage insights, Folder & Memory inspector (SOUL.md/MEMORY.md), logs tailing, dan access code protection
  - [x] Integrasi Hermes multi-profile (default, leadengineer, nara-marketer) & OpenCode CLI
- [x] **Apple Quick-Hub Promotor Portal (`https://link.herama.my.id`)**
  - [x] Portal PWA mobile-first untuk link aktivasi, cek garansi AppleCare, IMEI Kemenperin & Bea Cukai
  - [x] Backend SQLite CRUD (`http.server` dual-stack port 9050, ~10MB RAM)
  - [x] Managed via systemd service (`warranty-hub.service`) & Cloudflare Tunnel
- [x] **Hardening Keamanan Server VPS**
  - [x] Pindah port SSH ke `2222/tcp` dan set `PermitRootLogin no`
  - [x] Pasang dan aktifkan firewall `firewalld` (port terbuka: 2222, 20128)
  - [x] Pasang dan aktifkan `sshguard` untuk mitigasi serangan brute-force via nftables
  - [x] Selesaikan akses dashboard 9Router melalui firewall
  - Referensi teknis: [[Audit Keamanan VPS]]

## Eksplorasi & Proyek Lanjutan
- [ ] **Optimasi Model Routing & Fallback (9Router)**
  - Pengaturan Round Robin fallback Gemini Pro via Antigravity OAuth.
- [ ] **Pengembangan & Karakter Asisten Hermes**
  - Mengasah gaya percakapan [[Fern]] agar tetap natural, ringkas, dan bebas dari format robotik AI.

Kembali ke: [[Home]]