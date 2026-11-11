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
Threads social (@momen_klip): via Buffer GraphQL API (/root/scripts/threads_buffer.py). Always research trends & real Indonesian netizen phrasing before drafting; keep posts natural, lowercase, un-AI-like.