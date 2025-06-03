# Laporan Proyek Machine Learning - Robert Varian

## Project Overview

Perkembangan pesat teknologi digital memungkinkan ribuan lowongan pekerjaan diunggah secara online setiap hari. Namun, pencari kerja seringkali kesulitan untuk menemukan pekerjaan yang sesuai dengan keterampilan dan preferensi mereka karena banyaknya pilihan yang tersedia. Hal ini menyebabkan kebutuhan akan sistem rekomendasi pekerjaan yang mampu menyaring dan menyajikan peluang kerja yang relevan secara personal.

Menurut riset oleh Ricci et al. (2015), sistem rekomendasi dapat meningkatkan pengalaman pengguna dengan memberikan pilihan yang sesuai berdasarkan preferensi sebelumnya. Oleh karena itu, pembangunan sistem rekomendasi pekerjaan yang efektif sangat penting untuk membantu pencari kerja dalam mempercepat proses pencarian dan pengambilan keputusan.

## Business Understanding

### Problem Statements
1. Bagaimana membantu pencari kerja menemukan pekerjaan yang relevan dan sesuai dengan profil mereka?
2. Bagaimana menyajikan rekomendasi pekerjaan berdasarkan informasi pekerjaan yang tersedia, mengingat minimnya data interaksi pengguna?
3. Bagaimana mengukur efektivitas sistem rekomendasi dalam memberikan saran pekerjaan?

### Goals
1. Membangun sistem rekomendasi pekerjaan berbasis konten yang dapat merekomendasikan pekerjaan serupa berdasarkan deskripsi dan judul pekerjaan.
2. Mengimplementasikan metode Content-Based Filtering menggunakan TF-IDF dan cosine similarity.
3. Mengevaluasi performa sistem rekomendasi menggunakan metrik relevansi (precision dan recall secara konseptual).

### Solution statements
- Content-Based Filtering: Merekomendasikan pekerjaan berdasarkan kemiripan konten (job title, company, job description) menggunakan TF-IDF dan cosine similarity.
- Collaborative Filtering (untuk proyek lanjutan): Menggunakan data interaksi pengguna (lamaran atau rating) untuk rekomendasi personalisasi.

## Data Understanding

Dataset yang digunakan adalah data pekerjaan yang diunduh dari Kaggle: Jobs Data for Recommender Systems. Dataset ini berisi sekitar [jumlah baris dataset] entri pekerjaan dengan beberapa fitur seperti:
- job_title : Judul pekerjaan, misalnya "Data Scientist"
- company_name : Nama perusahaan penyedia pekerjaan
- job_description : Deskripsi detail pekerjaan
- Fitur lain seperti lokasi, kategori pekerjaan, dll.

Dataset mengandung data teks yang cukup beragam, sehingga perlu dilakukan pembersihan dan transformasi sebelum digunakan dalam model.

## Data Preparation

Beberapa tahapan data preparation yang dilakukan:
- Mengisi nilai kosong (null) pada kolom teks dengan string kosong agar tidak mengganggu proses ekstraksi fitur.
- Menggabungkan kolom job_title, company_name, dan job_description menjadi satu fitur teks gabungan untuk representasi konten pekerjaan.
- Membersihkan teks dari karakter atau format yang tidak diperlukan.
- Menggunakan TF-IDF Vectorizer untuk mengubah teks gabungan menjadi representasi vektor numerik.
- Menghitung matriks kesamaan kosinus antar pekerjaan untuk menentukan tingkat kemiripan pekerjaan satu sama lain.
Tahapan ini penting agar data teks yang beragam dapat diproses menjadi bentuk yang dapat dianalisis oleh algoritma rekomendasi.

## Modeling

Model utama yang dikembangkan adalah Content-Based Filtering yang merekomendasikan pekerjaan berdasarkan kemiripan konten.
- Input: judul pekerjaan
- Proses: cari pekerjaan lain dengan vektor TF-IDF yang paling mirip berdasarkan cosine similarity.
- Output: daftar 10 pekerjaan dengan tingkat kemiripan tertinggi.

Kelebihan
- Tidak memerlukan data interaksi pengguna, cocok untuk dataset dengan data pengguna terbatas.
- Rekomendasi didasarkan pada konten pekerjaan, sehingga relevan dengan deskripsi pekerjaan.

Kekurangan
- Tidak mempertimbangkan preferensi personal pengguna.
- Rekomendasi terbatas pada kemiripan konten, kurang dapat mengeksplorasi pekerjaan yang berbeda tapi relevan.

## Evaluation
Evaluasi utama dilakukan secara konseptual dengan pengukuran precision dan recall pada rekomendasi berdasarkan similarity score.
- Precision mengukur proporsi rekomendasi yang relevan.
- Recall mengukur proporsi pekerjaan relevan yang berhasil direkomendasikan.
Karena dataset tidak mengandung feedback pengguna, evaluasi kuantitatif menggunakan metrik RMSE atau MAE seperti pada Collaborative Filtering tidak dapat dilakukan secara langsung.

**---Ini adalah bagian akhir laporan---**

Referensi
Aggarwal, C. C. (2016). Recommender Systems: The Textbook. Springer.

Ricci, F., Rokach, L., & Shapira, B. (2015). Recommender Systems Handbook. Springer.

Mikolov, T., Chen, K., Corrado, G., & Dean, J. (2013). Efficient Estimation of Word Representations in Vector Space. arXiv preprint arXiv:1301.3781.

Manning, C. D., Raghavan, P., & Schütze, H. (2008). Introduction to Information Retrieval. Cambridge University Press.

Dataset Grocery Item Classification, Kaggle: https://www.kaggle.com/datasets/kaustubhb999/grocery-item-classification
