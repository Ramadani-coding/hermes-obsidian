# Nara Chronicle

Nara Chronicle adalah platform publikasi web editorial yang memuat esai mendalam mengenai riset AI terkini, kabar perkembangan model baru, arsitektur sistem server hemat daya, serta catatan riwayat kehidupan dan kerja nyata antara Rama dan Fern di atas VPS 2GB RAM.

## Filosofi Desain & Anti-Slop
- **Layout Marginalia Paper-Tone**: Menolak gaya dark-mode neon pasaran. Menggunakan palet warm broken-white (`#faf9f5`) dengan tinta gelap (`#1c1917`), dipadukan tipografi serif klasik untuk judul dan sans-serif bersih untuk body bacaan.
- **Standar 6 Modul Anti-Slop (Miqdad Badjuber)**:
  - `antislop` (Filosofi & Purpose Test)
  - `antislop-code` (Kodingan bersih tanpa boilerplate berlebih)
  - `antislop-copywriting` (Tulisan otentik, membuang jargon pemanis korporat)
  - `antislop-human` (Aksesibilitas dan kontras ramah mata)
  - `antislop-layoutmobile` (Mobile-first reflow, min tap target 44px, zero horizontal scroll leak)
  - `antislop-ui` (Tipografi terukur, hirarki visual jelas)
- **Bebas Gimmick Newsletter**: Disesuaikan atas arahan Rama dengan meniadakan formulir langganan/newsletter agar antarmuka fokus 100% pada pengalaman membaca yang tenang.

## Struktur Kategori
1. **Semua Catatan**: Menampilkan keseluruhan arsip esai dan catatan riwayat.
2. **Perkembangan Fern**: Catatan evaluasi memori, kapabilitas asisten, dinamika keseharian dengan Rama, dan catatan deadpan sistem.
3. **Berita & Update AI**: Berita teknologi kecerdasan buatan terbaru, model baru (seperti model Jev System 1 routing), analisis inferensi, dan protokol agen seperti MCP.
4. **Arsitektur Sistem**: Dokumentasi teknis konfigurasi VPS Ubuntu 24.04 RAM 2GB, native systemd, dual-stack socket networking, dan audit efisiensi memori tanpa Docker.

## Stack & Spesifikasi
- **Framework**: Next.js 15 (App Router), React 19, TypeScript
- **Styling**: Tailwind CSS
- **Iconography**: Lucide React
- **Lokasi Proyek**: `/home/ubuntu/nara-chronicle`
- **Integrasi MCP**: `@upstash/context7-mcp` terkonfigurasi di `.vscode/mcp.json` dan OpenCode

Kembali ke: [[Proyek]] | [[Home]]
