# 📺 Project Overview

Industri hiburan Jepang, khususnya anime, mengalami pertumbuhan signifikan dalam dekade terakhir. Dengan ribuan judul yang tersedia dan terus bertambah setiap tahun, pengguna sering kali kesulitan untuk menemukan anime yang sesuai dengan preferensi mereka. Sistem rekomendasi dapat membantu pengguna menemukan anime yang relevan secara personal, meningkatkan pengalaman pengguna dan memperpanjang waktu keterlibatan mereka dalam platform.

---

# 💼 Business Understanding

## Problem Statements:
- Pengguna kesulitan menemukan anime yang sesuai dengan preferensinya karena terlalu banyak pilihan.
- Platform tidak memiliki sistem yang memberikan rekomendasi berdasarkan perilaku pengguna atau konten.

## Goals:
- Mengembangkan sistem rekomendasi anime yang personal berbasis preferensi pengguna.
- Mengimplementasikan dan membandingkan beberapa pendekatan sistem rekomendasi untuk meningkatkan akurasi hasil rekomendasi.

## Solution Approach:
Solusi yang diusulkan dalam proyek ini mencakup dua pendekatan utama, yaitu:
- Content-Based Filtering (CBF) adalah pendekatan rekomendasi yang digunakan untuk merekomendasikan anime yang serupa dengan anime yang sudah disukai oleh pengguna. Pendekatan ini memanfaatkan atribut konten seperti genre, tema, alur cerita, karakteristik karakter, dan elemen-elemen lain dari anime tersebut untuk menemukan anime lain yang memiliki kesamaan. Sistem ini digunakan dalam proyek ini untuk memberikan rekomendasi yang lebih personal dan relevan bagi pengguna, berdasarkan apa yang sudah mereka sukai sebelumnya.
- Collaborative Filtering (CF) adalah pendekatan yang digunakan untuk memberikan rekomendasi berdasarkan perilaku pengguna lain yang memiliki preferensi serupa. Pendekatan ini memanfaatkan data rating atau interaksi pengguna lain untuk memperkirakan anime yang mungkin disukai oleh pengguna berdasarkan kesamaan dalam preferensi mereka. Dalam proyek ini, CF digunakan untuk memperkaya sistem rekomendasi dengan memberikan rekomendasi berdasarkan tren yang ditemukan di antara komunitas pengguna.

---

## 📁 Dataset

Dataset yang digunakan dalam proyek ini berasal dari [Anime Recommendations Database](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database) yang tersedia di Kaggle.

### Variabel-variabel pada **Anime Recommendation System** dataset adalah sebagai berikut:

#### Anime.csv
- **anime_id**: ID unik dari myanimelist.net yang mengidentifikasi anime.
- **name**: Nama lengkap anime.
- **genre**: Daftar genre yang dipisahkan koma untuk anime ini.
- **type**: Jenis anime, seperti movie, TV, OVA, dll.
- **episodes**: Jumlah episode dalam anime ini (1 jika tipe adalah movie).
- **rating**: Rating rata-rata (skala 1 sampai 10) untuk anime ini.
- **members**: Jumlah anggota komunitas yang tergabung dalam "grup" anime ini.

#### Rating.csv
- **user_id**: ID pengguna yang dihasilkan secara acak sehingga tidak dapat diidentifikasi.
- **anime_id**: ID anime yang telah diberi peringkat oleh pengguna.
- **rating**: Peringkat (skala 1 sampai 10) yang diberikan oleh pengguna ini (-1 jika pengguna menontonnya tetapi tidak memberikan peringkat).

## 📊 Kondisi Data

### 1. Data Anime

Berdasarkan hasil pengecekan data, kondisi data anime adalah:

- **Missing Values**: Terdapat nilai yang hilang pada beberapa kolom, seperti pada kolom `type`, yang berarti jenis anime tidak diketahui untuk beberapa entri.
- **Duplikat**: Data anime bebas dari duplikat, sehingga setiap anime diidentifikasi secara unik dengan `anime_id`.

### 2. Data Rating

