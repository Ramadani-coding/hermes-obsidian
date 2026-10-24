# Rencana dan Prosedur Migrasi Hermes ke PC Lokal

Dokumentasi arsitektur dan panduan transisi pemindahan seluruh ekosistem Hermes Agent, basis data, dan proyek digital dari peladen VPS Ubuntu ke lingkungan lokal PC Windows Rama menjelang berakhirnya masa sewa peladen.

## 1. Latar Belakang & Rasional Strategis

Per 20 Oktober 2026, masa sewa peladen VPS Ubuntu (spek 2 vCPU, RAM 2 GB) memasuki 10 hari terakhir dan diputuskan untuk tidak diperpanjang. Keputusan ini didasarkan pada pertimbangan efisiensi dan peningkatan kapabilitas komputasi:
- **Efisiensi Finansial**: Menghilangkan beban pengeluaran biaya langganan sewa VPS bulanan secara permanen.
- **Peningkatan Sumber Daya Komputasi**: PC kerja Rama dilengkapi prosesor bertenaga, kapasitas RAM yang jauh lebih longgar dibanding batasan 2 GB di VPS, serta kartu grafis terdedikasi NVIDIA GeForce GTX 1050 Ti. Hal ini membuka fleksibilitas untuk menjalankan beban komputasi berat (model AI lokal, PyTorch, Whisper, pemrosesan video, dan otomasi) tanpa risiko Out-Of-Memory (OOM).
- **Keamanan & Kedaulatan Data**: Seluruh data operasional, basis data sesi, dan token konfigurasi tersimpan secara privat di perangkat fisik milik sendiri.

---

## 2. Inventaris Komponen Sistem

Komponen yang wajib diamankan dan dipindahkan secara sistematis mencakup:

### A. Inti Hermes Agent (`~/.hermes`)
1. **Identitas & Karakter**: `~/.hermes/SOUL.md` (jiwa dan aturan interaksi Fern).
2. **Memori Permanen**: `~/.hermes/memories/USER.md` (profil Rama) dan `~/.hermes/memories/MEMORY.md` (catatan teknis permanen).
3. **Konfigurasi & Kredensial**: `~/.hermes/config.yaml`, `~/.hermes/.env` (kunci API 9Router, Hugging Face, Telegram bot token).
4. **Basis Data & State**:
   - `~/.hermes/state.db`: Seluruh riwayat percakapan sesi Telegram, cron, dan indeks pencarian FTS5.
   - `~/.hermes/kanban.db`: Papan kanban status penugasan agen visual.
5. **Ekstensi & Kustomisasi**: `~/.hermes/skills/` (seluruh modul keahlian terpasang) dan `~/.hermes/plugins/` (plugin router The Agency agents).

### B. Brankas Pengetahuan (Obsidian Vault)
- Lokasi: `/home/ubuntu/Documents/Obsidian Vault`
- Status Sinkronisasi: Terhubung langsung via SSH Deploy Key ke repositori GitHub `Ramadani-coding/hermes-obsidian`.

### C. Ekosistem Proyek Digital
1. **Nara Chronicle** (`/home/ubuntu/nara-chronicle`): Portal berita & refleksi harian (Next.js 15, Prisma ORM, Supabase Cloud PostgreSQL).
2. **Nara Topup** (`/home/ubuntu/nara-topup`): Platform top-up game instan (Next.js 15, Supabase, Midtrans QRIS).
3. **Warranty Quick-Hub** (`/home/ubuntu/warranty-hub`): Portal garansi promotor (Python dual-stack HTTP server, SQLite `warranty.db`).

---

## 3. Prosedur Pencadangan (Backup) di VPS

Sebelum masa sewa berakhir, eksekusi prosedur pencadangan berikut dari terminal VPS:

### A. Pembuatan Arsip Terpadu Hermes
```bash
# Buat arsip kompresi direktori konfigurasi dan basis data Hermes
tar --exclude='cache' \
    --exclude='audio_cache' \
    --exclude='image_cache' \
    --exclude='sandboxes' \
    -czvf ~/hermes_backup_complete.tar.gz -C /home/ubuntu .hermes
```

