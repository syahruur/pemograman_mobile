# Skripsi â€” Klasifikasi Sentimen Ulasan Aplikasi Access by KAI

Repositori penelitian skripsi Teknik Informatika. Isi disusun mengikuti alur kerja
penelitian, mulai dari mencari dataset sampai studi literatur.

> Status singkat: dataset sudah diambil (5.000 ulasan), lingkungan sudah siap,
> topik & judul sudah ditentukan, studi literatur (10 artikel + tabel state of the
> art) sudah disusun, dan **seluruh pipeline notebook 01â€“05 sudah selesai &
> tereksekusi** (scraping â†’ EDA â†’ prapemrosesan â†’ pemodelan Random Forest â†’
> evaluasi). Hasil utama: akurasi uji â‰ˆ **0,91**, F1 â‰ˆ **0,92**, AUC â‰ˆ **0,95**.

---

## 1. Mencari Dataset Publik

- **Sumber:** Google Play Store â€” aplikasi **Access by KAI**
  (`com.kai.kaiticketing`), pengembang PT. Kereta Api Indonesia (Persero),
  10.000.000+ unduhan, rating 4,79.
- **Jenis data:** ulasan/komentar pengguna (data teks tak terstruktur) beserta rating
  bintang 1â€“5.
- **Cara pengambilan:** *scraping* memakai pustaka `google-play-scraper`
  dengan parameter bahasa `id` dan negara `id`, urut `NEWEST` â€” dijalankan pada
  `notebook/01_scrapping_ulasan.ipynb`.
- **Berkas hasil:** `data/raw/reviews_access_by_kai.csv` (data mentah, tidak boleh
  diedit langsung).
- **Jumlah:** 5.000 ulasan; distribusi bintang
  {1: 1673, 2: 302, 3: 319, 4: 291, 5: 2415}; tidak ada ulasan teks kosong.
- **Tanggal ekstrak:** 2026-10-10 (tercatat pada kolom `scraped_at`).
- **Lisensi/sitasi:** ulasan adalah konten publik di Google Play Store; sumber disitasi
  lewat tautan aplikasi
  https://play.google.com/store/apps/details?id=com.kai.kaiticketing

> **Aturan data mentah:** CSV di `data/raw/` tidak pernah diedit langsung. Seluruh
> pembersihan dan transformasi dilakukan oleh kode dan disimpan ke `data/processed/`.

## 2. Persiapan Lingkungan Pengembangan Lokal

- **Bahasa & runtime:** Python 3.14.7 (pip 26.2.1)
- **Pustaka Python:** `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`,
  `wordcloud`, `jupyter`, `google-play-scraper`, `Sastrawi`
- **Alat pengembangan:** Git/GitHub (commit & push manual oleh penulis),
  editor (VS Code), dan asisten CLI (mis. opencode / Hermes).
- **Perintah menyiapkan:**

  ```
  python -m pip install pandas numpy scikit-learn matplotlib seaborn wordcloud jupyter google-play-scraper Sastrawi
  ```

- **Menjalankan notebook:**

  ```
  python -m jupyter notebook
  ```

  atau eksekusi headless:

  ```
  python -m nbconvert --to notebook --execute --inplace notebook/01_scrapping_ulasan.ipynb
  ```

## 3. Menentukan Topik Penelitian

- **Topik utama:** analisis sentimen / klasifikasi teks berbahasa Indonesia.
- **Domain:** aplikasi transportasi publik (kereta api) ulasan pengguna Access by KAI.
- **Pendekatan:** machine learning klasifikasi teks dengan ekstraksi fitur TF-IDF.
- **Alasan pemilihan:** ulasan berjumlah besar dan tidak terstruktur sehingga perlu
  diproses otomatis; topik kaya data, selesai satu semester, dan berkontribusi praktis
  bagi pengembang aplikasi.

## 4. Menentukan Judul Penelitian

Pola penulisan: `klasifikasi tentang A menggunakan algoritma B` (dapat ditambah
algoritma C bila nanti membandingkan dua model).