Berdasarkan hasil pengecekan data, kondisi data rating sebagai berikut:

- **Missing Values**: Terdapat missing pada kolom `rating`, yang perlu diperhatikan saat analisis lebih lanjut.
- **Nilai Rating -1**: Sebanyak 633,459 entri memiliki rating -1.0, yang menunjukkan bahwa pengguna belum memberikan rating pada anime tersebut. Data ini perlu dihapus agar tidak mengganggu proses model rekomendasi.
- **Duplikat**: Terdapat beberapa entri duplikat pada data rating, yang perlu dibersihkan agar data lebih akurat dan tidak terjadi pengulangan informasi.

---

# 🧹 Data Preprocessing
Dalam pemodelan sistem rekomendasi, preprocessing data adalah langkah penting untuk memastikan data yang digunakan dalam training dan testing sudah bersih, terstruktur, dan relevan. Berikut adalah penjelasan teknik-teknik preprocessing yang dilakukan:

1. Penyaringan Data:
```
anime_refined = anime[anime['episodes'] != 'Unknown'].copy()
```
Data yang memiliki nilai 'Unknown' pada kolom episodes dihapus untuk memastikan hanya data valid yang diproses.

2. Konversi Tipe Data:
```
anime_refined['episodes'] = anime_refined['episodes'].astype(int)
```
Kolom episodes, yang sebelumnya dalam bentuk string, diubah menjadi tipe data integer. 

3. Menghapus Missing Values:
```
anime_refined.dropna(inplace=True)
```
Baris dengan nilai null dihapus untuk mencegah error saat pelatihan model.

4. Kategorisasi Berdasarkan Episode:
Fungsi ``` episode_category ``` digunakan untuk mengubah jumlah episode menjadi kategori seperti "very_short", "short", dst.

5. Kategorisasi Berdasarkan Rating:
Fungsi ``` rating_level ``` digunakan untuk mengelompokkan nilai rating menjadi kualitas seperti "terrible", "good", dst.

6. Kategorisasi Berdasarkan Popularitas:
Fungsi ``` popularity_segment ``` mengelompokkan anime berdasarkan jumlah anggota yang menontonnya menjadi kategori seperti "low", "moderate", "popular".

7. Penggabungan Fitur:
```
anime_refined['combined_features'] = anime_refined['genre'] + ' ' + anime_refined['type'] + ' ' + anime_refined['size_category'] + ' ' + anime_refined['rating_category'] + ' ' + anime_refined['popularity_category']
```
Fitur seperti genre, tipe, kategori ukuran, rating, dan popularitas digabung menjadi satu fitur komposit.

8. Menghapus Kolom yang Tidak Dibutuhkan:
```
anime_model_data = anime_refined.drop(['episodes', 'rating', 'members'], axis=1).reset_index(drop=True)
```
Kolom-kolom yang tidak lagi diperlukan setelah dilakukan kategoriasi (seperti episodes, rating, dan members) dihapus dari dataset.

9. Menghapus Duplikat:
```
anime.drop_duplicates(inplace=True)
rating.drop_duplicates(inplace=True)
```
Menghapus duplikat dari dataset untuk menghindari bias pada model.

10. Mapping User dan Anime:
```
user_map = {uid: idx for idx, uid in enumerate(valid_ratings['user_id'].unique())}
anime_map = {aid: idx for idx, aid in enumerate(valid_ratings['anime_id'].unique())}
```
ID pengguna dan anime dimapping ke bentuk numerik untuk digunakan dalam algoritma berbasis matriks.

11. Normalisasi Rating:
```valid_ratings['normalized'] = valid_ratings['rating'].apply(lambda r: (r - rmin) / (rmax - rmin))
```
Rating dinormalisasi ke rentang 0-1 untuk memperlancar proses pembelajaran model.

12. Pemisahan Data Latih dan Uji:
```
X_train, X_test, y_train, y_test = train_test_split(X_matrix, y_vector, test_size=0.2, random_state=42)
```
Dataset dibagi menjadi data pelatihan dan data pengujian (80% untuk pelatihan, 20% untuk pengujian). 







