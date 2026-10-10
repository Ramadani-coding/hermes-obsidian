# Integrasi Codebase Knowledge Graph (Graphify) dan Antigravity

Dokumentasi arsitektur, instalasi, dan integrasi penjelajahan basis kode berbasis graf pengetahuan (Knowledge Graph) lokal menggunakan Graphify untuk asisten coding AI (Google Antigravity, Claude Code, Cursor, Codex) serta manajemen lingkungan pengembang di Windows.

## 1. Konsep Dasar Graphify

Graphify adalah perkakas pemetaan basis kode sumber terbuka yang mentransformasikan seluruh struktur proyek (kode program, konfigurasi, skema SQL, dan dokumentasi) menjadi graf pengetahuan relasional yang dapat ditelusuri (*traversable knowledge graph*).

### Mengapa Bukan RAG / Vector Store Biasa
- **Deterministik Lokal:** Pemetaan kode dijalankan menggunakan Tree-sitter AST parser secara murni lokal. Tidak memerlukan embeddings, tidak menggunakan vector database, dan tidak mengirim baris kode ke server eksternal.
- **Relasi Eksplisit:** Setiap simpul (*node*) merepresentasikan entitas nyata (fungsi, kelas, antarmuka, rute API, tabel basis data). Setiap garis hubung (*edge*) memiliki penanda kepastian:
  - `EXTRACTED`: Relasi eksplisit yang terbaca langsung dari pohon sintaks (impor modul, deklarasi tipe, pemanggilan metode).
  - `INFERRED`: Relasi yang disimpulkan melalui resolusi dependensi konteks.
- **Efisiensi Token Konteks:** Asisten AI tidak perlu membaca puluhan berkas secara mentah saat menelusuri alur fitur. AI cukup melakukan kueri jalur (*path tracing*) atau inspeksi tetangga pada graf, memangkas halusinasi dan konsumsi token secara drastis.

### Berkas Keluaran (`graphify-out/`)
- `graph.html`: Visualisasi graf interaktif berbasis browser (D3 force-directed) lengkap dengan kluster komunitas, fitur pencarian simpul, dan pemfilteran hierarki.
- `GRAPH_REPORT.md`: Ringkasan modul utama, metrik konektivitas sentral, dan rekomendasi pertanyaan arsitektural.
- `graph.json`: Berkas basis data graf berformat JSON yang menyimpan seluruh topologi kode untuk kueri cepat.

---

## 2. Pemasangan dan Integrasi Antigravity

Graphify mendukung integrasi langsung ke sistem aturan (*rules*) dan alur kerja (*workflows*) Google Antigravity.

### Instalasi CLI
Pada lingkungan Python:
```bash
pip install graphifyy
# atau menggunakan uv:
uv tool install graphifyy
```

### Registrasi Skill ke Proyek Antigravity
Jalankan perintah berikut di direktori utama (*root*) repositori proyek:
```bash
python -m graphify antigravity install
# atau jika path terdaftar global:
graphify antigravity install
```
Perintah ini otomatis mengonfigurasi direktori `.agents/`:
- `.agents/rules/graphify.md`: Aturan agar AI memprioritaskan kueri graf sebelum membaca berkas secara sporadis.
- `.agents/workflows/graphify.md`: Alur kerja standar penelusuran graf saat analisis kode.
- `.agents/skills/graphify/SKILL.md`: Definisi kapabilitas perkakas graf untuk asisten.

### Pemindaian Kode Tanpa Ketergantungan API LLM
Untuk memindai kode tanpa memerlukan API key pihak ketiga (murni AST lokal):
```bash
python -m graphify . --code-only
```
Opsi `--code-only` melewatkan dokumen non-kode (PDF/gambar/markdown panjang) yang membutuhkan pemrosesan semantik model, sehingga proses berjalan cepat, gratis, dan sepenuhnya offline.

### Kueri Langsung Melalui Terminal
- Menjelaskan simpul tertentu:
  ```bash
  python -m graphify explain "NamaFungsiAtauClass"
  ```
- Menemukan jalur terpendek antar-dua komponen:
  ```bash
  python -m graphify path "ModulA" "ModulB"
  ```

---

## 3. Catatan Lingkungan Windows dan Kompatibilitas

Dalam implementasi di lingkungan workstation Windows (khususnya yang menggunakan Git Bash/MINGW64):

### Penanganan PATH Eksekusi Python
Jika perintah `graphify` mengembalikan pesan galat `command not found`, penyebab utamanya adalah folder `Scripts` Python (misalnya `AppData/Local/Programs/Python/Python314/Scripts`) belum ditambahkan ke variabel sistem `PATH`.
- Solusi instan tanpa manipulasi PATH: Eksekusi perkakas selalu menggunakan modul bawaan Python:
  ```bash
  python -m graphify <subcommand>
  ```

### Konflik Docker Desktop / Hyper-V dengan Riot Vanguard (Valorant)
Pada komputer pengembang yang juga digunakan untuk bermain game kompetitif seperti Valorant, proteksi anti-cheat tingkat kernel (Riot Vanguard) sering kali menolak berjalan saat fitur virtualisasi Windows (`Virtual Machine Platform` atau `Hyper-V`) diaktifkan untuk kebutuhan Docker Desktop atau WSL2.

#### Opsi Mitigasi 1: Toggle Hypervisor via BCDEDIT
Pengembang tidak perlu melakukan bongkar-pasang fitur Windows secara manual. Cukup gunakan saklar konfigurasi bootloader melalui Command Prompt (Run as Administrator):
- **Saat Ingin Bermain Game (Menonaktifkan Hypervisor):**
  ```cmd
  bcdedit /set hypervisorlaunchtype off
  ```
  *(Memerlukan restart sistem. Driver Riot Vanguard akan berjalan lancar).*
- **Saat Ingin Bekerja / Mengembangkan Proyek (Mengaktifkan Hypervisor):**
  ```cmd
  bcdedit /set hypervisorlaunchtype auto
  ```
  *(Memerlukan restart sistem. Docker Desktop dan WSL2 dapat kembali aktif).*

#### Opsi Mitigasi 2: Menghindari Docker Lokal (Arsitektur Ramping)
- **Eksekusi MCP Berbasis Node.js:** Untuk server Model Context Protocol seperti GitHub MCP, gunakan runner langsung berbasis `npx` daripada kontainer Docker:
  ```json
  "github-mcp-server": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-github"],
    "env": {
      "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxxxxxxxxxx"
    }
  }
  ```
- **Offloading Kontainer ke VPS Ubuntu:** Jalankan kontainer Docker pada server VPS Linux jarak jauh via SSH remote context, menjaga PC workstation lokal tetap bersih tanpa beban Hyper-V.

---

Kembali ke: [[Pengetahuan]] | [[Home]]