- **Judul penelitian (ditetapkan):**
  **"Klasifikasi Sentimen Ulasan Aplikasi Access by KAI Menggunakan Algoritma Random Forest Berbasis TF-IDF"**
- **Alternatif (draf):**
  - "Analisis Sentimen Ulasan Pengguna Aplikasi Access by KAI dengan Metode Random Forest Berbasis TF-IDF"
  - "Penerapan Random Forest dan TF-IDF untuk Klasifikasi Sentimen Ulasan Aplikasi Access by KAI"
- **Template bila memakai dua algoritma (cadangan):**
  "Optimasi Klasifikasi Sentimen Ulasan Aplikasi Access by KAI Menggunakan Algoritma
  **[B]** dan **[C]**"

> Keputusan saat ini: **satu algoritma — Random Forest** (tuning hyperparameter dengan
> GridSearchCV).

## 5. Menentukan Pertanyaan Penelitian

- **RQ1:** Bagaimana menerapkan algoritma Random Forest dengan ekstraksi fitur TF-IDF
  untuk mengklasifikasi sentimen ulasan aplikasi Access by KAI?
- **RQ2:** Seberapa baik performa model Random Forest dalam mengklasifikasi sentimen
  (akurasi, precision, recall, F1-score, AUC-ROC)?
- **RQ3:** Bagaimana pengaruh tuning hyperparameter (GridSearchCV) terhadap performa
  model dibandingkan parameter default?

## 6. Studi Literatur 10 Artikel & Tabel Penelitian Terdahulu (State of the Art)

- **Target:** minimal 10 artikel jurnal/konferensi terkait analisis sentimen ulasan
  aplikasi, klasifikasi teks bahasa Indonesia, dan/atau algoritma Random Forest.
- **Kualitas sumber (wajib):** jurnal **minimal terakreditasi SINTA 3** (untuk artikel
  Indonesia) atau jurnal bereputasi setara (Scopus/IEEE/Elsevier) untuk artikel
  internasional. Cantumkan peringkat SINTA/kuartil pada kolom sumber.
- **Kata kunci pencarian:** `sentiment analysis google play reviews`,
  `analisis sentimen ulasan aplikasi`, `klasifikasi teks bahasa Indonesia Random Forest`,
  `TF-IDF random forest text classification`, `sentiment analysis KAI / transportasi`.
