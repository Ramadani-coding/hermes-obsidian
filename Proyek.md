# Proyek

Daftar proyek dan tugas yang sedang aktif.

## Prioritas Tinggi (Selesai / Aktif)
- [x] **Mission Control AI Team MVP & 2D Visual Office**
  - [x] Backend HTTP Server (`http.server` stdlib, port 9000, read-only SQLite `kanban.db` & `state.db`)
  - [x] Frontend SPA dark mode 7 tab (Overview, Agents, 2D Visual Office, Task Board, Calendar, Activity, Memory)
  - [x] Visual Office 2D pixel-art (Workspace 0 & Lounge room switching, automatic real data state mapping)
  - [x] Integrasi OpenCode AI CLI terhubung ke 9Router `combo-nara`
- [x] **Hardening Keamanan Server VPS**
  - [x] Pindah port SSH ke `2222/tcp` dan set `PermitRootLogin no`
  - [x] Pasang dan aktifkan firewall `firewalld` (port terbuka: 2222, 20128)
  - [x] Pasang dan aktifkan `sshguard` untuk mitigasi serangan brute-force via nftables
  - [x] Selesaikan akses dashboard 9Router melalui firewall
  - Referensi teknis: [[Audit Keamanan VPS]]

## Eksplorasi & Proyek Lanjutan
- [ ] **Optimasi Model Routing & Fallback (9Router)**
  - Pengaturan Round Robin fallback Gemini Pro via Antigravity OAuth.
- [ ] **Pengembangan & Karakter Asisten Hermes**
  - Mengasah gaya percakapan [[Fern]] agar tetap natural, ringkas, dan bebas dari format robotik AI.

Kembali ke: [[Home]]