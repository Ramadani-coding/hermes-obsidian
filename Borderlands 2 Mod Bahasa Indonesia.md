---
created: 2026-11-16
status: wave1-selesai
engine: Unreal Engine 3 (plain text .int di WillowGame/Localization/INT/)
---

# Proyek Mod Bahasa Indonesia Borderlands 2

## Riset Gaya Penulisan (2026-11-16)
- Nada: sinis, tongue-in-cheek, dark humor. Story utama serius (tonal touchstone Firefly), side quest surreal/gila.
- Karakter punya voice unik: Handsome Jack (sombong/mock), Tiny Tina (bocil random + brutal), Mr. Torgue (CAPS, obsessed ledakan), Scooter (redneck), Ellie (kasar), Tannis (awkward), Marcus (serakah).
- Banyak referensi pop culture/meme + joke random. Insult khas: jackhole, craphole, suckers, bitch.
- Kritik penulisan: "funniest, cleverest script since Portal 2" (pinsof via wiki).

## SOP Terjemahan
1. Register per karakter, bukan satu gaya untuk semua. Jack pakai kamu/lo merendahkan; Tina bahasa anak kecil; Torgue berteriak.
2. Joke: adapt ke setaraan Indonesia kalau ada yang pas, biarin kalau nggak ada. Jangan harfiah jadi aneh.
3. Nama diri/tempat/senjata/item tetap asli.
4. Placeholder {var}, newline, tag formatting dijaga verbatim.
5. Kalimat pendek, langsung, natural kayak ngomong. Hindari kata formal (hamba, mohon, dimohonkan, tersayang).
6. Tidak ada fan translation Indonesia resmi untuk BL2 — proyek dari nol.
7. Pipeline: batch 40-50 string/prompt, cache.json indexed, thread pool kalau proxy router aktif, "stream": false di payload.

## Prioritas Pengerjaan (request Rama 2026-11-16)
1. Dialog cerita utama + UI/menu dulu (core).
2. Side quest, item, skill menyusul per gelombang (rilis bertahap).
3. Batch nyimpen ke cache.json tiap selesai — proses putus bisa lanjut, nggak ulang dari awal.

## Progress
- File INT.zip dari Rama diterima (202 file, UTF-16 LE + BOM + CRLF, format INI UE3).
- Pipeline: /root/projects/bl2-id/pipeline.py (extract → translate via 9Router :20128, model rama-combo, cache.json, repair placeholder + validasi struktur).
- **WAVE 1 SELESAI 2026-11-17**: 33 file .int, 8.691 entry, 0 missing, encoding integrity NONE.
  - Isi: WillowMenu, WillowGame, GD_Episode01-04, Ake_Cinematics, Ake_VO_Episode_01-13,15-17, GD_Dialog_NPC, GD_DialogEpisode10.
  - Fix berantai: (a) 1.026 subtitle kehilangan bungkus `(Text="...",Time=0)` → audit + restore otomatis; (b) 206 UI English karena aturan "copy persis" kelewat agresif → pass 4 + whitelist ketat; (c) 200 `<StringAliasMap:...>` di-skip extract → entry ditambah manual ke work.json lalu apply ulang; (d) dropdown ValueStrings diterjemah mekanis (Rendah/Sedang/Tinggi/Layar Penuh).
  - Output: /root/projects/bl2-id/out/ + BL2_Indonesia_Wave1.zip (34 file, 170 KB, ada README_PASANG.txt).
- QA penuh (qa_full.py): 4.230 pasangan terjemah diperiksa. Ditemukan & diberesin: 11 baris kehilangan bungkus `(Text=...,Time=0)` + kuncinya (script restore salah tulis), 1 placeholder `%2` model, 92 baris nbsp berubah spasi, tooltip `%1` hilang, "Mau keep setting"/"Keep Setting?" masih English, label "KEPALA TERBUKA" jadi "KEPALA KARAKTER BARU!". Verifikasi akhir: 0 placeholder rusak, 0 struktur rusak, 0 keyless, 63 flag English semuanya nama proper/kata pinjaman gamer yang sengaja dijaga. Plural `(s)` sengaja di-drop (Indonesia nggak pakai).
- Wave berikutnya: Ake_VOCT_Contextual (10k barks), Ake_VOSQ_Sidequests (4.1k), GD_Challenges, skill/item lainnya.

## Pelajaran Teknis
- Validator .int wajib cek STRUKTUR, bukan cuma placeholder: `(Text="...",Time=0)` harus utuh.
- Prompt "salim persis kalau cuma token" bikin model over-skip UI normal (Start Game, Options) → whitelist dalam script, bukan diserahkan ke model.
- StringAliasMap = prefix internal + kata tampil; prefix dijaga, kata tampil tetap diterjemah.
- apply() menulis ulang file dari extract+cache → edit manual di out/ SELALU kalah; lakukan setelah apply terakhir atau lipat ke cache.

## Status
- Wave 1 siap uji in-game di PC Rama. Belum dites di game asli (baru terverifikasi byte-level + struktur).
