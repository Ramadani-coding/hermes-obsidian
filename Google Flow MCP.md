# Google Flow MCP Integration

Dokumentasi integrasi **Google Flow** via Model Context Protocol (MCP) pada Hermes Agent.

---

## 1. Ikhtisar
**Google Flow** (`https://flow.google.com`) adalah AI Creative Studio dari Google yang menggabungkan model video generatif mutakhir (Veo 3.1) dan Gemini Omni.

Fitur utama yang bisa diakses via MCP:
1. Pembuatan video sinematik dari teks prompt (*text-to-video*)
2. Pembuatan video dengan gambar referensi (*image-to-video* dan multi-reference consistency)
3. Ekstensi video dan interpolasi frame (*extend video*, *first-to-end frame*)
4. Flow Creation Agent chat interaktif untuk perencanaan dan pengeditan adegan

---

## 2. Konfigurasi MCP di Hermes Agent

### Endpoint Resmi
- Transport: HTTP / StreamableHTTP
- URL: `https://flow.googleapis.com/mcp`
- Header Autentikasi: `X-Goog-Api-Key: <GOOGLE_API_KEY>`

### Konfigurasi `~/.hermes/config.yaml`
```yaml
mcp_servers:
  flow:
    url: "https://flow.googleapis.com/mcp"
    headers:
      X-Goog-Api-Key: "<GOOGLE_API_KEY>"
    timeout: 180
    connect_timeout: 60
```

### Verifikasi Tool
Tools yang terdaftar via protokol MCP:
- `stream_chat` (Flow Creation Agent untuk chat, planning, direct image/video generation, dan extension)
- `batch_async_generate_video_text` (Generate video langsung dari teks prompt)
- `batch_async_generate_video_reference_images` (Generate video dengan aset referensi visual agar karakter/produk konsisten)
- `batch_check_async_video_generation_status` (Monitoring status render video, sisa kredit, dan link unduhan video)

---

## 3. CLI Client Lokal
Helper script mandiri tersedia di:
`/home/ubuntu/.hermes/scripts/flow_client.py`

Contoh perintah:
```bash
# Cek daftar tool Flow MCP
/home/ubuntu/.hermes/scripts/flow_client.py list-tools

# Panggil tool Flow
/home/ubuntu/.hermes/scripts/flow_client.py call-tool stream_chat --args '{"userMessage": {"userPrompt": {"parts": [{"text": "Create a 5-second cinematic promo video for digital store"}]}}}'
```
