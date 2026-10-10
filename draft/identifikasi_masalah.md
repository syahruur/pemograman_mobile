# Identifikasi Masalah — Klasifikasi Sentimen Ulasan Aplikasi Access by KAI

Dikerjakan mengikuti cara identifikasi masalah pada Buku Ajar Metodologi Penelitian Bab III
("Identifikasi Masalah dan Studi Literatur", pertemuan 3). Bukan latihan soal — hanya
penerapan metode identifikasinya ke tema skripsi.

> **Catatan:** dokumen ini menggantikan identifikasi masalah tema lama (klasifikasi kategori
> waktu tempuh Commuter Line) yang sudah tidak dipakai.

## Cara Identifikasi Masalah menurut Buku Ajar (Bab III)

1. Penelitian berbasis data: **pemilihan masalah selalu berpasangan dengan ketersediaan data**.
   Masalah terlalu luas → tidak selesai; terlalu sempit → kontribusi tidak berarti;
   datanya tidak ada → rancangan runtuh.
2. Ide bisa bersumber dari tiga arah: **masalah nyata di lingkungan sekitar**, **data publik
   yang tersedia**, dan **literatur ilmiah** (replikasi-perluasan).
3. Sebuah ide layak maju bila lolos **tiga saringan**: ketersediaan data, kebermaknaan,
   dan kelayakan teknis.
4. Rumusan masalah yang formal baru disusun pada Bab IV — di tahap ini cukup identifikasi.

## A. Sumber Ide untuk Tema Ini

- **Masalah nyata:** aplikasi Access by KAI dipakai lebih dari 10 juta pengguna dan menerima
  ribuan ulasan di Google Play Store. Ulasan itu **tidak terstruktur dan berbahasa campur
  (informal, singkatan, typo)** sehingga mustahil ditinjau manual oleh tim pengembang;
  sementara pengguna lain membaca rating bintang saja tanpa tahu keluhan spesifik apa yang
  dominan. PT KAI dan pengguna sama-sama butuh cara otomatis menandai umpan balik
  positif/negatif dari lautan teks tersebut.
- **Data publik:** ulasan Play Store dapat diambil legal lewat pustaka `google-play-scraper`
  — scraping penuh berhasil mengambil 5.000 ulasan dengan distribusi bintang
  {1: 1673, 2: 302, 3: 319, 4: 291, 5: 2415} (uji teknis awal 200 ulasan juga lolos).
- **Literatur:** analisis sentimen ulasan aplikasi/transportasi umumnya memakai Naive Bayes
  atau SVM; yang menjadi ruang kebaruan adalah **penerapan Random Forest dengan TF-IDF
  untuk mengklasifikasi sentimen ulasan Access by KAI**, sekaligus melihat pengaruh tuning
  hyperparameter terhadap performanya - penelusuran pustaka menyusul (Bab III Subbab 3.4)
  untuk memastikan celah ini.

## B. Identifikasi Masalah

Aplikasi Access by KAI menghasilkan ribuan ulasan pengguna yang seluruhnya berbentuk teks
bebas dengan rating bintang. Informasi berharga di dalamnya — keluhan error pembayaran,
masalah e-boarding pass, kritik fitur, pujaan layanan — tenggelam dalam volume yang tidak
bisa dibaca manusia. Pengembang tidak punya gambaran agregat apakah keluhan dominan bersifat
fungsional (aplikasi rusak) atau non-fungsional (tampilan, promo), dan calon pengguna hanya
melihat angka rating 4,79 tanpa konteks.

Di sisi penelitian, klasifikasi sentimen ulasan berbahasa Indonesia sudah banyak
dikerjakan, tetapi umumnya memakai Naive Bayes atau SVM, dan jarang menerapkan **Random
Forest** pada ulasan aplikasi kereta api Indonesia. Padahal Random Forest tahan terhadap
data berdimensi tinggi hasil TF-IDF dan tidak mudah overfitting. Belum ada (atau jarang)
kajian yang **menerapkan Random Forest beserta tuning hyperparameter-nya** untuk
mengklasifikasi sentimen ulasan Access by KAI.

**Inti masalah:** *bagaimana mengklasifikasi sentimen (positif/negatif) ulasan pengguna
Access by KAI dari teks ulasan menggunakan algoritma Random Forest, dan seberapa baik
performanya setelah hyperparameter di-tuning?*

## C. Tiga Saringan Kelayakan

| Saringan | Hasil | Keterangan |
|---|---|---|
| Ketersediaan data | **Lolos** | Sudah terambil 5.000 ulasan dari `com.kai.kaiticketing` (aplikasi resmi PT KAI, 10 jt+ unduhan) ke `data/raw/reviews_access_by_kai.csv`. Tautan sumber dicatat di README |
| Kebermaknaan | **Lolos** | Bagi PT KAI: umpan balik agregat otomatis; bagi pengguna: pemahaman kualitas layanan; akademis: baseline pembanding 4 algoritma |
| Kelayakan teknis | **Lolos** | Teks pendek, TF-IDF ringan, Random Forest + GridSearchCV, komputasi CPU biasa, muat satu semester |

**Peringatan keseimbangan kelas (Subbab 3.3):** pada 5.000 ulasan, positif (4–5) = 2.706 dan
negatif (1–2) = 1.975 (rasio ±1,4:1) sehingga relatif seimbang, tetapi tahap persiapan data
tetap memeriksanya dan menangani bila perlu (class weight, undersampling, atau SMOTE).

## D. Batas Masalah (agar tidak terlalu luas/terlalu sempit)

- **Cakupan:** satu aplikasi (Access by KAI), satu sumber (Google Play Store Indonesia),
  bahasa Indonesia — bukan seluruh aplikasi transportasi dan bukan Twitter/media sosial.
- **Target:** sentimen biner positif/negatif dari rating bintang — bukan aspek sentimen
  (fitur apa yang dikeluhkan) dan bukan regresi rating.
- **Fitur:** representasi TF-IDF dari teks ulasan — tanpa pendalaman word embedding/deep learning.
- **Model:** satu algoritma, Random Forest (dengan GridSearchCV) — tanpa perbandingan
  algoritma lain maupun transformer/LLM.

Batas ini menjaga masalah tetap spesifik, selesai satu semester, dan tetap punya kontribusi
(celah teruji).

> Rumusan masalah formal, tujuan, dan manfaat penelitian disusun pada Bab IV sesuai alur buku ajar.
