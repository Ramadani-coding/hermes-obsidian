# Pacific Pedia SMM Integration PRD

Dokumentasi rancangan produk, arsitektur teknis, dan skema database untuk platform otomasi SMM Panel berbasis **Pacific Pedia API v2.0**.

Tersimpan di: `/home/ubuntu/PRD-Pacific-Pedia-SMM.md`

---

## Ringkasan Spesifikasi
- **Framework**: Next.js 15 App Router (Server Actions & Route Handlers)
- **Styling**: Tailwind CSS
- **Database & Auth**: Supabase PostgreSQL + Supabase Auth
- **ORM**: Prisma ORM (Port 6543 pooler untuk runtime, port 5432 direct untuk migrasi)
- **Provider API**: Pacific Pedia API v2.0 (`/profile`, `/s2`)
- **Payment**: QRIS Dinamis (Midtrans/Tripay) + Saldo Akun Member

## Fitur Utama
1. **Direct Guest Checkout**: Pembeli bisa beli langsung via QRIS tanpa wajib daftar akun.
2. **Member & Reseller Tier**: Diskon harga khusus bagi reseller dengan saldo deposit.
3. **Auto Sync & Dynamic Margin**: Sinkronisasi katalog otomatis dengan margin profit terkontrol per kategori/layanan.
4. **Order Dispatcher & Auto Polling**: Eksekusi instan ke API Pacific Pedia setelah bayar, plus polling status otomatis dan penanganan garansi refill.
5. **Failover & Auto Refund**: Proteksi atomic transaksi jika saldo supplier kosong atau API gangguan.
