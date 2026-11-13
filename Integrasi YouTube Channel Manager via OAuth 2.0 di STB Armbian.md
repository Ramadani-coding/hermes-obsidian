# Integrasi YouTube Channel Manager via OAuth 2.0 di STB Armbian

Integrasi pengelolaan channel YouTube Rama (`@rammadan_code` / ID `UCegjQdF4jqXzr0hVdo9_lew`) langsung dari STB Armbian tanpa dependensi pihak ketiga (`0 MB` runtime idle, pure Python stdlib).

## Arsitektur & Jalur Autentikasi

1. **Google Cloud Console OAuth 2.0**
   - Project: `resolute-land-281302`
   - Client Type: Desktop app (Installed application)
   - Scope:
     - `https://www.googleapis.com/auth/youtube`
     - `https://www.googleapis.com/auth/youtube.force-ssl`
   - Kredensial tersimpan di: `/root/.hermes/youtube_credentials.json` (chmod 600)
   - Token & refresh token tersimpan di: `/root/.hermes/youtube_token.json` (chmod 600)

2. **Eksekutor Python Stdlib (`/root/scripts/youtube_manager.py`)**
   - Dirancang tanpa package `google-api-python-client` untuk menjaga konsumsi RAM STB tetap minimal.
   - Menggunakan modul bawaan `urllib.request`, `urllib.parse`, `json`, dan `time`.
   - Mengimplementasikan auto-refresh access token saat token kadaluarsa (toleransi 5 menit sebelum expire).

## Kemampuan & Fitur Operasional

- **Cek Profil & Statistik Channel**: Mengambil nama channel, custom handle, total subscriber, total video, dan view count.
- **Manajemen Playlist**:
  - Membaca seluruh playlist (public, unlisted, private).
  - Membuat playlist baru (`--title`, `--description`, `--privacy`).
  - Memasukkan video ke playlist tertentu (`--playlist-id`, `--video-id`, `--position`).
- **Pembaruan Metadata Video**:
  - Memperbarui judul video (`--title`).
  - Memperbarui deskripsi video (`--description`).
  - Memperbarui daftar tag video (`--tags "tag1, tag2"`).

## Perintah CLI

```bash
# Cek channel
python3 /root/scripts/youtube_manager.py check

# List seluruh playlist
python3 /root/scripts/youtube_manager.py playlists

# Buat playlist baru
python3 /root/scripts/youtube_manager.py create-playlist --title "Live Streaming" --privacy public

# Tambah video ke playlist
python3 /root/scripts/youtube_manager.py add-to-playlist --playlist-id "PLAYLIST_ID" --video-id "VIDEO_ID"

# Update judul dan deskripsi video
python3 /root/scripts/youtube_manager.py update-video --video-id "VIDEO_ID" --title "Judul Baru" --description "Deskripsi Baru"
```
