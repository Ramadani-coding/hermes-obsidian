# Proyek

Daftar proyek dan tugas yang sedang aktif.

## Prioritas Tinggi (Selesai / Aktif)
- [x] **[[Nara Chronicle]] (Editorial AI & Human Journal)**
  - Web jurnal terbuka editorial esai perkembangan AI, model baru, dan riwayat interaksi Fern & Rama.
  - Stack: Next.js 15, Tailwind CSS, layout Marginalia paper-tone (`#faf9f5`), anti-slop rules, mobile-responsive navigation.
  - Lokasi: `/home/ubuntu/nara-chronicle`
  - Status: Selesai dibangun. Terhubung ke Cloud Supabase PostgreSQL via Prisma ORM, fitur interaktif real-time (like/unlike RPC, guestbook komentar, view counter deduplikasi, reading progress bar 60fps, badge baru) aktif, serta otomasi cronjob publikasi berita AI harian (08:30 WITA) berjalan mandiri.
- [ ] **[[Nara Watch]] (Server & Website Uptime Monitor)**
  - Micro-SaaS monitoring uptime server & website instan via Bot Telegram (<30MB RAM usage).
  - Stack: Python Asyncio, HTTPX, SQLite3, Telegram Bot API.
  - Lokasi: `/home/ubuntu/nara-watch`
  - Status: MVP engine & test suite selesai dibangun (26MB RAM), struktur bot Telegram dan database siap.
- [ ] **[[Nara Topup]] (Top-up Game Instan)**
  - Platform top-up game instan otomatis tanpa registrasi akun (guest checkout, Midtrans QRIS, Supabase PostgreSQL, Prisma ORM, Next.js 15).
  - Lokasi: `/home/ubuntu/nara-topup`
  - Status: PRD matang, database Supabase terhubung, OpenCode MCP Context7 & Supabase aktif, frontend katalog etalase responsif selesai dibangun.
- [x] **[[Nara Premium]] (Nara Apps v2)**
  - Platform e-commerce otomatis penjualan akun premium (Midtrans QRIS + Supplier Premku API + GoWA WhatsApp gateway).
  - Web: https://premium.herama.my.id
  - Repo: https://github.com/Ramadani-coding/nara-appsV2
- [x] **Mission Control AI Team MVP & 2D Visual Office**
  - [x] Backend HTTP Server (`http.server` stdlib, port 9000, read-only SQLite `kanban.db` & `state.db`)
  - [x] Managed via systemd service (`mission-control.service` auto-restart & boot start)
  - [x] Cloudflare Tunnel & Cloudflare Access (`https://mission.herama.my.id`)
  - [x] Mobile responsive overhaul (bottom nav, mobile 2D office viewport, auto cache-busting)
  - [x] Frontend SPA dark mode 7 tab (Overview, Agents, 2D Visual Office, Task Board, Calendar, Activity, Memory)
  - [x] Visual Office 2D pixel-art (Workspace 0 & Lounge room switching, automatic real data state mapping)
  - [x] Integrasi OpenCode AI CLI & Nara Marketing Agent (`nara-marketer`) terhubung ke 9Router `combo-nara`
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