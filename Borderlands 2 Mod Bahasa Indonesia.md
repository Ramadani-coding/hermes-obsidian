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

## Status
- Rama akan beli game; file `.int` dikirim dari PC-nya (Fern berjalan di STB, tak bisa akses file PC).
- Mulai dari WillowGame.INT (dialog utama) lalu UI.
