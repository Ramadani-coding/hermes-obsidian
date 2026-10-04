# Google Stitch MCP Integration

Dokumentasi integrasi **Google Stitch** via Model Context Protocol (MCP) pada Hermes Agent dan Antigravity.

---

## 1. Ikhtisar
**Google Stitch** (`https://stitch.withgoogle.com`) adalah platform prototyping antarmuka pengguna (UI/UX) berbasis AI dari Google Labs yang ditenagai oleh **Gemini 3 Pro** dan **Gemini 3 Flash**. 

Melalui protokol MCP, asisten coding dan agen AI dapat langsung:
1. Membuat dan mengelola proyek desain UI.
2. Menghasilkan antarmuka aplikasi/web (*screens*) secara otomatis dari teks prompt bahasa natural.
3. Mengekstrak *Design DNA* (palet warna, tipografi, token desain `DESIGN.md`).
4. Mengambil kode komponen siap pakai (HTML/CSS dan React Tailwind) langsung ke repositori lokal.

---

## 2. Konfigurasi MCP di Hermes Agent

### Endpoint Resmi
- **Transport:** HTTP / StreamableHTTP
- **URL:** `https://stitch.googleapis.com/mcp`
- **Header Autentikasi:** `X-Goog-Api-Key: <STITCH_API_KEY>`

### Konfigurasi `~/.hermes/config.yaml`
```yaml
mcp_servers:
  stitch:
    url: "https://stitch.googleapis.com/mcp"
    headers:
      X-Goog-Api-Key: "[REDACTED]"
    timeout: 180
    connect_timeout: 60
```

### Verifikasi Koneksi
```bash
hermes mcp test stitch
```

Output:
- Status: `✓ Connected`
- Tools discovered: **15 tools** (`create_project`, `get_project`, `delete_project`, `list_projects`, `list_screens`, `get_screen`, `generate_screen_from_text`, `edit_screens`, `generate_variants`, `upload_design_md`, `create_design_system`, `create_design_system_from_design_md`, `update_design_system`, `list_design_systems`, `apply_design_system`).

---

## 3. CLI Client Lokal
Tersedia client eksekusi mandiri di VPS:
`/home/ubuntu/.hermes/scripts/stitch_client.py`

Contoh perintah:
```bash
# Cek daftar project
/home/ubuntu/.hermes/scripts/stitch_client.py list-projects

# Buat project baru
/home/ubuntu/.hermes/scripts/stitch_client.py create-project --name "Nara Topup UI"

# Generate tampilan baru dengan Gemini 3 Pro
/home/ubuntu/.hermes/scripts/stitch_client.py generate-screen --project "<PROJECT_ID>" --prompt "Modern minimalist crypto topup checkout with QRIS"
```

---

## 4. Integrasi dengan Antigravity IDE (Laptop User)
1. Buka **Agent Manager** (tekan `Ctrl + E`).
2. Masuk ke menu **MCP Servers** -> cari **Stitch**.
3. Masukkan Stitch API Key.
4. Di chat Antigravity, panggil: `"Use Stitch MCP to fetch design tokens and scaffold the frontend components"`.
