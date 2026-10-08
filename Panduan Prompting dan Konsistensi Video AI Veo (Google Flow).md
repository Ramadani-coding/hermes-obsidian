# Panduan Prompting dan Konsistensi Video AI Veo (Google Flow)

Panduan teknis dan rekayasa prompt untuk produksi video promosi UGC menggunakan model generatif **Veo 3.1** di platform **Google Flow**.

---

## 1. Karakteristik dan Limitasi Model Veo 3.1

Pengujian langsung pada model Veo 3.1 (termasuk varian Lite 720p) mengungkap beberapa batasan arsitektural penting:

1. **Resolusi dan Teks Mikro**: Model video generatif belum memiliki resolusi spasial yang cukup untuk merender teks kecil pada antarmuka layar HP atau laptop secara tajam. Mencoba memaksa talent memegang HP yang menampilkan web toko menghasilkan visual kabur dan berantakan (*slop*).
2. **Prinsip Decoupling**: Pisahkan aset promosi menjadi dua layer terpisah: video talent kreator (UGC) yang berbicara natural, dan rekaman layar / grafis vektor produk yang digabungkan pada proses editing akhir.
3. **Halusinasi Casing Luar Laptop (Exterior Lid Hallucination)**: Pada sudut pandang kamera depan (*front-facing medium close-up*), bagian laptop yang terlihat adalah punggung luar casing (*lid*). Jika prompt meminta green screen atau tampilan monitor pada laptop, Veo sering mengalami halusinasi fatal dengan menempelkan layar monitor hijau beserta tracking marker di punggung luar casing laptop. Solusinya adalah menyertakan negative constraint eksplisit: `normal solid space gray aluminum lid, completely smooth, strictly no exterior screens or displays`.
4. **Limitasi Durasi 8 Detik & Pacing Dialog**: Durasi generasi Veo dibatasi paten 5 hingga 8 detik per take. Memaksakan aksi fisik multi-fase dan naskah dialog panjang dalam 1 klip akan membuat AI mempercepat tempo bicara secara ekstrem seperti ngerap (*fast rapping*) dan artikulasi menjadi tidak jelas. Kuota naskah dialog wajib dibatasi maksimal 12 hingga 15 kata per klip 5-6 detik.
5. **Basis Bahasa Inggris**: Latent space dan tokenizer audio-visual model Veo 3 berbasis bahasa Inggris. Prompt deskriptif visual wajib menggunakan bahasa Inggris, sedangkan dialog bahasa Indonesia memerlukan struktur direktif fonetik khusus agar artikulasi tidak melenceng.

---

## 2. Metode Master Portrait untuk Konsistensi Wajah

Membuat video berseri dari teks prompt murni (*pure text-to-video*) selalu menghasilkan wajah karakter yang berubah-ubah karena seed sampling probabilistik.

### Alur Kerja (Workflow)
1. **Bikin Foto Master**: Hasilkan 1 gambar potret vertikal 9:16 dengan parameter fisik yang sangat spesifik (gaya rambut, pakaian, bentuk wajah, pencahayaan).
2. **Kunci Ciri Fisik**: Gunakan deskripsi pakaian dan rambut yang unik dan tidak ambigu, misalnya `22-year-old Indonesian woman with shoulder-length wavy dark brown hair, natural skin texture, wearing a casual beige crewneck shirt`.
3. **Upload sebagai Start Frame / Likeness**: Unggah foto master tersebut ke Google Flow sebagai *Character Reference* atau *Start Frame*. Dengan metode ini, Veo hanya menganimasikan ekspresi, gerak bibir, dan gestur tanpa mengacak struktur wajah dasar.

---

## 3. Koreografi Pergerakan Kamera Dinamis

Untuk video promosi UGC di mana talent berbicara sambil berjalan menuju kamera, model rentan mengalami guncangan perspektif atau distorsi tubuh jika kamera tidak diarahkan secara presisi.

