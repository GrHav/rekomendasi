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

Dataset ini awalnya berisi lebih dari 10.000 entri, namun untuk keperluan demonstrasi dan efisiensi eksperimen, proyek ini hanya mengambil 200 data film (.head(200)) teratas yang memiliki informasi genre dan judul yang lengkap.

Alasan menggunakan encoding='ISO-8859-1' pada saat membaca csv adalah untuk menghidari error yang ada dan dengan menggunakan encoding tersebut dapat lebih toleran terhadap karakter non-ASCII. 

Exploratory Data Analysis (EDA):
- Visualisasi data yang missing lumayan banyak pada kolom genre dan dan poster
## Data Preparation

Beberapa tahapan data preparation yang dilakukan:
- Penghapusan Kolom selain Title dan Genre dan mengambil 200 data teratas. `df = df[['Title', 'Genre']].dropna().drop_duplicates().head(200)`
- Penghapusan Nilai Kosong: Menghapus data yang memiliki nilai kosong pada kolom Title atau Genre dengan `dropna()`.
- Penghapusan Duplikasi: Menghapus entri duplikat berdasarkan kolom yang digunakan dengan `drop_duplicates()`.
- TF-IDF Vectorization: Mengubah genre menjadi representasi numerik menggunakan TF-IDF dengan mengabaikan stopwords bahasa Inggris (tfidf = TfidfVectorizer(stop_words='english')).
- Matriks TF-IDF: mengubah kolom genre pada data menjadi vektor numerik menggunakan TF-IDF (tfidf_matrix = tfidf.fit_transform(df['Genre']))
- Cosine Similarity Calculation: Menghitung kemiripan antar film menggunakan cosine similarity dari matriks TF-IDF yang baru dibuat (cosine_sim = cosine_similarity(tfidf_matrix, tfidf_matrix)).
- Preprocessing Judul Film: Membersihkan judul dari tahun rilis agar fungsi pencarian tidak terpengaruh format nama dengan membuat fungsi clean_title untuk membersihkannya.

### TF-IDF 
Fungsi utama : digunakan untuk mengukur kepentingan suatu kata dalam sebuah dokumen relatif terhadap seluruh kumpulan dokumen.
Kelebihan :
- Sederhana dan cepat untuk diimplementasikan
- Efektif untuk menyeleksi kata-kata penting dan mengabaikan kata umum
- Cocok untuk dokumen teks pendek maupun panjang
Kekurangan:
- Tidak menangkap makna konteks atau sinonim
- Tidak mempertimbangkan urutan kata
- Tidak bekerja baik pada data sangat besar tanpa preprocessing efisien
  
Parameter yang digunakan di dalam proyek ini adalah:
- stop_words='english'. Dimana stop word yang dihilangkan adalah stop word dalam bahasa inggris.
- 
## Modeling

Model utama yang digunakan adalah Content-Based Filtering menggunakan TF-IDF dan Cosine Similarity.
### Cosine Similarity
Fungsi utama : digunakan untuk mengukur kemiripan antara dua vektor teks berdasarkan sudut diantara keduanya dalam ruang vektor.
Kelebihan :
- Cocok untuk data berdimensi tinggi
- Tidak terpengaruh oleh panjang dokumen
- Efisien untuk mengukur kemiripan konten berbasis kata
Kekurangan:
- Tidak mempertimbangkan kata-kata semantik
- Bergantung pada representasi vektor
- Tidak mempertimbangkan urutan kata

Cara kerja Content-Based Filtering menggunakan TF-IDF dan Cosine Similarity adalah sebagai berikut:
1. Dalam sistem rekomendasi Content-Based Filtering ini, pendekatan utama yang digunakan adalah representasi teks menggunakan TF-IDF vectorization, yang kemudian diukur kemiripannya menggunakan Cosine Similarity.
2. Representasi Genre dengan TF-IDF
Genre dari masing-masing film direpresentasikan sebagai vektor TF-IDF. Kata-kata umum (seperti "the", "and") dihapus dengan parameter stop_words='english' agar tidak memengaruhi hasil. Hasil dari proses ini adalah tfidf_matrix, yaitu representasi numerik dari genre semua film.
3. Menghitung Cosine Similarity Antar Film
Setelah mendapatkan TF-IDF matrix, digunakan cosine_similarity untuk menghitung seberapa mirip setiap film dengan film lainnya berdasarkan genre. Hasilnya adalah matriks simetri cosine_sim, yang menyimpan skor kemiripan antara setiap pasangan film.

Dari kemiripan skor antar setiap pasang film nantinya akan diambil yang mendekati sebagai rekomendasi film kepada pengguna.

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
#### Recall dan Precision
- Pada model ini mendapatkan precision berupa 0.73 dan recall berupa 1.00 pada saat pengujian input judul berupa Jumanji dengan genre action, adventure, family.
#### Precision
Precision mengukur berapa banyak dari film yang direkomendasikan itu benar-benar relevan dengan film input. Dalam hal ini, relevansi biasanya dihitung berdasarkan genre yang cocok.
Jika precision = 0.73, artinya sekitar 73% dari genre dalam film rekomendasi cocok atau relevan dengan genre film Jumanji. Ini menunjukkan bahwa sebagian besar rekomendasi genre-nya sesuai.
#### Recall
Recall mengukur seberapa lengkap sistem menangkap genre dari film input dalam rekomendasi yang diberikan.
Jika recall = 1.00, artinya tidak ada genre penting pada film input dilewatkan. Dapat diartikan bahwa sistem mampu menangkap semua aspek genre film input dalam rekomendasi yang diberikan. Mungkin ada genre lain yang tidak relevan akan tetapi sisi kelengkapan genre input dalam rekomendasi sudah sangat baik

