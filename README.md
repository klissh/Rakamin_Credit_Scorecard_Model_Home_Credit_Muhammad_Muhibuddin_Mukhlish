# Rakamin_Credit_Scorecard_Model_Home_Credit_Muhammad_Muhibuddin_Mukhlish

# Analisis Risiko Kredit Home Credit (Credit Scorecard Model)

**Virtual Internship Experience - Rakamin Academy x Home Credit**

## Ringkasan Proyek

Proyek ini adalah bagian dari Virtual Internship Experience (VIX) Data Scientist dengan Rakamin Academy dan Home Credit. Tujuan utamanya adalah untuk membangun model *machine learning* yang dapat memprediksi apakah seorang peminjam akan mengalami kesulitan membayar pinjaman (default) atau tidak.

Analisis ini menggunakan data `application_train.csv` yang disediakan oleh Home Credit. Model yang dihasilkan bertujuan untuk membantu perusahaan mengoptimalkan proses pemberian pinjaman dan meminimalkan kerugian akibat kredit macet.

## Business Problem

**Problem Statement:**
Home Credit bertujuan untuk mengoptimalkan proses pemberian pinjamannya dengan meminimalkan jumlah pelanggan yang disetujui untuk pinjaman namun pada akhirnya gagal bayar (default).

**Goal:**
Meminimalkan jumlah pelanggan yang disetujui untuk pinjaman tetapi pada akhirnya gagal bayar.

**Objectives:**
1.  Mengidentifikasi karakteristik pelanggan yang gagal bayar (defaulter).
2.  Membangun model *machine learning* (ML) yang dapat memprediksi pelanggan yang berpotensi menjadi gagal bayar.

**Metrik Evaluasi:**
Fokus utama evaluasi model adalah pada **Precision** (Presisi) untuk kelas 1 (gagal bayar), karena tujuan bisnisnya adalah meminimalkan *false positive* (pelanggan yang diprediksi akan membayar lunas padahal sebenarnya gagal bayar).

## Alur Kerja Proyek

[cite_start]Proyek ini mengikuti alur kerja standar data science [cite: 231-238]:
1.  **Problem Research:** Mendefinisikan masalah, tujuan, dan metrik evaluasi.
2.  **Data Pre-processing:** Membersihkan data, menangani nilai yang hilang, *feature engineering*, dan *encoding*.
3.  **Data Visualization & Business Insight (EDA):** Menganalisis data untuk menemukan pola dan wawasan bisnis.
4.  **Machine Learning Implementation:** Melatih beberapa model klasifikasi (Logistic Regression, Decision Tree, AdaBoost).
5.  **Model Evaluation:** Mengevaluasi model berdasarkan metrik yang telah ditentukan dan memilih model terbaik.
6.  **Feature Importance:** Menganalisis fitur apa yang paling berpengaruh terhadap prediksi model.
7.  **Business Recommendation:** Memberikan rekomendasi bisnis yang dapat ditindaklanjuti berdasarkan hasil analisis.

## 1. Data Pre-processing

Langkah-langkah pembersihan dan persiapan data yang dilakukan:
* **Penanganan Nilai Hilang:**
    * Menghapus kolom dengan nilai hilang (missing values) lebih dari 50%.
    * Mengisi nilai numerik yang hilang dengan **median**.
    * Mengisi nilai kategorikal yang hilang dengan **modus**.
* **Feature Engineering & Selection:**
    * Mengubah `DAYS_BIRTH` menjadi `YEARS_BIRTH` agar lebih mudah diinterpretasi.
    * Menghapus kolom `SK_ID_CURR` (sebagai identifier) dan kolom `AMT_REQ_CREDIT_BUREAU` yang redundan (Hour, Day, Week, Mon, Qrt).
* **Encoding:** Menerapkan **One-Hot Encoding** pada 13 kolom kategorikal.
* **Penanganan Data Tidak Seimbang (Imbalanced):** Dataset utama sangat tidak seimbang (91.9% Lunas vs 8.1% Gagal Bayar). Untuk mengatasinya, teknik oversampling **SMOTE** diterapkan pada data latih (train data).
* **Scaling:** Menerapkan **StandardScaler** pada data numerik untuk normalisasi.
* **Splitting Data:** Data dibagi menjadi 80% data latih (train) dan 20% data uji (test).

## 2. EDA & Insight Bisnis Utama

Analisis data eksploratif menghasilkan beberapa wawasan kunci:

1.  **Insight Usia:** Peminjam dalam kelompok usia **31-40 tahun** memiliki proporsi gagal bayar (TARGET=1) tertinggi. Sebaliknya, peminjam di kelompok usia **61-70 tahun** adalah yang paling patuh membayar (TARGET=0).
2.  **Insight Cek Kredit:** Peminjam yang gagal bayar (TARGET=1) cenderung memiliki median jumlah permintaan cek kredit BIK (`AMT_REQ_CREDIT_BUREAU_YEAR`) yang lebih tinggi dalam satu tahun terakhir.
3.  **Insight Telepon:** Terdapat korelasi kuat antara tidak diberikannya nomor telepon dan gagal bayar. **75.5%** dari peminjam yang gagal bayar (TARGET=1) adalah mereka yang **tidak memberikan nomor telepon** (`FLAG_PHONE` = 0).
4.  **Insight Kepemilikan Properti:** Secara mengejutkan, **68.4%** dari peminjam yang gagal bayar (TARGET=1) adalah mereka yang **memiliki** rumah atau flat (`FLAG_OWN_REALTY` = Y). Ini adalah temuan menarik yang memerlukan investigasi bisnis lebih lanjut.

