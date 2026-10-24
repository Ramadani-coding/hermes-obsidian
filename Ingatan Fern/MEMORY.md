Assistant persona is named Fern. Character: disciplined, truthful, pragmatic, polite, acting as a strict yet caring caretaker. Style: natural Indonesian casual texting style like close friends (match Rama's relaxed typing, never stiff like AI); strictly NO em-dashes ('—' or '--') and NO colons (':') for listing; NEVER end messages with reflexive polite AI assistant questions; deadpan and direct critique when Rama slacks off, but always completes tasks meticulously and accurately. Never provides unverified info. Always to the point unless asked for details.
§
Obsidian vault path: /home/ubuntu/Documents/Obsidian Vault, initialized as git repository linked to remote GitHub repo Ramadani-coding/hermes-obsidian.
§
Standing rule: only document genuinely important technical setups, system architecture, crucial guides, or explicitly requested topics into Obsidian vault (~/Documents/Obsidian Vault) and git push. Do NOT document casual Q&A or minor conversation topics.
§
Services: 9Router (20128), Warranty Hub (9050, link.herama.my.id), DualStackHTTPServer (IPv4/IPv6 '::' IPV6_V6ONLY=0). Profiles: default, leadengineer, nara-marketer. VPS sisa 6 hari (akhir Okt 2026, tdk diperpanjang), rencana migrasi ke PC lokal & STB Armbian. Cron marketing & ruang.service dipause; evaluasi harian 03:00 WITA wajib aktif.
§
Dashboard infra: running on port 9119 via Cloudflare Tunnel (service: cloudflared), public URL https://hermes.herama.my.id, auth via dashboard.basic_auth (username: cocode). Vault path is ~/Documents/Obsidian Vault synced with GitHub repo Ramadani-coding/hermes-obsidian.
§
VPS environment: Ubuntu 24.04, 2GB RAM (~500MB free). Local LLM (nara-combo) runs on 127.0.0.1:20128; OPENAI_API_KEY in .env is for local endpoint, not OpenAI cloud (STT requires cloud provider like Groq due to RAM constraints). fail2ban is installed and active on sshd.
§
User has created a Hugging Face Serverless Inference API setup. Hugging Face Access Token (`HF_TOKEN`) is set in `.env` with "Make calls to Inference Providers" permission. The `black-forest-labs/FLUX.1-schnell` model via HF inference works properly and generates images quickly.