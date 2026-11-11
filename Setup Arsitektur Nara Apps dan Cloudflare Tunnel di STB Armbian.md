# Setup Arsitektur Nara Apps dan Cloudflare Tunnel di STB Armbian

Dokumentasi arsitektur dan panduan penerapan bare-metal Nara Apps v2 (Toko Digital Premium) dan integrasi Cloudflare Tunnel pada perangkat STB Armbian (ARM64, RAM 2GB).

## 1. Latar Belakang & Pertimbangan Efisiensi
Menjalankan Docker compose secara langsung di STB Armbian dengan RAM 1.8 GB berisiko tinggi memicu Linux OOM Killer, terutama pada proses `npm ci`, kompilasi `tsc`, dan bundling `vite build`. Sebagai solusi aman dan hemat sumber daya, arsitektur diubah menjadi sistem bare-metal:
- Kompilasi TypeScript dan build frontend dilakukan di PC Windows lokal pengembang.
- STB Armbian hanya menerima file artefak matang (`dist/`) dan mengeksekusi runtime Node.js serta Nginx ringan.
- Total konsumsi RAM seluruh layanan terjaga di kisaran ~48% (sisa memori bebas ~900 MB).

## 2. Topologi & Alokasi Layanan
- **Frontend SPA**: React 19, Vite, Tailwind CSS v4, dan Lucide React. Disajikan oleh Nginx di port 80 melalui direktori `/var/www/nara-store` dengan konfigurasi SPA fallback ke `index.html`.
- **Backend API**: Node.js Express + Drizzle ORM di port 5001. Dikelola oleh PM2 (`nara-backend`) dengan fitur pemantauan proses mandiri.
- **Cache & Queue**: Redis server lokal di port 6379, dikonfigurasi hemat memori (`--maxmemory 64mb --maxmemory-policy noeviction --appendonly no --save ""`).
- **Domain Publik & SSL**: Cloudflare Tunnel (`cloudflared.service`) mengarahkan subdomain `premium.herama.my.id` langsung ke `http://127.0.0.1:80`.
- **Integrasi Repositori**: GitHub MCP Server native Go ARM64 (`/usr/local/bin/github-mcp-server` v2.1.0) dengan mode lazy dan idle timeout 120 detik untuk menghemat RAM.

## 3. Konfigurasi Nginx (`/etc/nginx/sites-available/nara-store`)
Nginx bertindak sebagai web server statis sekaligus reverse proxy API:
- `location /`: Melayani berkas statis frontend (`try_files $uri $uri/ /index.html`).
- `location /api/`: Mem-proxy request ke backend internal `http://127.0.0.1:5001/api/`.

## 4. Ketahanan Booting (Autostart)
Seluruh layanan inti telah dikonfigurasi untuk hidup otomatis saat STB menyala pasca pemadaman listrik:
- `systemd linger` untuk root aktif.
- Unit service aktif on boot: `pm2-root.service`, `nginx.service`, `redis-server.service`, `cloudflared.service`, dan `hermes-gateway.service`.

Tautan Terkait:
- [[Pengetahuan]]
- [[Proyek]]
- [[Nara Premium]]
- [[Home]]
