# Catatan Harian

Kumpulan log harian dan catatan aktivitas.

## 2026-10-03
- Pemeriksaan berkala: Tidak ada sesi percakapan atau tugas teknis baru dari Rama dalam 24 jam terakhir. Seluruh layanan berjalan stabil.

## 2026-10-02
- Evaluasi arsitektur deployment VPS (RAM 2 GB): Analisis perbandingan native `systemd` service vs Docker container untuk Mission Control, Warranty Quick-Hub, dan Cloudflare Tunnel. Diputuskan tetap mempertahankan deployment native Python stdlib & cloudflared via systemd demi efisiensi memori (menghemat overhead Docker daemon ~50-100 MB).
- Verifikasi dan audit berkala proteksi server via `sshguard` & `sshd`: Terdata 499 log serangan dari 161 IP scanner, dengan 18 aksi pemblokiran otomatis `nftables` pada 7 IP agresif. Pertahanan server terverifikasi tangguh dan efisien.

## 2026-10-01
- Perancangan dan peluncuran aplikasi [[Warranty Quick-Hub Promotor Portal]] (`https://link.herama.my.id`) berbasis Python dual-stack HTTP server + SQLite (`warranty.db`) di port 9050, terhubung via Cloudflare Tunnel.
- Pembentukan agen baru **Nara** (`nara-marketer`) sebagai spesialis pemasaran produk digital [[Nara Premium]], terintegrasi ke 9Router `combo-nara` dan Mission Control Dashboard.
- Overhaul UI Mobile Mission Control Dashboard (`https://mission.herama.my.id`): perbaikan header, bottom nav touch targets, visual office status bar, modal full-screen, serta penambahan header `Cache-Control: no-cache, must-revalidate` pada HTTP static server.

## 2026-09-30
- Pemulihan memori, SOUL, dan profil Fern/Rama pasca reset sistem.
- Pembuatan dan pemasangan SSH Deploy Key baru untuk sinkronisasi repository vault `Ramadani-coding/hermes-obsidian`.
- Audit dan perapian catatan hasil hardening keamanan VPS (firewalld, sshguard, port 2222, dan pembukaan port 20128 untuk 9Router).
- Penyesuaian konfigurasi Hermes (`cron.wrap_response: false`) agar pesan pengingat/jadwal tampil bersih dan natural.
- Menganalisis perbedaan rotasi multi-akun (load-balancing otomatis) vs rotasi model di 9Router, mendokumentasikannya ke dalam [[Arsitektur dan Konfigurasi 9Router]]. Disarankan menggunakan mode Fallback agar performa respons tetap konsisten.
- Menyiapkan metode transfer identitas dan ingatan Fern antar-VPS via arsip file `SOUL.md` dan `memories/`, didokumentasikan di [[Transfer Ingatan Fern ke VPS Lain]].
- Menjelaskan panduan akses Obsidian Vault di perangkat mobile (Android via Obsidian Git plugin + GitHub PAT; iOS via Working Copy atau Remotely Save).
- Meninjau proyek `Mission Control Hermes` buatan komunitas (antarmuka kantor piksel 2D read-only dan pemantauan tugas/token berbasis Node.js 20+).

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