# Pacific Pedia SMM Integration PRD

Dokumentasi rancangan produk, arsitektur teknis, dan skema database untuk platform otomasi SMM Panel berbasis **Pacific Pedia API v2.0** dengan sistem **Deposit-Only** dan **Clerk Auth**.

File spesifikasi lengkap: `/home/ubuntu/PRD-Pacific-Pedia-SMM.md`

---

## Ringkasan Spesifikasi & Tech Stack
- **Framework**: Next.js 15 App Router (Server Actions & Route Handlers)
- **Styling**: Tailwind CSS & Shadcn UI
- **Authentication**: **Clerk** (`@clerk/nextjs`) dengan webhook sinkronisasi user ke database lokal
- **Database**: Supabase PostgreSQL dengan Row Level Security (RLS)
- **ORM**: Prisma ORM (Port 6543 pooler untuk runtime, port 5432 direct untuk migrasi)
- **Payment Gateway**: **Midtrans Core API** (Strictly **QRIS Only** via `payment_type: "qris"`)
- **Upstream Provider**: Pacific Pedia API v2.0 (`/profile`, `/s2`)

---

## Aturan Bisnis & Arsitektur Inti
1. **Pure Deposit-Based System**: Tidak ada *guest checkout*. Pengguna wajib login via Clerk dan memiliki saldo deposit aktif untuk memesan.
2. **Deposit Strictly QRIS**: Pengisian saldo hanya melalui satu metode yaitu QRIS Dinamis dari Midtrans Core API (`/v2/charge`). Notifikasi webhook memvalidasi signature SHA512 dan menambah saldo secara otomatis melalui `prisma.$transaction`.
3. **Atomic Order Execution**: Pemesanan memotong saldo akun secara instan. Jika API Pacific Pedia merespons error atau saldo supplier habis, sistem secara otomatis mengeksekusi *auto-refund* ke saldo akun pengguna.
4. **Sinkronisasi Katalog & Rate**: Penanganan otomatis rate per 1.000 vs rate paket 1 dari Pacific Pedia dengan margin keuntungan dinamis.
