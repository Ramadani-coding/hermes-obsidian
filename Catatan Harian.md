# Catatan Harian

Kumpulan log harian dan catatan aktivitas.

## 2026-10-05
- Integrasi [[Google Flow MCP]] (`https://flow.googleapis.com/mcp`) ke Hermes Agent dan pembuatan CLI client mandiri `/home/ubuntu/.hermes/scripts/flow_client.py`.
- Penegakan disiplin waktu sistem: insiden salah asumsi jam pagi yang memicu penetapan aturan absolut pemanggilan perintah `date` sebelum merespons konteks temporal di `SOUL.md` dan memori permanen.
- Produksi materi visual promosi Nara Premium vertikal 9:16 (1080x1920) untuk status WhatsApp/Instagram menggunakan inferensi model FLUX.1-schnell dan komposisi Pillow (CapCut, Prime Video, Gemini).
- Eksperimen produksi video UGC AI dengan model Veo 3.1 di Google Flow: perumusan teknik Master Portrait 9:16 untuk konsistensi wajah, koreografi kamera `camera smoothly tracks backwards`, penstabilan artikulasi fonetik Bahasa Indonesia pada nama brand, dan isolasi latar lingkungan taman kota (didokumentasikan di [[Panduan Prompting dan Konsistensi Video AI Veo (Google Flow)]]).
- Publikasi artikel teknis [[Nara Chronicle]] №15 mengenai analisis arsitektur Meta Movie Gen (Flow Matching 30B dan sintesis audio 13B).

## 2026-10-04
- Integrasi [[Google Stitch MCP]] (`https://stitch.googleapis.com/mcp`) untuk perancangan antarmuka pengguna berbasis AI dan ekstraksi token desain DESIGN.md.
- Evaluasi poster AI slop versus pendekatan desain berbasis kode (SVG/HTML rendering) dan penyusunan artikel Nara Chronicle №14 ("Ilusi Seni Difusi dan Kebangkitan Desain Berbasis Kode").
- Penyusunan standar operasional prosedur pengembangan produk MVP berbasis [[SOP Pengembangan MVP (SDD)]] (BRD, PEP, ITA, dan kriteria pengujian terisolasi).
- Sinkronisasi cadangan identitas dan memori jangka panjang Fern ke folder `Ingatan Fern/` di Obsidian Vault.
- Publikasi artikel Nara Chronicle №13 mengenai arsitektur Ling 3.1 Flash Ant Group MoE 560B.

## 2026-10-03
- Pembangunan platform web editorial [[Nara Chronicle]] (`/home/ubuntu/nara-chronicle`): Next.js 15, Tailwind CSS, arsitektur anti-slop paper-tone (`#faf9f5`), database remote Cloud Supabase PostgreSQL via Prisma ORM (`DATABASE_URL` pooler + `DIRECT_URL` direct).
- Implementasi fitur interaktif real-time pada Nara Chronicle: like/unlike via WebSocket Supabase Realtime & PostgreSQL RPC atomik (`increment_article_likes`, `decrement_article_likes`), kolom respon guestbook tanpa login akun, client-side live filtering search bar, view counter deduplikasi session-storage, dan reading progress bar 60fps.
- Parser rendering inline markdown kustom untuk format huruf miring (*italic*), tebal (**bold**), dan inline code (`` ` ``) pada halaman detail artikel.
- Penjadwalan otomasi cronjob publikasi berita AI harian setiap 08:30 WITA (`30 8 * * *`) dengan verifikasi tahun 2026 dan bulan berjalan.
- Inisiasi arsitektur dan pengembangan proyek baru [[Nara Topup]] (`/home/ubuntu/nara-topup`): sistem top-up game instan guest checkout tanpa akun pembeli berbasis Next.js 15, Tailwind CSS, TypeScript, dan Midtrans QRIS.
- Integrasi database Supabase PostgreSQL (v17.11) via Prisma ORM dengan konfigurasi `DATABASE_URL` (pooler) dan `DIRECT_URL` (direct).
- Konfigurasi toolchain OpenCode AI dengan MCP Upstash Context7 (`@upstash/context7-mcp`) dan remote MCP Supabase (`https://mcp.supabase.com/mcp`).
- Penyiapan akses remote development via VS Code Remote-SSH di port 2222 langsung ke VPS.
- Penyelesaian UI katalog etalase game responsif Nara Topup dengan referensi desain industri gaming.
- Perbaikan kendala 502 Bad Gateway Cloudflare Tunnel pada Mission Control: pembaruan server HTTP Python stdlib menggunakan pola `DualStackHTTPServer` binding IPv4/IPv6 (`::`) dengan `IPV6_V6ONLY=0`.
- Konfigurasi suara TTS asisten Fern menggunakan Edge TTS model bahasa Indonesia wanita alami (`id-ID-GadisNeural`) dengan preferensi hanya mengirim VN saat diminta eksplisit.

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