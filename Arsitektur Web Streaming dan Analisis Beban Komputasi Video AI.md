# Arsitektur Web Streaming dan Analisis Beban Komputasi Video AI

Dokumentasi teknis mengenai dekonstruksi arsitektur situs web streaming film terdistribusi serta analisis kelayakan beban komputasi pemrosesan video AI pada infrastruktur VPS hemat daya.

## 1. Dekonstruksi Arsitektur Platform Streaming Film Web (Kasus MDFlix)

Platform streaming film web independen modern (seperti MDFlix) umumnya mengadopsi arsitektur terpisah (decoupled architecture) untuk meminimalisasi biaya infrastruktur dan menghindari risiko penalti hak cipta (DMCA). Sistem ini memisahkan secara mutlak antara lapisan metadata dan lapisan distribusi pemutaran video.

### A. Lapisan Metadata & Aset Visual (The Movie Database / TMDB)
- **Sumber Data Utama**: REST API resmi The Movie Database (`api.themoviedb.org/3`).
- **Data yang Diambil**: Judul karya, sinopsis multi-bahasa, genre ID, tanggal rilis, rating kritikus, jumlah vote, daftar pemeran (cast), serta relasi musim/episode serial TV.
- **Distribusi Aset Gambar**: Poster dan backdrop tidak di-host pada server aplikasi, melainkan langsung diarahkan ke CDN gambar TMDB (`image.tmdb.org/t/p/w500/` atau `w780/`).
- **Skema Penomoran Unik**: Setiap film atau serial memiliki identifikasi global berupa `tmdbId` dan `imdbId` yang menjadi kunci integrasi ke pemutar video.

### B. Lapisan Distribusi Video (Multi-Provider Iframe Embed Scraper)
Server aplikasi web tidak menyimpan berkas video (MP4/MKV/HLS) maupun menyiarkan bandwidth video secara mandiri. Penyajian video sepenuhnya memanfaatkan pemutar tersemat (`<iframe>`) dari jaringan aggregator/scraper pihak ketiga:
- **Penyedia Embed Utama**:
  - VidLink (`https://vidlink.pro/movie/{tmdbId}` dan `https://vidlink.pro/tv/{tmdbId}/{season}/{episode}`)
  - VidSrc (`https://vidsrc.to/embed/movie/{tmdbId}` dan `https://vidsrc.wiki/embed/movie/{tmdbId}`)
  - 2Embed (`https://www.2embed.cc/embed/{tmdbId}` dan `https://www.2embed.cc/embedtv/{tmdbId}&s={season}&e={episode}`)
  - MultiEmbed (`https://multiembed.mov/?video_id={tmdbId}&tmdb=1`)
  - VSEmbed (`https://vsembed.ru/embed/movie/{tmdbId}`)
- **Keuntungan Arsitektural**:
  1. *Zero Video Storage*: Server web hanya membutuhkan kapasitas penyimpanan kecil untuk basis kode Next.js/React (<500MB).
  2. *Zero Bandwidth Egress*: Beban lalu lintas data streaming berukuran gigabyte ditanggung sepenuhnya oleh server embed pihak ketiga dan CDN mereka.
  3. *Mitigasi DMCA*: Penyedia situs web beroperasi sebagai pengindeks konten dan agregator katalog, bukan hoster berkas media.

---

## 2. Analisis Beban Pemrosesan Video AI (Kasus Auto-Clipper / Cheat Clip PRO)

Aplikasi auto-clipper cerdas (seperti repositori `galihjuansaputra/cheat-clip-pro`) dirancang untuk memotong video durasi panjang (YouTube/Drive) menjadi konten klip vertikal pendek berasio 9:16 dengan subtitle animasi otomatis.

### A. Komponen Pipeline Pemrosesan
1. **Pengunduh Media & Ekstraksi Audio**: Menggunakan `yt-dlp` untuk mengunduh stream video/audio resolusi tinggi.
2. **Ekstraksi Momen Kunci (LLM Reasoning)**: Mengirimkan metadata atau transkrip video ke Google Gemini Flash API via Google AI Studio untuk mendeteksi segmen viral berbasis retensi audiens.
3. **Transkripsi Suara Presisi (Audio AI Engine)**: Menjalankan model `openai-whisper` (berbasis framework PyTorch) secara lokal untuk mengekstrak timestamp tingkat kata (word-level timestamps) untuk efek karaoke.
4. **Computer Vision & Tracking Wajah**: Menjalankan OpenCV (`opencv-python`) dengan algoritma pelacakan wajah (YuNet / Static Saliency Spectral Residual) guna menjaga posisi wajah pembicara tetap berada di titik tengah potongan vertikal 9:16.
5. **Video Slicing & Subtitle Rendering**: Menggunakan binary `ffmpeg` dengan filter subtitle `libass` untuk merender tipografi animasi dan mengekspor klip akhir.

### B. Evaluasi Kelayakan pada VPS Minimal (2GB RAM / 2 vCPU)
Mengeksekusi pipeline ini pada lingkungan VPS dengan spesifikasi 2GB RAM dan 2 virtual core CPU memiliki risiko kegagalan sistem yang sangat tinggi:
- **PyTorch & Whisper Memory Spike**: Model Whisper versi *base* atau *small* menuntut alokasi RAM aktif 1GB hingga 2GB+ saat memproses tensor audio. Pada VPS yang telah menjalankan bot Hermes, 9Router, dan daemon database, beban ini memicu *Out Of Memory* (OOM) Killer Linux yang dapat mematikan proses inti sistem.
- **Saturasi CPU & Throttling**: Transcoding video beresolusi 1080p bersamaan dengan komputasi matriks OpenCV menaikkan load average CPU hingga 100% dalam durasi panjang. Kondisi ini membuat server tidak responsif, meningkatkan latensi bot, dan berisiko terkena suspend oleh penyedia VPS akibat pemakaian CPU berlebihan terus-menerus.

### C. Keputusan Rekayasa & Offloading Beban Komputasi
Berdasarkan evaluasi stabilitas sistem, diputuskan aturan pemisahan beban (workload offloading):
1. **VPS Linux**: Dikhususkan secara ketat untuk daemon yang beroperasi hening dan hemat daya (Hermes Agent Gateway, 9Router AI Model Router, Cloudflare Tunnel, Telegram Bot, dan web API ringan).
2. **Workstation PC Lokal (Windows)**: Seluruh pemrosesan video berat, instalasi PyTorch, transkripsi Whisper lokal, dan rendering FFmpeg dialihkan ke PC lokal Rama yang dibekali perangkat keras mandiri (prosesor multi-core dan akselerasi GPU), terhindar dari batasan memori dan risiko OOM server.

---

Tautan Terkait:
- [[Pengetahuan]]
- [[Audit Keamanan VPS]]
- [[Home]]
