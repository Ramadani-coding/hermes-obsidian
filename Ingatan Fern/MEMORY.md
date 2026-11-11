Obsidian vault at ~/Documents/Obsidian Vault is synced to Ramadani-coding/hermes-obsidian.git via PAT token. GitHub MCP server (go binary, lazy mode) connected. Hermes dashboard at 127.0.0.1:9119.
§
Services on STB Armbian: 9Router (20128), Nara Premium bare-metal (Nginx port 80, backend PM2 port 5001, Redis 6379, cloudflared tunnel). Evaluasi harian 03:00 WITA aktif (deliver local).
§
HF Serverless Inference: HF_TOKEN set in .env with inference permission. Image generation via black-forest-labs/FLUX.1-schnell works fast and reliably.
§
Antigravity integrates Upstash and Supabase MCP. Voice memos via Edge TTS id-ID-GadisNeural.
§
The Agency agents (282 specialists across 18 divisions) installed at ~/agency-agents, integrated via lazy-router plugin ~/.hermes/plugins/agency-agents-router (tools: agency_agents_search, agency_agents_inspect, agency_agents_load, agency_agents_delegate).
§
GitHub MCP Server (v2.1.0 Go binary at /usr/local/bin/github-mcp-server) configured in config.yaml (stdio, lazy: true, idle_timeout_seconds: 120, token in .env). Full repo access via mcp__github_* tools.
§
GitHub MCP: /usr/local/bin/github-mcp-server (v2.1.0 Go native, lazy: true, idle_timeout: 120s) with PAT in .env. Cloudflare Tunnel runs via systemd cloudflared.service. nara-appsV2 runs bare-metal (/root/projects/nara-appsV2, PM2 nara-backend :5001, Nginx :80 for /var/www/nara-store, local Redis).