Obsidian vault at ~/Documents/Obsidian Vault synced to hermes-obsidian.git via PAT. GitHub MCP connected. Dashboard at 127.0.0.1:9119.
§
Services on STB: 9Router (20128), Nara Premium (80/5001/6379), Catatan Fern (3050), cloudflared. Crons: Evaluasi 03:00 WITA (local), Threads spontan, Riset Loker & Auto-Draft 08:00 WITA (origin).
§
HF Serverless Inference: HF_TOKEN set in .env with inference permission. Image generation via black-forest-labs/FLUX.1-schnell works fast and reliably.
§
Antigravity integrates Upstash and Supabase MCP. Voice memos via Edge TTS id-ID-GadisNeural.
§
The Agency agents (282 specialists across 18 divisions) installed at ~/agency-agents, integrated via lazy-router plugin ~/.hermes/plugins/agency-agents-router (tools: agency_agents_search, agency_agents_inspect, agency_agents_load, agency_agents_delegate).
§
GitHub MCP: /usr/local/bin/github-mcp-server (v2.1.0 Go native, lazy: true, idle_timeout: 120s) with PAT in .env. Cloudflare Tunnel runs via systemd cloudflared.service. nara-appsV2 runs bare-metal (/root/projects/nara-appsV2, PM2 nara-backend :5001, Nginx :80 for /var/www/nara-store, local Redis).
§
Threads social (@momen_klip): via Zernio API (/root/scripts/threads_manager.py, posts + reply comments) & Buffer backup (/root/scripts/threads_buffer.py) with spontaneous cron. Research trends & real netizen phrasing; natural lowercase, un-AI-like.
§
Host timezone: Asia/Makassar (WITA, UTC+8). Communication rule: mirror casual human tone (register bundle, validasi suasana/ledek santai sebelum solusi teknis), buang istilah buku teks/akademis saat chat santai, proaktif simpan & terapkan temuan baru.
§
STB: 1.8GiB RAM, zram swap, 14GB microSD; avoid Docker/Vite builds. Nginx: nara-store (:80) & fern-journal (:3050). 9Router (20128) 5 Antigravity accounts. YouTube (@rammadan_code): /root/scripts/youtube_manager.py. Work email: ahmadramadani.work@gmail.com (/root/scripts/email_manager.py).