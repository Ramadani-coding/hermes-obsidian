# Integrasi Email Lamaran Kerja di STB Armbian

Integrasi manajemen akun email profesional Rama (`ahmadramadani.work@gmail.com`) langsung dari STB Armbian untuk keperluan lamaran kerja, penerimaan panggilan interview, dan penyusunan draf surat lamaran/CV.

## Arsitektur & Jalur Koneksi

1. **IMAP SSL (Port 993 - Port Terbuka)**:
   - Digunakan untuk membaca inbox, mencari email masuk, memeriksa notifikasi keamanan/interview, dan membuat draf email langsung ke folder `[Gmail]/Draf`.
   - Menggunakan App Password akun Google (`khkk ctgm zdes zosd`) yang tersimpan aman di `/root/.hermes/email_config.json` (chmod 600).

2. **Pengiriman Email via Gmail API (Port 443 HTTPS)**:
   - Mayoritas ISP rumahan di Indonesia (IndiHome/Telkom) memblokir port outbound SMTP (25, 465, 587) untuk mencegah spam botnet.
   - Solusi: Pengiriman email langsung dilakukan melalui endpoint resmi Gmail API (`https://gmail.googleapis.com/gmail/v1/users/me/messages/send`) melalui port 443 HTTPS menggunakan OAuth 2.0.

3. **Skrip Eksekutor Python Stdlib (`/root/scripts/email_manager.py`)**:
   - Zero-dependency (tanpa paket berat, 0 MB runtime idle).
   - Mendukung fitur: `inbox`, `read <id>`, `search <query>`, `draft`, dan `send`.

## Perintah CLI

```bash
# Cek 5 email terbaru di inbox
/root/scripts/email_manager.py inbox --limit 5

# Baca isi email berdasarkan ID
/root/scripts/email_manager.py read 4

# Cari email dari HRD atau panggilan kerja
/root/scripts/email_manager.py search "interview"

# Buat draf surat lamaran kerja ke Gmail Drafts
/root/scripts/email_manager.py draft \
  --to "hrd@perusahaan.com" \
  --subject "Lamaran Pekerjaan - Frontend Developer - Ahmad Ramadani" \
  --body "Yth. HRD..."
```
