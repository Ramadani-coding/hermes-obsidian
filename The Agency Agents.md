# The Agency Agents (282 Spesialis AI)

Dokumentasi integrasi repositori [The Agency](https://github.com/msitarzewski/agency-agents) ke dalam ekosistem Hermes Agent.

## Ringkasan Integrasi
- **Total Spesialis**: 282 agen AI lintas 18 divisi (Engineering, Design, Marketing, Product, Security, Support, dll).
- **Arsitektur Integrasi**: Menggunakan plugin native **Lazy Router** (`agency-agents-router`) yang dipasang di `~/.hermes/plugins/agency-agents-router/`.
- **Lokasi Repositori**: `/home/ubuntu/agency-agents`
- **Efisiensi Token**: Plugin menyimpan seluruh metadata dan prompt agen di file JSON lokal (`data/agents.json`). Hermes tidak memuat seluruh agen ke context window awal (menghindari boros token dan context pollution), melainkan mencari dan memuat spesialis secara *on-demand*.

## Pembagian Divisi Agen
- **Engineering** (65 agen): Frontend, Backend, API Platform, Database Optimizer, SRE, Mobile, DevOps, dll.
- **Design** (10 agen): UI Designer, UX Architect, Design System Lead, dll.
- **Marketing & Paid Media** (44 agen): Content Strategist, Copywriter, SEO, Paid Ads, Social Media, dll.
- **Specialized** (59 agen): Peran-peran unik, automasi tingkat lanjut, dan workflow niche.
- **Security & Testing** (21 agen): Penetration Tester, Security Auditor, QA Automation, Performance Tester.
- **Divisi Lain**: Product (6), Project Management (7), Sales (9), Support (6), Game Development (21), GIS (13), Academic (6), Finance (5), Healthcare (3), Spatial Computing (6), Research (1).

## Tool yang Disediakan Hermes Plugin
1. `agency_agents_search`: Mencari agen spesialis berdasarkan query atau divisi tertentu.
2. `agency_agents_inspect`: Memeriksa detail identitas, persona, dan deliverable agen tertentu.
3. `agency_agents_load`: Memuat context dan standar kerja agen ke sesi aktif saat ini.
4. `agency_agents_delegate`: Mendelegasikan tugas ke subagent terisolasi (`delegate_task`) menggunakan persona spesialis Agency yang dipilih.

## Perintah Instalasi & Pembaruan
```bash
cd /home/ubuntu/agency-agents
git pull
./scripts/convert.sh --tool hermes
./scripts/install.sh --tool hermes
systemctl --user restart hermes-gateway.service
```