### B. Sinkronisasi Git Repositori Proyek
Pastikan semua perubahan lokal di setiap repositori telah di-commit dan di-push:
```bash
# 1. Obsidian Vault
cd "/home/ubuntu/Documents/Obsidian Vault"
git add .
git commit -m "chore: final sync before PC migration"
git push origin main

# 2. Nara Chronicle
cd /home/ubuntu/nara-chronicle
git push origin main

# 3. Warranty Hub
cd /home/ubuntu/warranty-hub
tar -czvf ~/warranty_hub_backup.tar.gz .
```

### C. Pengunduhan Berkas Arsip ke PC Lokal
Dari terminal PowerShell di PC Windows Rama, unduh berkas arsip menggunakan SCP:
```powershell
scp -P 2222 ubuntu@<IP_VPS>:~/hermes_backup_complete.tar.gz C:\HermesBackup\
scp -P 2222 ubuntu@<IP_VPS>:~/warranty_hub_backup.tar.gz C:\HermesBackup\
```

---

## 4. Prosedur Pemulihan (Restoration) di PC Windows

Direkomendasikan menggunakan lingkungan **WSL2 (Windows Subsystem for Linux - Ubuntu 24.04 LTS)** agar seluruh skrip otomasi Linux, daemon systemd, dan dependensi Python/Node.js berjalan identik tanpa kendala pathing Windows.

### A. Ekstraksi Konfigurasi di WSL2
```bash
# Pastikan direktori tujuan siap
mkdir -p ~/.hermes
tar -xzvf /mnt/c/HermesBackup/hermes_backup_complete.tar.gz -C ~/

# Verifikasi integritas memori dan database
ls -la ~/.hermes/memories/
sqlite3 ~/.hermes/state.db "SELECT count(*) FROM messages;"
```

### B. Penyiapan Dependensi & Lingkungan Eksekusi
1. Pasang Python 3.11/3.12 dan pip:
   ```bash
   sudo apt update && sudo apt install -y python3 python3-pip python3-venv git jq curl
   ```
2. Pasang Node.js LTS (v20+):
   ```bash
   curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
   sudo apt install -y nodejs
   ```
3. Pasang kembali Hermes Agent CLI via pip atau installer resmi.

### C. Konfigurasi Jaringan & Cloudflare Tunnel
Agar webhook Telegram dan akses dashboard/web publik (`hermes.herama.my.id`, `link.herama.my.id`) tetap terhubung tanpa IP publik statis:
1. Pasang `cloudflared` di WSL2 atau Windows Service.
2. Login menggunakan akun Cloudflare: `cloudflared tunnel login`.
3. Arahkan ingress traffic domain ke port layanan lokal PC (misal: 127.0.0.1:9119 untuk Hermes Dashboard, 127.0.0.1:9050 untuk Warranty Hub).

---

## 5. Penataan Operasional Selama Masa Transisi

Selama sisa 10 hari di VPS:
- **Layanan Dipause**: Seluruh cron promosi Nara Marketing, kurasi berita AI harian, dan daemon kantor 3D `ruang.service` dinonaktifkan sementara untuk menjaga ketenangan server dan menghemat kuota API.
- **Layanan Wajib Aktif**: Rutinitas evaluasi malam harian jam 03:00 WITA (`208923cdca2e`) tetap berjalan wajib setiap subuh guna menjaga kontinuitas ingatan, sinkronisasi jiwa ke GitHub, dan pencatatan jurnal refleksi.
- **Kesiapan Perekrutan Ulang**: Agen-agen spesialis (Nara Marketer, Lead Engineer, The Agency) akan direkrut dan diaktifkan kembali begitu kantor baru di PC lokal Rama telah beroperasi secara stabil.

Kembali ke: [[Pengetahuan]]