- **Sumber:** Google Scholar, Garuda, SINTA (cek peringkat akreditasi di
  https://sinta.kemdikbud.go.id/journals), IEEE Xplore, ScienceDirect â€” PDF disimpan di
  `literatur/`.
- **Keluaran:** tabel penelitian terdahulu (state of the art) dan *research gap*.

Semua artikel di bawah sudah dicek peringkatnya pada
https://sinta.kemdiktisaintek.go.id/journals (status per 2026).

**Tabel penelitian terdahulu (state of the art):**

| No | Penulis (Tahun) | Judul | Jurnal (SINTA/Kuartil) | Metode/Algoritma | Data & Domain | Hasil | Celah / Kebaruan | Tautan / DOI |
|----|-----------------|-------|------------------------|------------------|---------------|-------|-----------------|--------------|
| 1 | Mustakim, H., & Priyanta, S. (2022) | Aspect-Based Sentiment Analysis of KAI Access Reviews Using NBC and SVM | IJCCS â€” **SINTA 2** | TF(-IDF); NaÃ¯ve Bayes (Multinomial) & SVM, 3 skenario (skenario 3 SVM + tuning) | Ulasan **KAI Access** di Google Play Store | Skenario terbaik (SVM + tuning): akurasi **91,63%**; precision 77,60%; recall 74,47%; F1 75,55% | Berbasis aspek; hanya NBC & SVM; belum memakai Random Forest; tuning tidak dirancang khusus untuk RF | [DOI](https://doi.org/10.22146/ijccs.68903) Â· [Artikel](https://journal.ugm.ac.id/ijccs/article/view/68903) |
| 2 | Nur Alfiana, H., Doewes, A., & Widoyono, B. (2026) | Aspect-Based Sentiment Analysis of Access by KAI Application Reviews Using IndoBERT for Multi-Label Classification Tasks | JUTIF â€” **SINTA 2** | IndoBERT (fine-tuning) untuk 3 tugas ABSA; dibandingkan SVM/XGBoost+TF-IDF, BiLSTM, mBERT, XLM-R | Ulasan **Access by KAI** (Google Play), anotasi hibrida | IndoBERT terbaik â€” klasifikasi aspek & sentimen akurasi **0,928**; klasifikasi gabungan aspek-sentimen akurasi **0,962** | Deep learning/transformer (berat); fokus multi-label per aspek; tidak menguji RF + TF-IDF yang ringan | [DOI](https://doi.org/10.52436/1.jutif.2026.7.1.5402) Â· [Artikel](https://jutif.if.unsoed.ac.id/index.php/jurnal/article/view/5402) |
| 3 | Faradita, F. A. I., & Sibaroni, Y. (2025) | Multi-Aspect Sentiment Analysis on KAI Access App Reviews (Google Play Store) Using CNN-LSTM Method | IEEE ICoDSA 2025 â€” **Scopus (IEEE)** | CNN-LSTM (deep learning) untuk klasifikasi multi-aspek | Ulasan **KAI Access** (Google Play Store) | Model CNN-LSTM untuk klasifikasi multi-aspek; performa kuantitatif tercantum pada artikel IEEE (pp. 1106â€“1111) | Deep learning, komputasi berat; tidak membandingkan model machine learning klasik (RF) | [DOI](https://doi.org/10.1109/icodsa67155.2025.11157016) Â· [Artikel](https://ieeexplore.ieee.org/document/11157016) |
| 4 | Tangdilomban, C. T., Ramdhan, S. Y., Rizal, M., Suhaeni, C., & Sartono, B. (2026) | From Sparse Features to Transformers: A Statistical Evaluation of TF-IDF, FastText, and IndoBERT for Sentiment Classification of Indonesian Travel App Reviews | JAIC â€” **SINTA 3** | TF-IDF, FastText, IndoBERT Ã— SVM, Logistic Regression, Random Forest (9 konfigurasi); GridSearchCV; stratified 5-fold CV; uji-t | 4.000 ulasan **Traveloka & Tiket.com** (Google Play) | IndoBERT + LR terbaik (F1 **0,9261**); TF-IDF + LR/SVM kompetitif (F1 â‰ˆ 0,892) | Domain travel app (bukan Access by KAI); RF bukan model terbaik; tidak berfokus tuning RF | [DOI](https://doi.org/10.30871/jaic.v10i3.12610) Â· [Artikel](https://jurnal.polibatam.ac.id/index.php/JAIC/article/view/12610) |
| 5 | Sasmita, R. M., Meiriza, A., & Novianti, H. (2025) | An Ensemble Learning Approach for Sentiment Analysis of Maxim Application Reviews Using SVM, KNN, and Random Forest | JAIC â€” **SINTA 3** | SVM, KNN, RF + Voting Classifier & Combined Classifier; TF-IDF unigram; stemming Sastrawi | 2.851 ulasan aplikasi **Maxim** (Google Play) | RF & Combined Classifier terbaik: akurasi **85%**; precision 87%; recall 85%; F1 86%; ROC-AUC **0,91** | Domain ojek online (Maxim); multi-model (ensemble); tidak menganalisis pengaruh hyperparameter RF | [DOI](https://doi.org/10.30871/jaic.v9i6.11447) Â· [Artikel](https://jurnal.polibatam.ac.id/index.php/JAIC/article/view/11447) |
| 6 | Al Hakim, M. G., & Irwiensyah, F. (2024) | Analisis Sentimen Terhadap Ulasan Pengguna Pada Aplikasi Traveloka Menggunakan Metode NaÃ¯ve Bayes | BITS â€” **SINTA 3** | NaÃ¯ve Bayes; preprocessing (case folding, stopword, tokenizing, stemming) | 10.000 ulasan aplikasi **Traveloka** (Google Play) | Akurasi **89,35%**; precision 88,44%; recall 95,05%; F1 91,62% | Hanya NaÃ¯ve Bayes; tidak menerapkan/membandingkan Random Forest | [DOI](https://doi.org/10.47065/bits.v6i3.6119) Â· [Artikel](https://ejurnal.seminar-id.com/index.php/bits/article/view/6119) |
| 7 | Amelia, D., Setiaji, P., & Setiawan, R. R. (2025) | Perbandingan Algoritma SVM dan Naive Bayes dalam Klasifikasi Sentimen pada Ulasan Aplikasi Traveloka dan Agoda | JEPIN â€” **SINTA 3** | SVM & NaÃ¯ve Bayes; BoW & TF-IDF; 3 kelas sentimen (positif/negatif/netral) | Ulasan **Traveloka & Agoda** (Google Play, web scraping) | SVM akurasi **92%**; NaÃ¯ve Bayes maksimum **96%** (lebih stabil pada kelas netral) | Hanya dua algoritma klasik (SVM/NB); belum melibatkan RF | [DOI](https://doi.org/10.26418/jp.v11i2.96869) Â· [Artikel](https://jurnal.untan.ac.id/index.php/jepin/article/view/96869) |
| 8 | Sidik, P., Sunarya, I. M. G., & Gunadi, I. G. A. (2025) | Comparison of Random Forest and Support Vector Machine Methods in Sentiment Analysis of Student Satisfaction Questionnaire Comments at ITB STIKOM Bali | JAIC â€” **SINTA 3** | RF vs SVM; split 80:20; dengan & tanpa SMOTE | Komentar kuesioner kepuasan mahasiswa **ITB STIKOM Bali** | Sebelum SMOTE: SVM 0,90 vs RF 0,81; setelah SMOTE keduanya **0,90** | Data kuesioner internal (bukan Google Play); tanpa tuning hyperparameter RF; domain pendidikan | [DOI](https://doi.org/10.30871/jaic.v9i3.9617) Â· [Artikel](https://jurnal.polibatam.ac.id/index.php/JAIC/article/view/9617) |
| 9 | Anjani, A. F., Anggraeni, D., & Tirta, I. M. (2023) | Implementasi Random Forest Menggunakan SMOTE untuk Analisis Sentimen Ulasan Aplikasi Sister for Students UNEJ | TEKNOSI â€” **SINTA 3** | Random Forest + SMOTE; TF-IDF; split 60:40â€“90:10; interpretasi SHAP | 913 ulasan aplikasi **Sister for Students UNEJ** (Google Play) | Model terbaik RF + SMOTE (90:10): akurasi testing **98,9%**; precision 96,7%; recall 100%; F1 98,3%; AUC **99,2%** | Domain aplikasi akademik; belum ada perbandingan default vs GridSearchCV; aplikasi berbeda | [DOI](https://doi.org/10.25077/teknosi.v9i2.2023.163-172) |
| 10 | Amalina, I. N., Norhikmah, & Ashari, W. M. (2025) | Analysis of the Performance Comparison between Random Forest and SVM RBF in Detecting Cyberbullying on Imbalanced Data with the SMOTE Approach | SISTEMASI â€” **SINTA 3** | RF vs SVM-RBF + SMOTE | 47.693 tweet berbahasa Inggris (deteksi cyberbullying) | RF terbaik sebelum SMOTE: akurasi **88,52%**; precision 89,07%; recall 94,00%; F1 **91,49%** | Teks Inggris & domain deteksi (bukan ulasan aplikasi Indonesia); bukan klasifikasi sentimen | [DOI](https://doi.org/10.32520/stmsi.v14i6.5574) Â· [Artikel](https://sistemasi.ftik.unisi.ac.id/index.php/stmsi/article/view/5574) |

**Ringkasan *state of the art*:**

- Analisis sentimen ulasan aplikasi dari Google Play Store sudah banyak dilakukan,
  umumnya dengan pelabelan berbasis rating bintang atau anotasi manual, lalu
  klasifikasi memakai machine learning (NaÃ¯ve Bayes, SVM, Random Forest, KNN) atau
  deep learning (CNN-LSTM, BERT/IndoBERT).
- Khusus **Access by KAI / KAI Access**, penelitian terdahulu memakai NaÃ¯ve Bayes &
  SVM (Mustakim & Priyanta, 2022), IndoBERT (Nur Alfiana dkk., 2026), dan CNN-LSTM
  (Faradita & Sibaroni, 2025) â€” semuanya cenderung **berbasis aspek** dan
  **deep learning/transformer**.
- Kombinasi **TF-IDF + Random Forest** terbukti lazim dan kompetitif
  (Tangdilomban dkk., 2026; Sasmita dkk., 2025), dan menonjol pada data tidak
  seimbang ketika dipadukan **SMOTE** (Anjani dkk., 2023).
- Metrik evaluasi yang umum dipakai: **akurasi, precision, recall, F1-score, dan
  AUC-ROC**; sebagian penelitian menambahkan *cross-validation* dan uji statistik.

**Celah penelitian (*research gap*):**

1. Belum ada penelitian yang menerapkan **Random Forest + TF-IDF pada ulasan
   aplikasi Access by KAI** â€” penelitian KAI Access yang ada memakai NBC/SVM,
   IndoBERT, atau CNN-LSTM.
2. Penelitian Random Forest sebelumnya (Sasmita dkk., 2025; Sidik dkk., 2025;
   Anjani dkk., 2023) umumnya **tidak membandingkan parameter *default* vs *tuning*
   (GridSearchCV)** secara sistematis.
3. Sebagian besar studi KAI Access bersifat **berbasis aspek/multi-label**; masih
   jarang yang mengerjakan **klasifikasi sentimen biner tingkat dokumen** yang
   sederhana dan mudah direproduksi.

**Posisi & kebaruan penelitian ini:** menerapkan **Random Forest berbasis TF-IDF**
untuk klasifikasi sentimen biner ulasan **Access by KAI**, sekaligus mengukur
dampak *tuning hyperparameter* (GridSearchCV) terhadap performa dibandingkan
parameter *default*.

**Daftar rujukan tabel (DOI):**

1. Mustakim & Priyanta (2022). IJCCS, 16(2), 113. https://doi.org/10.22146/ijccs.68903
2. Nur Alfiana, Doewes & Widoyono (2026). JUTIF, 7(1), 286â€“306. https://doi.org/10.52436/1.jutif.2026.7.1.5402
3. Faradita & Sibaroni (2025). ICoDSA (IEEE), 1106â€“1111. https://doi.org/10.1109/icodsa67155.2025.11157016
4. Tangdilomban dkk. (2026). JAIC, 10(3), 3071â€“3083. https://doi.org/10.30871/jaic.v10i3.12610
5. Sasmita, Meiriza & Novianti (2025). JAIC, 9(6), 3697â€“3705. https://doi.org/10.30871/jaic.v9i6.11447
6. Al Hakim & Irwiensyah (2024). BITS, 6(3), 1448â€“1456. https://doi.org/10.47065/bits.v6i3.6119
7. Amelia, Setiaji & Setiawan (2025). JEPIN, 11(2). https://doi.org/10.26418/jp.v11i2.96869
8. Sidik, Sunarya & Gunadi (2025). JAIC, 9(3), 794â€“802. https://doi.org/10.30871/jaic.v9i3.9617
9. Anjani, Anggraeni & Tirta (2023). TEKNOSI, 9(2), 163â€“172. https://doi.org/10.25077/TEKNOSI.v9i2.2023.163-172
10. Amalina, Norhikmah & Ashari (2025). SISTEMASI. https://doi.org/10.32520/stmsi.v14i2.5574

---

## Lampiran â€” Struktur Folder

```
skripsi/
|-- literatur/        # PDF jurnal, buku ajar, catatan tinjauan pustaka
|-- data/
|   |-- raw/          # data mentah, TIDAK PERNAH diubah
|   |-- processed/    # data hasil cleaning dan transformasi (dibuat oleh kode)
|-- notebook/         # Jupyter Notebook: scraping, EDA, eksperimen
|-- model/            # model tersimpan (.pkl/.joblib), log eksperimen
|-- draft/            # draf proposal, skripsi, artikel
|-- panduan/          # buku ajar metodologi penelitian & pedoman penulisan
|-- README.md
`-- .gitignore
```

## Lampiran â€” Target, Fitur, dan Alur Notebook

- **Target:** sentimen dari rating bintang â€” positif: bintang 4â€“5; negatif: bintang 1â€“2;
  bintang 3 (netral) dikeluarkan dari klasifikasi biner (diputuskan di
  `03_persiapan_data.ipynb`).
- **Fitur:** teks ulasan hasil prapemrosesan (cleaning, case folding, normalisasi
  slang, stopword removal dengan negasi dipertahankan, stemming Sastrawi) lalu
  **TF-IDF unigram + bigram** (`min_df=2`, `sublinear_tf=True`).
- **Ketidakseimbangan kelas:** mentah positif 2.706 vs negatif 1.975; setelah ulasan
  netral (319) dan teks kosong (163) dibuang menjadi positif 2.545 vs negatif 1.973
  (rasio Â±1,3:1 â€” relatif seimbang); tetap diperiksa dan dapat ditangani bila perlu
  (class weight / resampling).
- **Algoritma:** Random Forest (tuning hyperparameter dengan GridSearchCV).

| Urutan | Berkas | Isi |
|--------|--------|-----|
| 1 | `01_scrapping_ulasan.ipynb` | Ambil ulasan dari Play Store â†’ `data/raw/` |
| 2 | `02_eda_eksplorasi.ipynb` | Distribusi bintang & sentimen, panjang ulasan, kata terbanyak, wordcloud |
| 3 | `03_persiapan_data.ipynb` | Cleaning, case folding, normalisasi slang, stopword, stemming, label, train/test split |
| 4 | `04_model_klasifikasi.ipynb` | TF-IDF (fit pada train) + Random Forest + tuning hyperparameter GridSearchCV |
| 5 | `05_evaluasi.ipynb` | Akurasi, precision, recall, F1-score, confusion matrix, AUC-ROC |

Hasil olahan disimpan di `data/processed/`; model tersimpan di `model/` dengan penamaan
yang memuat tanggal atau nomor eksperimen.

## Lampiran â€” Hasil Eksperimen (ringkas)

- **Data bersih:** 4.518 ulasan (2.545 positif, 1.973 negatif); split stratified 80:20
  â†’ latih 3.614 baris, uji 904 baris.
- **Fitur TF-IDF:** 4.529 fitur (unigram + bigram), di-*fit* hanya pada data latih.
- **Performa pada data uji (904 ulasan):**

  | Model | Akurasi | Precision | Recall | F1 | AUC-ROC |
  |-------|--------:|----------:|-------:|---:|--------:|
  | RF default | 0,9082 | 0,9532 | 0,8802 | 0,9152 | 0,9545 |
  | RF tuning (GridSearchCV) | 0,9049 | 0,9471 | 0,8802 | 0,9124 | 0,9567 |

- **Parameter terbaik GridSearchCV:** `n_estimators=300`, `max_depth=None`,
  `max_features='log2'`, `min_samples_split=5`, `min_samples_leaf=1`,
  `class_weight=None` (F1 CV â‰ˆ 0,9047).
- **Catatan RQ3:** tuning hyperparameter hanya memberi perubahan kecil; parameter
  *default* sudah mendekati optimal pada ruang pencarian yang diuji (default sedikit
  unggul pada akurasi/F1, tuning sedikit unggul pada AUC).
- **Berkas keluaran:** `data/processed/dataset_bersih.csv`, `train.csv`, `test.csv`,
  `prediksi_test.csv`; `model/perbandingan_default_vs_tuned.csv`,
  `model/rekap_evaluasi_uji.csv`, dan gambar di `model/gambar/`.

## Lampiran â€” Referensi Metodologi

- Buku ajar Metodologi Penelitian (24TIF505) â€” tersedia di `panduan/`
- Peraturan Rektor tentang Pedoman Penulisan Skripsi, Tugas Akhir dan Tesis â€” tersedia di
  `panduan/`