#### Evaluasi Konseptual
Karena tidak tersedia data eksplisit dari pengguna (seperti rating), maka evaluasi dilakukan secara konseptual:
- Precision: Mengukur seberapa relevan film yang direkomendasikan.
- Recall: Mengukur seberapa banyak film relevan yang berhasil direkomendasikan dari seluruh kemungkinan film yang relevan.
Evaluasi kuantitatif dengan metrik seperti RMSE atau MAE tidak dilakukan karena model tidak memprediksi rating.

## Menjawab Problem Statements
1. Bagaimana membantu pengguna menemukan film yang relevan dan sesuai dengan minat mereka?
- Sistem menggunakan teknik TF-IDF untuk mengekstrak fitur genre dan cosine similarity untuk menghitung kemiripan antar film. Dengan pendekatan ini, sistem mampu memberikan rekomendasi film yang memiliki genre serupa dengan film yang telah disukai sebelumnya.
- Contoh: Pengguna mencari film “Jumanji”, sistem merekomendasikan film lain dengan genre petualangan dan fantasi seperti:
      - Mighty Morphin Power Rangers: The Movie
      - The Amazing Panda Adventure
      - Free Willy 2: The Adventure Home
Rekomendasi tersebut relevan karena berbagi genre yang serupa, sehingga sistem telah berhasil membantu pengguna menavigasi pilihan film secara lebih efisien.

2. Bagaimana menyajikan rekomendasi film berdasarkan genre film yang telah disukai pengguna?
- Genre merupakan satu-satunya fitur yang digunakan dalam sistem. TF-IDF mengubah informasi genre menjadi vektor numerik, dan cosine similarity mengukur kemiripan antar film berdasarkan vektor tersebut.
-  Kelebihan pendekatan ini:
       - Genre adalah fitur yang konsisten dan tersedia untuk hampir semua film
       - Tidak memerlukan data pengguna atau interaksi historis
       - Efektif dalam konteks cold-start problem untuk pengguna baru
- Keterbatasan:
       - Rekomendasi hanya berdasarkan genre, sehingga bisa kurang akurat bila pengguna menginginkan pertimbangan lain seperti alur cerita, sutradara, atau aktor.
       - Bisa terjadi tumpang tindih genre yang membuat rekomendasi kurang spesifik.

3. Bagaimana mengukur relevansi dan kemiripan antar film untuk menghasilkan rekomendasi yang akurat?
- Pengukuran relevansi dan kemiripan dilakukan dengan:
       - Menggunakan cosine similarity antar vektor TF-IDF yang mewakili genre film.
       - Semakin tinggi nilai cosine similarity, semakin mirip genre film-film tersebut.
- Evaluasi Kualitatif:
Manual inspection terhadap hasil rekomendasi menunjukkan bahwa film-film yang direkomendasikan memiliki genre yang serupa dan masuk akal secara tematis.
- Evaluasi Kuantitatif (Opsional - Eksploratif):
     - Silhouette Score digunakan untuk mengevaluasi seberapa baik film dikelompokkan berdasarkan kemiripan genre.
     - Hasil Silhouette Score = 0.234, yang meskipun tidak tinggi, menunjukkan ada struktur yang dapat dimanfaatkan dalam sistem.
- Precision dan Recall:
     -  Precision menunjukkan seberapa banyak genre dalam rekomendasi memang relevan dengan film input (dalam contoh 0.73 berarti 73% genre rekomendasi cocok).
     -  Recall menunjukkan seberapa lengkap genre film input berhasil tercakup dalam rekomendasi (dalam contoh recall 1.00 berarti semua genre input berhasil ditemukan di rekomendasi).

Dengan pendekatan ini, sistem dapat memberikan rekomendasi yang cukup relevan dan membantu pengguna dalam menemukan film sesuai preferensi genre mereka, meskipun masih ada ruang untuk pengembangan lebih lanjut dengan menambah fitur lain.

Referensi
- Aggarwal, C. C. (2016). Recommender Systems: The Textbook. Springer.
- Ricci, F., Rokach, L., & Shapira, B. (2015). Recommender Systems Handbook. Springer.
- Mikolov, T., Chen, K., Corrado, G., & Dean, J. (2013). Efficient Estimation of Word Representations in Vector Space. arXiv preprint arXiv:1301.3781.
- Manning, C. D., Raghavan, P., & Schütze, H. (2008). Introduction to Information Retrieval. Cambridge University Press.
- Dataset: Movie Genre from its Poster. Kaggle: https://www.kaggle.com/datasets/neha1703/movie-genre-from-its-poster
