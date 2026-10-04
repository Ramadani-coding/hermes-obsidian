# SOP Pengembangan MVP (Spec-Driven Development)

Standar Operasional Prosedur (SOP) tim untuk pembuatan proyek, micro-SaaS, dan fitur baru menggunakan metode **Spec-Driven Development (SDD)**. Tujuannya adalah mengunci kebutuhan bisnis, arsitektur teknis, dan acceptance criteria sebelum baris kode pertama ditulis, guna mencegah halusinasi AI, scope creep, dan kegagalan build di tengah jalan.

## Prinsip Utama
1. **Spec Dulu, Baru Kode**: Jangan pernah mulai coding tanpa dokumen spesifikasi yang disetujui.
2. **Acceptance Criteria Sebagai Kontrak**: Kriteria selesai wajib berbentuk *Given-When-Then* yang terukur dan dapat diverifikasi secara empiris.
3. **Pemisahan Peran Builder dan Verifier**: Agent atau proses yang menulis kode tidak boleh menilai sendiri kodenya tanpa uji verifikasi runtime/test yang objektif.

## Alur Pipeline SDD
1. **BRD (Business Requirements Document)**:
   - File: `docs/project/01-BRD.md`
   - Berisi latar belakang masalah, target pengguna, tujuan bisnis, batasan, serta daftar Epic dan Fitur (Must-Have vs Nice-to-Have).
2. **PEP (Project Execution Plan)**:
   - File: `docs/project/02-PEP.md`
   - Berisi milestone eksekusi, urutan pengerjaan fitur, strategi acceptance criteria, dan mitigasi risiko teknis.
3. **ITA (Information & Technical Architecture)**:
   - File: `docs/project/03-ITA.md`
   - Berisi pemilihan platform/stack, skema database, kontrak API endpoint, dan struktur direktori proyek.
4. **Spesifikasi Fitur (Feature Specs)**:
   - File: `docs/features/FEAT-<id>-<nama>.md`
   - Berisi rincian skenario pengujian dengan format *Given-When-Then*.
5. **Eksekusi & Pembuatan (Builder)**:
   - Implementasi kode murni mengacu pada file ITA dan Feature Specs. Dilarang menambah fitur liar di luar dokumen.
6. **Verifikasi Terukur (Verifier)**:
   - Uji runtime dan build otomatis. Status skenario wajib dilaporkan apa adanya (PASS, FAIL, atau UNVERIFIED).

## Template & Skill Lokal
- Lokasi starter template: `/home/ubuntu/templates/mvp-sdd-template`
- Skill Hermes: `spec-driven-development`

Kembali ke: [[Pengetahuan]] | [[Home]]