### Frasa Kunci Prompt
```text
The exact same Indonesian woman is walking forward smoothly towards the camera while speaking enthusiastically. Camera smoothly tracks backwards at eye level maintaining a steady medium shot.
```
- **`Camera smoothly tracks backwards`**: Memerintahkan virtual camera bergerak mundur secara konstan mengikuti kecepatan langkah karakter.
- **`Maintaining a steady medium shot`**: Menjaga proporsi tubuh talent tetap stabil di area dada ke atas (*medium shot*), mencegah pembesaran ekstrem atau pemotongan kepala.
- **`Natural casual walking pace`**: Menstabilkan ritme langkah kaki agar tidak terlihat kaku atau meluncur (*foot sliding*).

---

## 4. Presisi Artikulasi Audio Bahasa Indonesia

Model audio-visual terintegrasi pada Veo cenderung melafalkan kata-kata non-Inggris dengan aksen asing atau pengucapan yang belepotan jika hanya diberi dialog mentah.

### Teknik Optimasi Skrip Audio
1. **Direktif Artikulasi**: Tambahkan instruksi fonetik `The woman speaks fluent Indonesian at a steady, relaxed pace, articulating each word and syllable clearly and distinctly, and says: "..."` tepat sebelum dialog dimulai.
2. **Sinkronisasi Bibir Terbuka**: Tambahkan instruksi penutup audio `Clear realistic lip-sync matching the spoken Indonesian words, wide and natural mouth movements, no subtitles`.
3. **Penulisan Kata Serapan**: Pisahkan awalan bahasa Indonesia pada istilah asing menggunakan tanda hubung (misal: `di-restock`), sehingga mesin tidak membacanya sebagai gabungan kata bahasa Inggris yang keliru.
4. **Batas Kuota Kata**: Jangan melebihi 12–15 kata dalam 1 klip agar ritme pengucapan tetap santai, percaya diri, dan mudah dipahami penonton.

---

## 5. Metode Cut on Action (Workflow Pemisahan 2 Klip)

Model AI generatif saat ini belum memiliki pemahaman temporal yang cukup untuk memadukan transisi emosi kompleks dan multi-aksi fisik dalam satu take panjang (misal: kesal di depan layar -> menggerakkan tangan menutup laptop -> menoleh -> tersenyum -> berbicara solusi). Memaksakan hal tersebut dalam 1 prompt memicu cacat visual seperti deformasi tangan elastis (*rubber hands*) atau distorsi wajah (*face morphing*).

Solusi standar industri adalah memecah adegan menjadi 2 klip terpisah dengan teknik **Cut on Action**:

### Klip 1 (Hook Aksi: 2–3 Detik)
Murni aksi fisik dan ekspresi emosional, tanpa dialog lisan agar AI fokus pada dinamika tubuh.
```text
Cinematic 9:16 vertical video. Eye-level medium close-up shot facing an expressive 22-year-old Indonesian woman with shoulder-length wavy dark brown hair, wearing a stylish casual beige crewneck shirt, sitting at a modern wooden desk in a cozy home studio. She stares intently at her unseen laptop screen, suddenly rolls her eyes and shakes her head in sharp frustration. With a crisp, decisive physical motion, her right hand reaches out and firmly folds the laptop lid completely shut flat onto the desk. The closed laptop is a normal sleek space gray aluminum lid, completely smooth, strictly no exterior screens or displays. Sharp focus on face and hands, natural fluid motion, cinematic 4k, 24fps.

Audio: Realistic ambient room sounds, subtle frantic mouse clicks, ending with a sharp, crisp acoustic thud of the laptop lid snapping shut. Strictly no spoken dialogue.
```

### Klip 2 (Solusi & Dialog: 5–6 Detik)
Laptop dalam posisi sudah tertutup sejak frame awal (detik ke-0). Talent sudah tersenyum menghadap kamera, fokus 100% pada ekspresi ramah, gestur terbuka, dan artikulasi bibir.
*(Gunakan tangkapan layar wajah dari Klip 1 pada slot Character Reference sebelum generate).*
```text
Cinematic 9:16 vertical video. Eye-level medium shot of the identical 22-year-old Indonesian woman with shoulder-length wavy dark brown hair, wearing the same beige shirt, sitting at the exact same wooden desk in the cozy home studio. From the very first frame, the sleek space gray laptop is already completely closed flat on the desk in front of her. She is smiling warmly and confidently directly into the camera. As she speaks, she uses open, distinct, and natural hand gestures to explain the solution to the viewer. Warm cinematic desk lighting, soft creamy blurred background, sharp 4k detail, 24fps.

Audio: The woman speaks fluent Indonesian in an energetic, inviting, and crystal-clear voice, calmly articulating each word and syllable with distinct diction without rushing, and says: "Capek kekunci watermark terus? Gas upgrade Canva Pro di Nara Premium, otomatis aktif via QRIS!" Flawless lip-sync synchronized with the spoken Indonesian words, wide and clear mouth movements, natural smiling expressions, no subtitles.
```

