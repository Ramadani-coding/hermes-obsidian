# Panduan Prompting dan Konsistensi Video AI Veo (Google Flow)

Panduan teknis dan rekayasa prompt untuk produksi video promosi UGC menggunakan model generatif **Veo 3.1** di platform **Google Flow**.

---

## 1. Karakteristik dan Limitasi Model Veo 3.1

Pengujian langsung pada model Veo 3.1 (termasuk varian Lite 720p) mengungkap beberapa batasan arsitektural penting:

1. **Resolusi dan Teks Mikro**: Model video generatif belum memiliki resolusi spasial yang cukup untuk merender teks kecil pada antarmuka layar HP secara tajam. Mencoba memaksa talent memegang HP yang menampilkan web toko menghasilkan visual kabur dan berantakan (*slop*).
2. **Prinsip Decoupling**: Pisahkan aset promosi menjadi dua layer terpisah: video talent kreator (UGC) yang berbicara natural, dan rekaman layar / grafis vektor produk yang digabungkan pada proses editing akhir.
3. **Basis Bahasa Inggris**: Latent space dan tokenizer audio-visual model Veo 3 berbasis bahasa Inggris. Prompt deskriptif wajib menggunakan bahasa Inggris, sedangkan dialog bahasa Indonesia memerlukan struktur direktif fonetik khusus agar artikulasi tidak melenceng.

---

## 2. Metode Master Portrait untuk Konsistensi Wajah

Membuat video berseri dari teks prompt murni (*pure text-to-video*) selalu menghasilkan wajah karakter yang berubah-ubah karena seed sampling probabilistik.

### Alur Kerja (Workflow)
1. **Bikin Foto Master**: Hasilkan 1 gambar potret vertikal 9:16 dengan parameter fisik yang sangat spesifik (gaya rambut, pakaian, bentuk wajah, pencahayaan).
2. **Kunci Ciri Fisik**: Gunakan deskripsi pakaian dan rambut yang unik dan tidak ambigu, misalnya `natural shoulder-length wavy black hair with curtain bangs, wearing a modern casual pastel beige crewneck shirt`.
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
1. **Direktif Artikulasi**: Tambahkan instruksi fonetik `articulating each word clearly and distinctly` tepat sebelum dialog dimulai.
2. **Penulisan Kata Serapan**: Pisahkan awalan bahasa Indonesia pada istilah asing menggunakan tanda hubung (misal: `di-restock`), sehingga mesin tidak membacanya sebagai gabungan kata bahasa Inggris yang keliru.
3. **Penyebutan Nama Brand**: Berikan penekanan pengucapan pada nama merek (misal: "Nara Premium") agar tidak terpotong saat sinkronisasi bibir (*lip-sync*).

### Contoh Format Prompt Audio Teruji
```text
Audio: The woman speaks fluent Indonesian in an energetic, inviting voice, articulating each word clearly and distinctly, and says "Aplikasi sudah pada di-restock, transaksi juga aman! Gas langsung aja kalian ke website Nara Premium!" Clear lip-sync matching the Indonesian words, natural casual walking pace, expressive hand gestures, 4k detail, no subtitles.
```

---

## 5. Penguncian Latar Lingkungan (Environment Locking)

Prompt generik seperti `outdoor background` akan memicu halusinasi lingkungan yang berganti-ganti di setiap iterasi generasi video (kadang di jalan raya, kadang di gedung kantor).

Untuk mempertahankan kesinambungan (*scene continuity*):
- Deskripsikan latar spesifik secara konsisten di seluruh urutan prompt:
  ```text
  paved pathway in a lush green city park with tall shady trees and neat lawns, soft natural daylight
  ```
- Pertahankan baris deskripsi lingkungan yang sama persis di setiap potongan adegan lanjutan.

---

## 6. Integrasi Google Flow MCP vs Web Studio

- **Protokol MCP (`https://flow.googleapis.com/mcp`)**: Efektif untuk discovery tool, validasi skema parameter, dan integrasi perencanaan adegan (*Flow Creation Agent*). Namun untuk eksekusi render video generasi panjang, endpoint saat ini membutuhkan konteks sesi applet internal Google Labs.
- **Strategi Efisien**: Gunakan agen Hermes/Antigravity untuk merekayasa arsitektur prompt, skrip dialog, dan foto referensi master, kemudian eksekusi proses render video langsung pada UI Google Flow Studio.

---

Kembali ke: [[Google Flow MCP]] | [[Pengetahuan]]
