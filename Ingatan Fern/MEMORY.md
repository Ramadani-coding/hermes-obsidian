Obsidian vault at ~/Documents/Obsidian Vault is synced to Ramadani-coding/hermes-obsidian.git via PAT token. GitHub MCP server (go binary, lazy mode) connected. Hermes dashboard at 127.0.0.1:9119.
§
Services on STB Armbian: 9Router (20128), Nara Premium (Nginx 80, PM2 5001, Redis 6379), Catatan Fern static journal (nara.herama.my.id, Nginx 3050/80, /root/scripts/build_journal.py), cloudflared tunnel. Evaluasi harian 03:00 WITA aktif (deliver local).
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
STB host: 1.8GiB RAM, zram swap, 14GB microSD only (no HDD); avoid Docker/Vite builds or headless Chromium. Nginx: nara-store (:80) & fern-journal (:3050) require default_server + server_name. 9Router (192.168.1.20:20128) pools 5 Antigravity OAuth accounts.