### Penyambungan di NLE / CapCut
1. Susun Klip 1 dan Klip 2 secara berurutan di timeline.
2. Potong Klip 1 tepat pada frame saat penutup laptop menyentuh permukaan meja.
3. Sambungkan langsung ke frame awal Klip 2.
4. Tambahkan sound effect (SFX) benturan lembut (*Slam/Whoosh*) pada titik sambungan. Transisi visual akan terlihat mulus dan profesional karena perhatian penonton terfokus pada gerakan fisik tangan.

---

## 6. Penguncian Latar Lingkungan (Environment Locking)

Prompt generik seperti `outdoor background` akan memicu halusinasi lingkungan yang berganti-ganti di setiap iterasi generasi video.

Untuk mempertahankan kesinambungan (*scene continuity*):
- Deskripsikan latar spesifik secara konsisten di seluruh urutan prompt (misalnya: `modern wooden desk in cozy home studio with warm soft indoor lighting and blurred bookshelf`).
- Pertahankan baris deskripsi lingkungan yang sama persis di setiap potongan adegan lanjutan.

---

## 7. Standarisasi SOP Petunjuk Agen (Agent Instructions)

Gunakan template pedoman ini pada kolom *Agent Instructions* / petunjuk sistem di platform agen AI agar pembuatan prompt video promosi selalu mengikuti kaidah teruji:

```text
Peran: Expert Creative Director & Short-Form Video Prompt Engineer khusus model Google Veo 3.1 (Google Flow).

Aturan Wajib Pembuatan Prompt Video Iklan 9:16:
1. Bahasa & Format: Deskripsi visual dalam bahasa Inggris sinematik presisi. Audio WAJIB menyertakan tag: Audio: The [person] speaks fluent Indonesian at a steady, relaxed pace, articulating each word clearly and distinctly, and says: "[Dialog]". Tambahkan "Clear realistic lip-sync matching the spoken Indonesian words, wide and natural mouth movements, no subtitles."
2. Metode Cut on Action: Pisahkan adegan menjadi 2 klip terpisah. Klip 1 (Hook 2-3s, aksi fisik murni, strictly no spoken dialogue). Klip 2 (Solusi 5-6s, laptop sudah tertutup dari frame awal, fokus senyum dan bicara artikulatif).
3. Batasan Dialog: Maksimal 12-15 kata per klip agar tempo pengucapan santai dan alami.
4. Decoupling & Anti-Halusinasi: Punggung casing laptop wajib dikunci: "normal solid space gray aluminum lid, completely smooth, strictly no exterior screens or displays". Dilarang merender UI aplikasi di layar kecil, lakukan via overlay editing.
5. Konsistensi Karakter: Kunci deskripsi fisik secara detail dan identik di setiap prompt serta gunakan Character Reference.
```

---

## 8. Integrasi Google Flow MCP vs Web Studio

- **Protokol MCP (`https://flow.googleapis.com/mcp`)**: Efektif untuk discovery tool, validasi skema parameter, dan integrasi perencanaan adegan (*Flow Creation Agent*). Namun untuk eksekusi render video generasi panjang, endpoint saat ini membutuhkan konteks sesi applet internal Google Labs.
- **Strategi Efisien**: Gunakan agen Hermes/Antigravity untuk merekayasa arsitektur prompt, skrip dialog, dan foto referensi master, kemudian eksekusi proses render video langsung pada UI Google Flow Studio.

---

Kembali ke: [[Google Flow MCP]] | [[Pengetahuan]]