## 3. Hasil Model

Tiga model klasifikasi diuji. [cite_start]**AdaBoost Classifier** dipilih sebagai model final karena memberikan kinerja yang paling stabil dan general antara data latih dan data uji[cite: 134, 135].

| Model | Akurasi (Train) | Akurasi (Test) | Presisi (Test - Target 1) | Keterangan |
| :--- | :---: | :---: | :---: | :--- |
| Logistic Regression | 70.22% | 69.96% | 70% | Underfitting |
| Decision Tree | 100.0% | 87.35% | 89% | Overfitting |
| **AdaBoost Classifier** | **85.85%** | **86.50%** | **86%** | **Model Final (Stabil)** |

## 4. Feature Importance (AdaBoost)

Fitur-fitur berikut memiliki pengaruh paling signifikan terhadap prediksi model AdaBoost:
1.  `AMT_REQ_CREDIT_BUREAU_YEAR` (Jumlah cek kredit BIK dalam 1 tahun terakhir)
2.  `FLAG_PHONE` (Apakah pelanggan memberikan nomor telepon)
3.  `OBS_30_CNT_SOCIAL_CIRCLE` (Observasi lingkungan sosial pelanggan)
4.  `NAME_INCOME_TYPE_Working` (Tipe pendapatan: Bekerja)
5.  `FLAG_OWN_REALTY_Y` (Pelanggan memiliki properti)

## 5. Rekomendasi Bisnis

Berdasarkan wawasan dan hasil model, berikut adalah rekomendasi yang dapat ditindaklanjuti untuk Home Credit:

1.  [cite_start]**Perketat Penilaian untuk Cek Kredit Tinggi:** Terapkan penilaian risiko yang lebih ketat atau peninjauan manual untuk pelanggan dengan jumlah `AMT_REQ_CREDIT_BUREAU_YEAR` yang tinggi[cite: 178, 179]. Ini menunjukkan peminjam mungkin secara aktif mencari banyak pinjaman.
2.  [cite_start]**Jadikan `FLAG_PHONE` Faktor Risiko:** Pelanggan yang tidak memberikan nomor telepon (`FLAG_PHONE` = 0) harus secara otomatis mendapatkan skor risiko yang lebih tinggi[cite: 182]. Pertimbangkan verifikasi kontak alternatif sebagai langkah wajib untuk aplikasi ini.
3.  [cite_start]**Gunakan Data Sosial & Usia:** Gunakan `OBS_30_CNT_SOCIAL_CIRCLE` sebagai indikator awal risiko di area tertentu[cite: 183, 187]. [cite_start]Fokuskan upaya akuisisi pelanggan pada demografi usia yang lebih stabil (misal 60+ tahun) dan terapkan pemeriksaan tambahan untuk kelompok usia berisiko tinggi (31-40 tahun)[cite: 90, 91].
4.  [cite_start]**Investigasi Anomali Kepemilikan Properti:** Lakukan analisis bisnis mendalam untuk memahami *mengapa* pelanggan yang memiliki properti (`FLAG_OWN_REALTY_Y`) memiliki tingkat gagal bayar yang lebih tinggi[cite: 192, 193].

## Cara Menjalankan Proyek

1.  **Clone Repositori:**
    ```bash
    git clone [https://github.com/USERNAME/REPOSITORY-NAME.git](https://github.com/USERNAME/REPOSITORY-NAME.git)
    cd REPOSITORY-NAME
    ```

2.  **Instalasi Dependensi:**
    Proyek ini menggunakan library Python standar untuk data science. Anda dapat menginstalnya menggunakan pip:
    ```bash
    pip install pandas numpy matplotlib seaborn scikit-learn imbalanced-learn
    ```

3.  **Jalankan Notebook:**
    Buka dan jalankan file Jupyter Notebook `Credit_Scorecard_Model_Home_Credit_Muhammad_Muhibuddin_Mukhlish.ipynb` di lingkungan Anda (misal: Jupyter Lab, VS Code, atau Google Colab).

4.  **Dataset:**
    Pastikan file `application_train.csv` berada di direktori yang sama atau sesuaikan path file di dalam notebook.

## Penulis

* [cite_start]**Muhammad Muhibuddin Mukhlish** [cite: 1]
    * [Link LinkedIn Anda]
    * [Link GitHub Anda]

## Ucapan Terima Kasih

[cite_start]Terima kasih kepada **Rakamin Academy** dan **Home Credit** atas kesempatan Virtual Internship Experience (VIX) Data Scientist ini[cite: 3, 6].
