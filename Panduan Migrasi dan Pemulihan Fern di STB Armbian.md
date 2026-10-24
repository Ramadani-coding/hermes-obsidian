# Panduan Migrasi dan Pemulihan Fern di STB Armbian

Dokumentasi langkah demi langkah untuk memindahkan dan menghidupkan kembali asisten AI Fern (Hermes Agent) pada perangkat Set Top Box (STB) berbasis Linux Armbian (ARM64/aarch64).

---

## 1. Prasyarat di STB Armbian

Sebelum memulai, pastikan STB sudah terhubung ke jaringan internet dan paket pendukung dasar terpasang:

```bash
sudo apt update && sudo apt install -y curl git python3 python3-pip jq
```

Pastikan juga arsitektur sistem sesuai:
```bash
uname -m
# Biasanya 'aarch64' untuk prosesor ARM 64-bit
```

---

## 2. Instalasi Hermes Agent

Jalankan skrip instalasi resmi Hermes Agent untuk Linux ARM:

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

Setelah instalasi binary selesai, jalankan wizard awal Hermes:
```bash
hermes setup
```

---

## 3. Menyiapkan SSH Key & Kloning Ingatan dari GitHub

Semua ingatan, profil pengguna, dan kepribadian Fern disimpan di repositori privat GitHub Obsidian.

1. Buat SSH key baru di STB:
   ```bash
   ssh-keygen -t ed25519 -C "stb-armbian-fern"
   ```
2. Salin isi public key (`cat ~/.ssh/id_ed25519.pub`) dan tambahkan ke GitHub (**Settings -> SSH and GPG keys** atau sebagai Deploy Key di repo `hermes-obsidian`).
3. Clone repositori ke direktori home STB:
   ```bash
   git clone git@github.com:Ramadani-coding/hermes-obsidian.git ~/hermes-obsidian
   ```

---

## 4. Pemulihan Jiwa dan Memori Fern

Salin file identitas, preferensi pengguna, dan catatan permanen ke direktori kerja runtime Hermes:

```bash
# Pastikan folder memories Hermes sudah dibuat
mkdir -p ~/.hermes/memories

# Salin jiwa, profil pengguna, dan memori jangka panjang
cp ~/hermes-obsidian/"Ingatan Fern"/SOUL.md ~/.hermes/SOUL.md
cp ~/hermes-obsidian/"Ingatan Fern"/USER.md ~/.hermes/memories/USER.md
cp ~/hermes-obsidian/"Ingatan Fern"/MEMORY.md ~/.hermes/memories/MEMORY.md
```

Tiga file inti ini mengembalikan:
- `SOUL.md`: Kepribadian pragmatis, kuudere/tsundere santai, aturan tanpa em-dash, dan gaya chat Telegram kasual.
- `USER.md`: Profil Rama, hobi, bisnis Nara, preferensi kerja, dan aturan komunikasi.
- `MEMORY.md`: Catatan arsitektur teknis, aturan sistem, dan memori durable yang sudah terkumpul.

---

## 5. Konfigurasi Bot Telegram

Agar Fern bisa langsung merespons di Telegram seperti biasa, buat atau edit file konfigurasi lingkungan:

```bash
nano ~/.hermes/.env
```

Masukkan variabel Telegram berikut:
```env
TELEGRAM_BOT_TOKEN=isi_dengan_token_bot_telegram_kamu
TELEGRAM_ALLOWED_USERS=1733376699
TELEGRAM_HOME_CHANNEL=1733376699
```

---

## 6. Konfigurasi Model AI (Efisiensi Resource STB)

Mengingat STB memiliki spesifikasi RAM terbatas (biasanya 1 GB sampai 2 GB), disarankan untuk langsung menghubungkan Hermes ke provider model cloud (tanpa proxy lokal yang memakan RAM):

Buka file `~/.hermes/config.yaml` dan atur model provider:

```yaml
model:
  default: "anthropic/claude-3-5-sonnet" # atau google/gemini-2.0-flash
  provider: "openrouter"
```

Tambahkan API key penyedia model di `~/.hermes/.env`:
```env
OPENROUTER_API_KEY=sk-or-v1-...
```

*(Opsional)* Jika tetap ingin memakai 9Router lokal di STB:
```bash
npm install -g 9router
9router -H 127.0.0.1 -p 20128 --tray
```

---

## 7. Menjalankan Gateway dan Otomasi Systemd

Jalankan gateway messaging Hermes:
```bash
hermes gateway start
```

Untuk memastikan Fern otomatis hidup kembali saat STB menyala (booting):
```bash
systemctl --user enable hermes-gateway.service
systemctl --user start hermes-gateway.service
```

---

## 8. Verifikasi Keberhasilan

Buka obrolan Telegram dengan Fern dan kirim pesan sapaan santai. Fern akan langsung mengenali Rama, memanggil dengan panggilan biasa, dan melanjutkan konteks tanpa kehilangan identitas.
