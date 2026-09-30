# Catatan Harian

Kumpulan log harian dan catatan aktivitas.

## 2026-10-01
- Perancangan dan peluncuran aplikasi [[Warranty Quick-Hub Promotor Portal]] (`https://link.herama.my.id`) berbasis Python dual-stack HTTP server + SQLite (`warranty.db`) di port 9050, terhubung via Cloudflare Tunnel.
- Pembentukan agen baru **Nara** (`nara-marketer`) sebagai spesialis pemasaran produk digital [[Nara Premium]], terintegrasi ke 9Router `combo-nara` dan Mission Control Dashboard.
- Overhaul UI Mobile Mission Control Dashboard (`https://mission.herama.my.id`): perbaikan header, bottom nav touch targets, visual office status bar, modal full-screen, serta penambahan header `Cache-Control: no-cache, must-revalidate` pada HTTP static server.

## 2026-09-30
- Pemulihan memori, SOUL, dan profil Fern/Rama pasca reset sistem.
- Pembuatan dan pemasangan SSH Deploy Key baru untuk sinkronisasi repository vault `Ramadani-coding/hermes-obsidian`.
- Audit dan perapian catatan hasil hardening keamanan VPS (firewalld, sshguard, port 2222, dan pembukaan port 20128 untuk 9Router).
- Penyesuaian konfigurasi Hermes (`cron.wrap_response: false`) agar pesan pengingat/jadwal tampil bersih dan natural.

## 2026-09-29
- Membentuk identitas asisten [[Fern]] dan profil pengguna [[Rama]].
- Menyiapkan vault Obsidian lokal di VPS dan mengaitkan sinkronisasi GitHub via SSH Deploy Key.
- Mengonfigurasi Web Dashboard Hermes di port 9119 dan menghubungkannya ke domain publik via Cloudflare Tunnel.
- Menyelesaikan kendala Host header dan WebSocket origin mismatch, serta memasang autentikasi basic login (didokumentasikan di [[Setup Dashboard Hermes dan Cloudflare Tunnel]]).
- Melakukan audit keamanan VPS dan mencatat rekomendasinya di [[Audit Keamanan VPS]].
- Menambahkan dokumentasi mengenai perbedaan [[Profil Hermes]] vs Sub-Agent.
- Menambahkan dokumentasi mengenai [[Konsep Sesi dan Grup di Hermes]] (isolasi obrolan via `group_sessions_per_user`).
- Menyesuaikan aturan pencatatan ke Obsidian agar hanya menyimpan informasi teknis krusial dan mendirikan rutinitas evaluasi malam harian.

Kembali ke: [[Home]]