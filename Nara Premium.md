# Nara Premium (Nara Apps v2)

Dokumentasi arsitektur sistem dan alur bisnis **Nara Premium**, platform e-commerce D2C (Direct-to-Consumer) penjualan produk digital dan akun premium otomatis 24/7.

## Ringkasan Bisnis
- **Nama Platform:** Nara Premium (Nara Apps v2)
- **URL Web:** [https://premium.herama.my.id](https://premium.herama.my.id)
- **Repositori GitHub:** [https://github.com/Ramadani-coding/nara-appsV2](https://github.com/Ramadani-coding/nara-appsV2)
- **Model Bisnis:** Penjualan akun premium & produk digital (Netflix, Spotify, Canva Pro, CapCut Pro, AM, IQIYI, Prime Video, ChatGPT Plus, lisensi software).
- **Operasional:** 100% Otomatis tanpa campur tangan manusia (Auto-Fulfillment & Auto-Delivery).

## Alur Transaksi Otomatis (End-to-End)
1. **Pemilihan Produk:** Pelanggan memilih akun digital di etalase web (desain *Neobrutalism*) atau via Bot Discord (`/produk`).
2. **Pembayaran Instan:** Sistem menerbitkan QRIS via **Midtrans Core API**.
3. **Deteksi Callback Webhook:** Webhook Midtrans menerima konfirmasi pembayaran lunas secara real-time.
4. **Auto-Fulfillment Supplier:** Backend Express otomatis menembak API supplier (**Premku / Premiumku API**) untuk pemesanan stok akun/lisensi.
5. **Auto-Delivery & Notifikasi Dual-Channel:**
   - Kredensial akun (email, password, tata cara login) langsung tampil di halaman invoice web atau DM Discord.
   - Gateway **GoWA** otomatis mengirim notifikasi rincian pesanan dan kredensial langsung ke WhatsApp pembeli.

## Arsitektur & Tech Stack
- **Frontend Web:** React 18 + Vite, TypeScript, TailwindCSS (Neobrutalism), Framer Motion, Lucide React, React Router DOM v6.
- **Backend Service:** Node.js Express, TypeScript, Drizzle ORM.
- **Database & Auth:** PostgreSQL via Supabase DB + Supabase Auth JWT.
- **Bot Discord:** discord.js v14, qrcode.
- **Integrasi Pihak Ketiga:**
  - Payment Gateway: Midtrans (QRIS API)
  - Supplier API: Premiumku / Premku API
  - WhatsApp Gateway: GoWA
- **Deployment:** Docker & Docker Compose di VPS Linux.
- **Otomatisasi Promosi & Webhook:**
  - Discord Webhook Auto-Promo (Jadwal 12:30 WITA & 20:00 WITA via Hermes Cron).
  - GoWA Inbound Webhook (`/api/webhook/whatsapp`) untuk auto-forward broadcast WhatsApp admin bertag `#dc` langsung ke Discord.
  - Generator Poster WhatsApp Story (1080x1920 UI card-based, rasio 9:16 safe zone, Poppins, vector icons di `/home/ubuntu/.hermes/scripts/generate_story_poster.py`).
  - Generator Poster Instagram Feed (1080x1350 putih-biru, rasio 4:5 di `/home/ubuntu/.hermes/scripts/generate_ig_poster_blue.py`).
  - Template Google Stitch Flyer 9:16 (Project Stitch `Nara Premium Social Poster` ID `8640218841062584220`, HTML `/home/ubuntu/nara-stitch-flyer.html`, output `/home/ubuntu/nara-stitch-flyer.png` 1080x1920).

## Fitur Utama Admin Dashboard
- Statistik penjualan, kalkulasi Net Profit Margin (laba murni dari selisih harga jual vs modal supplier).
- Auto-reset statistik bulanan setiap tanggal 1.
- Ranking produk terlaris berdasarkan unit, omzet, dan profit.
- Auto-sync katalog & stok dari provider Premiumku dengan pengaturan margin markup Rupiah.
- Manajemen pesanan real-time dengan direct link chat WhatsApp pembeli dan tombol retry order manual.

## Tautan Terkait
- Pemilik: [[Rama]]
- Daftar Proyek: [[Proyek]]
- Halaman Utama: [[Home]]
