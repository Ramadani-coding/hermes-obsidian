---
created: 2026-11-16
status: riset-awal
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
- Total 25.932 key / 25.538 non-kosong / 195 file. Unik ~?
- Pipeline dibangun: /root/projects/bl2-id/pipeline.py (extract -> translate via 9Router http://127.0.0.1:20128, key env HERMES_CUSTOM_192_168_1_20_20128_API_KEY, model rama-combo, batch 40, cache.json, ThreadPool 5).
- Encoding dijaga presisi (BOM + CRLF + quote style, verified byte-level).
- Skip rule: value kosong, token teknis (NoEN, edge_*), <StringAliasMap:>, glyph button, tanpa huruf.
- Tes kecil 8 file sukses, hasil natural ("Cash Udah Maksimal", "Rekaman ECHO. Kayaknya ini berisi info soal Kopral Reiss.").
- Wave 1 (core, ~8.5k string / 3.9k unik): WillowMenu, WillowGame, GD_Episode01-04, Ake_Cinematics, Ake_VO_Episode_01-17, GD_Dialog_NPC, GD_DialogEpisode10.
- Wave berikutnya: Ake_VOCT_Contextual (10k barks), Ake_VOSQ_Sidequests (4.1k), GD_Challenges, GD_Assassin_Skills, item/skill lainnya.

## Status
- Rama akan beli game; file `.int` dikirim dari PC-nya (Fern berjalan di STB, tak bisa akses file PC).
- Mulai dari WillowGame.INT (dialog utama) lalu UI.
