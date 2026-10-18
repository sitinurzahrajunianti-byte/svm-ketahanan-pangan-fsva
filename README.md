# Klasifikasi Status Ketahanan Pangan Kab/Kota Indonesia dengan SVM

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/sitinurzahrajunianti-byte/svm-ketahanan-pangan-fsva/blob/main/Model_SVM_pada_data_FSVA.ipynb)

Proyek ini membangun model **Support Vector Machine (SVM)** untuk memprediksi kelompok ketahanan pangan 514 kabupaten/kota di Indonesia dari 12 indikator, memakai data **Peta Ketahanan dan Kerentanan Pangan (FSVA) 2025**.

## Tujuan
Mengetahui apakah indikator mentah ketahanan pangan (kemiskinan, stunting, cadangan beras, dan lainnya) cukup untuk memetakan status ketahanan pangan suatu daerah, dan indikator mana yang paling menentukan.

## Data
- **Sumber:** Badan Pangan Nasional, FSVA Nasional 2025.
- **Cakupan:** 514 kabupaten/kota di 38 provinsi, tahun 2025.
- **Fitur (12 indikator mentah):** NCPR, ENERGI, PROHE, CBPK, MISKIN, CVHARGA, POU, RLSP, TNPAIR, AMANPANGN, PPH, STUNTING.
- **Target:** kelompok ketahanan pangan (PKOMP) 1–6, dari Sangat Rentan sampai Sangat Tahan. Dibuat juga versi 2 kelas: **Rentan** (kelompok 1–3) dan **Tahan** (kelompok 4–6).
- Kolom skor (P*), IKP, dan peringkat **tidak dipakai** sebagai fitur untuk menghindari *data leakage*.

> File data tidak disertakan di repositori ini. Silakan unduh dari sumber resmi Badan Pangan Nasional.

## Metode
1. **Eksplorasi data:** kualitas data, statistik deskriptif, outlier, korelasi, dan perbandingan indikator antar kelompok.
2. **Preprocessing:** standarisasi fitur (`StandardScaler`) di dalam pipeline.
3. **Penanganan data tidak seimbang:** dibandingkan tiga strategi, yaitu tanpa penanganan, `class_weight`, dan SMOTE.
4. **Tuning:** `GridSearchCV` (kernel RBF dan linear, nilai C dan gamma).
5. **Evaluasi:** *nested cross-validation* (5 fold luar, 3 fold dalam) pada seluruh data, dengan metrik utama **F1-macro**.
6. **Interpretasi:** analisis kesalahan prediksi, *permutation importance*, dan visualisasi batas keputusan.

## Hasil utama

| Tugas | Strategi terbaik | Akurasi | F1-macro |
|---|---|---|---|
| 6 kelas | Tanpa penanganan | 92,2% | 0,900 |
| 2 kelas (Rentan vs Tahan) | SMOTE | 99,0% | 0,982 |

- Dari 40 daerah yang salah prediksi pada model 6 kelas, **semuanya meleset hanya satu kelompok**.
- Indikator paling berpengaruh: **NCPR, CBPK, PROHE, dan MISKIN**.
- Pada 6 kelas, SMOTE dan `class_weight` tidak lebih baik daripada tanpa penanganan.
- Provinsi dengan proporsi daerah rentan tertinggi: Papua Pegunungan, Papua, Papua Tengah, NTT, dan Maluku.

## Keterbatasan
- Kelompok ketahanan pangan di data asli **dihitung dari indikator yang sama**, sehingga model pada dasarnya mempelajari ulang rumus resmi. Akurasi tinggi sebagian berasal dari hal ini.
- Data hanya satu tahun (2025), sehingga tidak menangkap perubahan antar waktu.
- Hasil menggambarkan hubungan (korelasi), bukan sebab-akibat.
- Kelompok rentan berjumlah kecil (19–42 daerah), sehingga metrik per kelompok kurang stabil.

## Cara menjalankan
1. Klik tombol **Open in Colab** di atas.
2. Unggah file data FSVA 2025 saat diminta.
3. Pilih **Runtime → Run all**.

## Teknologi
Python, pandas, scikit-learn, imbalanced-learn, matplotlib, seaborn.
