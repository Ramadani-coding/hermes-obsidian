# Nara Topup

Dokumentasi arsitektur sistem, alur transaksi, dan konfigurasi teknis **Nara Topup**, platform e-commerce top-up game instan otomatis tanpa akun.

## Ringkasan Bisnis
- **Nama Platform:** Nara Topup
- **Lokasi Project:** `/home/ubuntu/nara-topup`
- **Model Bisnis:** Penjualan voucher dan mata uang digital game online (Mobile Legends, Free Fire, PUBG Mobile, Genshin Impact, Valorant, dll) secara instan 24/7.
- **Konsep Unggulan:** Guest checkout tanpa registrasi akun, fokus kecepatan transaksi, dan pembayaran QRIS instan.

## Alur Transaksi (Guest Checkout)
1. **Pilih Game & Input Akun:** Pembeli memilih judul game pada katalog etalase, lalu memasukkan ID pengguna (User ID / Zone ID / Server ID) yang divalidasi sistem.
2. **Pilih Denominasi & Nominal:** Memilih item voucher, diamond, koin, atau battle pass yang diinginkan.
3. **Pembayaran Instan:** Sistem men-generate pembayaran QRIS dinamis secara real-time via **Midtrans Core API**.
4. **Validasi Webhook:** Webhook Midtrans menerima callback pembayaran lunas secara instan.
5. **Auto-Fulfillment Aggregator:** Sistem otomatis menembak API distributor/provider voucher game untuk pengisian saldo ke akun pembeli secara otomatis.
6. **Pelacakan & Notifikasi:** Pembeli dapat memantau status transaksi melalui menu lacak pesanan berbasis nomor WhatsApp atau Invoice ID.

## Arsitektur & Tech Stack
- **Frontend & Backend:** Next.js 15 (App Router), React, TypeScript, Tailwind CSS.
- **Desain Antarmuka:** Tema gaming modern responsif dengan live search filter, promo banner slider, dan kartu katalog game terinspirasi standar industri (Windah Top Up Store).
- **Database:** PostgreSQL (Supabase Postgres 17.11) dikelola via Prisma ORM.
- **Environment Database:**
  - `DATABASE_URL`: Connection string pooler untuk query transaksi aplikasi.
  - `DIRECT_URL`: Connection string direct untuk migrasi skema Prisma.
- **Payment Gateway:** Midtrans Core API (QRIS).
- **Infrastruktur Pengembangan:**
  - Remote development via VS Code Remote-SSH di port `2222`.
  - Tooling AI OpenCode dilengkapi `@upstash/context7-mcp` untuk dokumentasi Next.js/Tailwind real-time.
  - Remote MCP Supabase (`https://mcp.supabase.com/mcp`) untuk manajemen tabel database langsung.

## Tahapan Milestone
- **Fase 1 (Selesai):** Pembuatan PRD, setup direktori project, koneksi database Supabase terverifikasi, instalasi MCP Context7 & Supabase pada OpenCode, serta implementasi UI katalog frontend responsif.
- **Fase 2 (Berikutnya):** Finalisasi skema tabel Prisma, integrasi API gateway Midtrans QRIS, dan koneksi API supplier aggregator game.

## Tautan Terkait
- Pemilik: [[Rama]]
- Ekosistem Produk: [[Nara Premium]]
- Daftar Proyek: [[Proyek]]
- Halaman Utama: [[Home]]