# Ruang (Hermes 3D & 2D Virtual Office)

Dokumentasi arsitektur, instalasi, dan panduan operasional **Ruang** (`yugienugraha/ruang`) sebagai platform 3D Virtual Office & Read-Only Mission Control untuk tim agen Hermes dan OpenCode.

---

## 1. Ikhtisar & Arsitektur
**Ruang** adalah dashboard pemantau kru agen AI lokal yang menyediakan antarmuka visual kantor virtual 3D (Three.js & React Three Fiber) dan 2D pixel-art.

### Karakteristik Sistem
- **Read-Only**: Seluruh data (kru agen, status aktif, kanban board, cron calendar, memori SOUL/MEMORY, log) dibaca secara realtime via `hermes` CLI tanpa memodifikasi database internal.
- **Efisiensi Sumber Daya**: Backend Express 5 berjalan di Node.js (hanya memakan ~21MB RAM).
- **Nuansa Lokal Indonesia**: Tampilan visual 3D mencakup ruang kerja, meja meeting gorengan, galon air minum, gerobak bakso, sepeda kopi keliling, bendera Merah Putih, serta lantai dua (ruko) untuk tempat tidur agen idle.

---

## 2. Status Layanan di VPS
- **Systemd Service**: `systemctl --user status ruang`
- **Port Internal**: `3001` (bind ke `127.0.0.1`)
- **Lokasi Instalasi**: `~/.local/share/ruang`
- **Executable**: `~/.local/bin/ruang`
- **Konfigurasi Autentikasi**: Hash scrypt di `~/.config/ruang/access.json` (opsional)

---

## 3. Cara Mengakses

### Via Termius / SSH Port Forwarding (Lokal Laptop/HP)
Karena aplikasi berjalan di localhost server (`127.0.0.1:3001`) demi keamanan dari bot internet:
1. Hubungkan koneksi SSH ke VPS di Termius (Port: `2222`).
2. Buat aturan **Port Forwarding**:
   - **Local Port**: `3001`
   - **Destination**: `127.0.0.1`
   - **Destination Port**: `3001`
3. Buka browser di perangkat Anda:
   `http://localhost:3001` atau `http://127.0.0.1:3001`

### Perintah Manajemen Layanan
```bash
# Cek status
systemctl --user status ruang

# Restart layanan
systemctl --user restart ruang

# Matikan layanan
systemctl --user stop ruang

# Reset / generate access code baru
ruang access-code new
```

---

## 4. Troubleshooting Akses & Port Forwarding

### Kendala `ERR_CONNECTION_RESET` di Browser
Jika browser menampilkan pesan koneksi direset saat membuka `http://localhost:3001`:
1. **Periksa Protokol URL**: Pastikan URL diketik lengkap dengan `http://` bukan `https://`. Browser modern sering kali memaksakan HTTPS secara otomatis, yang menyebabkan reset koneksi seketika saat diterima oleh port HTTP biasa.
2. **Status Tunnel Termius**: Pastikan aturan forwarding di Termius sudah diaktifkan (toggle switch berwarna hijau/aktif). Menyimpan formulir tanpa mengaktifkan tunnel tidak akan membuka port lokal.
3. **Verifikasi Host Intermediate**: Pastikan aturan port forwarding terhubung ke host server yang tepat (`IP: 43.133.158.228`, `Port: 2222`), bukan server lain yang tersimpan di Termius.
4. **Gunakan Binding 127.0.0.1**: Jika `localhost` gagal di-resolve karena resolusi IPv6 `::1`, gunakan alamat eksplisit `http://127.0.0.1:3001`.

---

Kembali ke: [[Proyek]] | [[Pengetahuan]] | [[Home]]
