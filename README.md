# Laporan Proyek Machine Learning - Robert Varian

## Project Overview

Industri hiburan yang berkembang pesat telah menghasilkan ribuan film baru setiap tahunnya. Banyaknya pilihan ini seringkali menyulitkan penonton untuk menemukan film yang sesuai dengan preferensi mereka. Oleh karena itu, sistem rekomendasi menjadi alat penting untuk menyaring dan menyajikan film-film yang relevan berdasarkan minat pengguna (Ricci et al., 2015).

Dalam proyek ini, dikembangkan sistem rekomendasi film berbasis content-based filtering menggunakan data film dari Kaggle. Sistem ini menggunakan pendekatan TF-IDF (Term Frequency-Inverse Document Frequency) dan cosine similarity untuk menghitung kemiripan antar film berdasarkan informasi genre-nya. Pendekatan ini telah terbukti efektif dalam sistem rekomendasi berbasis konten, terutama ketika data pengguna tidak tersedia atau sangat terbatas (Aggarwal, 2016; Manning et al., 2008).

Sistem ini ditujukan untuk membantu pengguna menemukan film dengan genre yang serupa dengan film yang mereka sukai sebelumnya, tanpa memerlukan data interaksi pengguna seperti rating atau ulasan. Dataset yang digunakan adalah Movie Genre from its Poster (Kaggle, 2018), yang menyediakan informasi judul, genre, dan URL poster film. Sistem ini dirancang untuk memberikan hasil yang dapat dijelaskan secara intuitif berdasarkan kemiripan genre.

Referensi : Ricci, F., Rokach, L., & Shapira, B. (2015). Recommender Systems Handbook. Springer, Aggarwal, C. C. (2016). Recommender Systems: The Textbook. Springer, Manning, C. D., Raghavan, P., & Schütze, H. (2008). Introduction to Information Retrieval. Cambridge University Press, Kaggle. (2018). Movie Genre from its Poster. Retrieved from: https://www.kaggle.com/datasets/neha1703/movie-genre-from-its-poster.

## Business Understanding

### Problem Statements
1. Bagaimana membantu pengguna menemukan film yang relevan dan sesuai dengan minat mereka?
2. Bagaimana menyajikan rekomendasi film berdasarkan genre film yang telah disukai pengguna?
3. Bagaimana mengukur relevansi dan kemiripan antar film untuk menghasilkan rekomendasi yang akurat?

### Goals
1. Mengembangkan sistem rekomendasi berbasis konten yang dapat menyarankan film berdasarkan genre yang serupa.
2. Menggunakan teknik TF-IDF dan cosine similarity untuk menghitung kemiripan antar film.
3. Memberikan antarmuka pemanggilan fungsi yang sederhana untuk melakukan pencarian film berdasarkan masukan judul film.

### Solution statements
- Content-Based Filtering: Sistem menyarankan film yang memiliki kemiripan genre dengan film yang telah dipilih pengguna.
- Sistem tidak memerlukan data pengguna atau interaksi historis, cukup mengandalkan konten film (genre).

## Data Understanding

### Dataset
Dataset yang digunakan adalah Movie Genre from its Poster dari [Kaggle](https://www.kaggle.com/datasets/neha1703/movie-genre-from-its-poster), yang berisi informasi seperti:
- IMDBID: id dari IMDB
- Imdb Link : link untuk menuju IMDB film
- Title: Judul film
- Genre: Kumpulan genre film
- IMDB Score: Skor film (tidak digunakan dalam model utama)
- Poster: URL poster film (tidak digunakan dalam model utama)

Dataset ini awalnya berisi lebih dari 10.000 entri, namun untuk keperluan demonstrasi dan efisiensi eksperimen, proyek ini hanya mengambil 200 data film teratas yang memiliki informasi genre dan judul yang lengkap.

Exploratory Data Analysis (EDA):
- Visualisasi data yang missing lumayan banyak pada kolom genre dan dan poster
## Data Preparation

Beberapa tahapan data preparation yang dilakukan:
- Penghapusan Nilai Kosong: Menghapus data yang memiliki nilai kosong pada kolom Title atau Genre.
- Penghapusan Duplikasi: Menghapus entri duplikat berdasarkan kolom yang digunakan dengan drop_duplicated.
- TF-IDF Vectorization: Mengubah genre menjadi representasi numerik menggunakan TF-IDF dengan mengabaikan stopwords bahasa Inggris.
- Cosine Similarity Calculation: Menghitung kemiripan antar film menggunakan cosine similarity dari vektor TF-IDF.
- Preprocessing Judul Film: Membersihkan judul dari tahun rilis agar fungsi pencarian tidak terpengaruh format nama.
  
## Modeling

Model utama yang digunakan adalah Content-Based Filtering menggunakan TF-IDF dan Cosine Similarity.
#### Input 
Input : Judul film, misalnya "Jumanji"
#### Proses
- Bersihkan nama judul dari tahun (contoh: "Jumanji (1995)" → "jumanji")
- Temukan indeks film dalam dataset
- Hitung kemiripan cosine dengan semua film lainnya
- Urutkan berdasarkan skor kemiripan
- Ambil `n` film teratas (kecuali film itu sendiri)
#### Output
Rekomendasi 5 film dengan genre yang paling mirip, contohnya:
- Mighty Morphin Power Rangers: The Movie
- Across the Sea of Time
- The Amazing Panda Adventure
- Free Willy 2: The Adventure Home
- Kids of the Round Table

Kelebihan
- Tidak memerlukan data pengguna atau feedback historis
- Cocok untuk sistem rekomendasi baru (cold-start problem untuk pengguna baru)
- Hasil rekomendasi mudah dijelaskan berdasarkan kemiripan konten

Kekurangan
- Rekomendasi terbatas pada kemiripan konten (genre), tidak mempertimbangkan preferensi personal pengguna
- Tidak bisa menyarankan film di luar genre yang biasa ditonton pengguna

## Evaluation
#### Silhouette Score
- Untuk mengevaluasi kualitas kemiripan antar film, dilakukan pengukuran Silhouette Score.
- Hasil skor: 0.234, yang mengindikasikan bahwa struktur kemiripan antar film cukup lemah, namun tetap memberikan pemisahan kluster yang dapat dimanfaatkan.
- Nilai ini bisa ditingkatkan dengan:
    - Menambahkan fitur tambahan (misalnya deskripsi, sinopsis, aktor)
    - Menggabungkan genre dengan metadata lainnya
#### Evaluasi Konseptual
Karena tidak tersedia data eksplisit dari pengguna (seperti rating), maka evaluasi dilakukan secara konseptual:
- Precision: Mengukur seberapa relevan film yang direkomendasikan.
- Recall: Mengukur seberapa banyak film relevan yang berhasil direkomendasikan dari seluruh kemungkinan film yang relevan.
Evaluasi kuantitatif dengan metrik seperti RMSE atau MAE tidak dilakukan karena model tidak memprediksi rating.

Referensi
- Aggarwal, C. C. (2016). Recommender Systems: The Textbook. Springer.
- Ricci, F., Rokach, L., & Shapira, B. (2015). Recommender Systems Handbook. Springer.
- Mikolov, T., Chen, K., Corrado, G., & Dean, J. (2013). Efficient Estimation of Word Representations in Vector Space. arXiv preprint arXiv:1301.3781.
- Manning, C. D., Raghavan, P., & Schütze, H. (2008). Introduction to Information Retrieval. Cambridge University Press.
- Dataset: Movie Genre from its Poster. Kaggle: https://www.kaggle.com/datasets/neha1703/movie-genre-from-its-poster
