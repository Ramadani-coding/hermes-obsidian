# Konsep Sesi Obrolan dan Grup di Hermes

Penjelasan mengenai cara kerja isolasi sesi antara obrolan pribadi (DM) dan grup di Telegram/platform messaging.

## 1. Memori Jangka Panjang vs Konteks Obrolan
- **Memori Permanen (`SOUL.md`, `MEMORY.md`, `USER.md`):** Tetap aktif dan diingat di mana pun Hermes berada. Persona Fern, biodata Rama, dan aturan kerja tidak akan hilang.
- **Konteks Sesi Berjalan (Chat History):** **Tidak terbawa.** Setiap ruang obrolan (DM vs Grup) memiliki kunci sesi (`session_key`) yang terisolasi. Obrolan teknis atau pribadi yang terjadi di DM tidak akan bocor ke obrolan grup.

## 2. Isolasi Sesi di Grup (`group_sessions_per_user`)
- Secara bawaan, Hermes mengaktifkan `group_sessions_per_user: true`.
- Artinya, setiap anggota di dalam grup memiliki sesi percakapan sendiri dengan bot. Pertanyaan atau konteks pengguna A tidak akan bercampur dengan pengguna B.
- Fitur ini menjaga privasi pengguna, mencegah konflik konteks antar-anggota, dan mengontrol penggunaan token LLM.

Tautan Terkait:
- [[Profil Hermes]]
- [[Pengetahuan]]
- [[Home]]
