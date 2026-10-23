# Arsitektur Gateway QRIS Mandiri via Reverse Engineering GoPay Merchant

Dokumentasi analisis arsitektur, mekanisme kerja, dan evaluasi risiko teknis penerapan gateway pembayaran QRIS mandiri berbasis rekayasa balik (reverse engineering) API GoBiz dan GoPay Merchant.

## Latar Belakang dan Konteks
Pada pengembangan platform digital D2C (Direct to Consumer) dan pembayaran instan tanpa akun (seperti [[Nara Topup]] dan [[Nara Premium]]), ketergantungan pada payment gateway pihak ketiga (seperti Midtrans atau Xendit) memerlukan biaya transaksi (MDR/fee agregator) serta proses verifikasi legalitas usaha yang cukup formal.

Eksplorasi terhadap repositori komunitas (`ahmadzakiyox/gopay-api-gateaway`) membuka studi kelayakan penggunaan akun mitra usaha resmi GoPay (GoBiz / GoPay Merchant) secara langsung sebagai backend penerima pembayaran QRIS otomatis tanpa perantara agregator pihak ketiga.

## Komponen dan Alur Kerja Arsitektur

### 1. Manajemen Sesi dan Otentikasi (Session Manager)
- **Inisialisasi Login CLI**: Login pertama kali dilakukan melalui terminal menggunakan nomor ponsel merchant yang terdaftar di GoBiz / GoPay Merchant.
- **Verifikasi OTP**: Pengguna memasukkan kode OTP yang dikirimkan Gojek via SMS atau WhatsApp.
- **Persistensi Token**: Akses token dan refresh token yang diperoleh dari endpoint privat Gojek disimpan secara lokal ke dalam file konfigurasi sesi JSON.
- **Auto-Refresh Lifecycle**: Daemon backend secara berkala memperbarui token sebelum masa kedaluwarsa tiba (siklus per 6 jam) atau saat menerima respons HTTP status `401 Unauthorized`, menjaga sesi tetap aktif tanpa login ulang manual.

### 2. Mesin Pembuat QRIS Dinamis Lokal (Dynamic QRIS Engine)
Sistem tidak membuat API call ke Gojek untuk tiap pembentukan QR code baru, melainkan memproses string QRIS statis bawaan merchant secara lokal di server.
- **Standar EMVCo**: String QRIS nasional Indonesia mengikuti spesifikasi EMVCo QR Code standard (format TLV: Tag-Length-Value).
- **Injeksi Tag 01**: Mengubah nilai tag `01` (Point of Initiation Method) dari `11` (statis) menjadi `12` (dinamis).
- **Injeksi Tag 54**: Menyisipkan tag `54` yang memuat nominal transaksi spesifik (contoh: `540515000` untuk Rp 15.000).
- **Kalkulasi Ulang CRC16**: Menghitung ulang nilai checksum pada tag `63` menggunakan algoritma CRC-16/CCITT-FALSE (polinomial `0x1021`, nilai awal `0xFFFF`).
- **Penguncian Nominal**: Saat kode QR di-scan oleh pembeli via e-wallet (GoPay, OVO, Dana, BCA, dll), nominal terkunci secara otomatis sehingga pembeli tidak dapat mengubah angka transfer.

### 3. Pengecekan Mutasi dan Polling Real-Time
- **Polling Analitik Transaksi**: Saat halaman kasir checkout terbuka di sisi pelanggan, server melakukan polling berkala ke endpoint API privat analitik merchant (`api.gojekapi.com/merchant-analytics/v2/merchants/transactions`).
- **Validasi Nilai dan Jendela Waktu**: Server memfilter riwayat mutasi masuk berdasarkan kecocokan nominal dan rentang timestamp yang valid.
- **Pencegahan Double-Claim**: Menggunakan mekanisme pencatatan ID transaksi unik ke dalam cache memori atau basis data lokal agar mutasi dana yang sama tidak dapat diklaim dua kali oleh pesanan yang berbeda dengan nominal serupa.

## Perbandingan Arsitektur: Gateway Agregator vs Gateway Mandiri

| Parameter | Agregator Resmi (Midtrans / Xendit) | Gateway Mandiri (GoBiz Reverse Engineering) |
| :--- | :--- | :--- |
| **Biaya Transaksi (MDR)** | Ada potongan fee agregator per transaksi + PPN | Nol fee tambahan pihak ketiga (hanya MDR standar QRIS) |
| **Mekanisme Notifikasi** | Webhook instan berbasis event push server-to-server | Polling berkala client/server (beban request berulang) |
| **Keandalan API** | SLA resmi, dokumentasi publik, stabil | Tidak resmi, rawan berubah sewaktu-waktu tanpa pemberitahuan |
| **Risiko Keamanan Akun** | Bebas risiko pemblokiran, legalitas terjamin | Rawan suspend atau limitasi akun akibat request anomali |
| **Ketergantungan Kode** | SDK resmi teruji | Kode repositori sering ter-obfuscate dan sulit diaudit |

## Evaluasi Risiko dan Rekomendasi Teknis

1. **Obfuscation Kode Sumber**: File krusial login dan pengelolaan sesi pada repositori pihak ketiga sering kali diobfuscate rapat. Hal ini menyulitkan audit keamanan, berisiko menyembunyikan backdoor, atau membocorkan kredensial sesi ke server eksternal.
2. **Kerapuhan Jalur API Privat**: Karena menggunakan endpoint privat aplikasi mobile yang tidak didokumentasikan resmi oleh Gojek, setiap pembaruan arsitektur atau header pengaman pada backend Gojek dapat menyebabkan sistem mati seketika.
3. **Risiko Penutupan Akun Usaha**: Aktivitas polling transaksi dengan frekuensi tinggi dapat memicu deteksi bot anti-fraud internal Gojek yang berujung pada pembekuan saldo atau penutupan permanen akun merchant.
4. **Rekomendasi untuk Ekosistem Nara**:
   - Untuk lingkungan produksi utama [[Nara Topup]] dan [[Nara Premium]], tetap gunakan Midtrans QRIS demi kepastian webhook, stabilitas operasional, dan keamanan dana pelanggan.
   - Gateway mandiri dapat dipertimbangkan hanya sebagai bahan riset sekunder atau fallback terbatas di lingkungan pengujian mandiri, dengan syarat kode sumber berhasil dideobfuscate penuh dan diaudit keamanannya.

Kembali ke: [[Pengetahuan]] | [[Home]]
