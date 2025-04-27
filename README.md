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

- **Missing Values**: Terdapat nilai yang hilang pada beberapa kolom, seperti pada kolom `type`.
- **Tipe Data**: Terdapat tipe data yang tidak sesuai pada kolom `episodes`.
- **Duplikat**: Data anime bebas dari duplikat, sehingga setiap anime diidentifikasi secara unik dengan `anime_id`.

### 2. Data Rating

Berdasarkan hasil pengecekan data, kondisi data rating sebagai berikut:

- **Missing Values**: Terdapat missing pada kolom `rating`, yang perlu diatasi.
- **Nilai Rating -1**: Sebanyak 633,459 entri memiliki rating -1.0, menunjukkan bahwa pengguna belum memberikan rating pada anime tersebut. Data ini perlu dihapus agar tidak mengganggu proses model rekomendasi.
- **Duplikat**: Terdapat beberapa entri duplikat pada data rating, yang perlu dibersihkan agar tidak terjadi pengulangan informasi.

---

# 🧹 Data Preprocessing
Dalam pemodelan sistem rekomendasi, preprocessing data adalah langkah penting untuk memastikan data yang digunakan dalam training dan testing sudah bersih, terstruktur, dan relevan. Berikut adalah penjelasan teknik-teknik preprocessing yang dilakukan:

## 1. Penyaringan Data:
```
anime_refined = anime[anime['episodes'] != 'Unknown'].copy()
```
Data yang memiliki nilai 'Unknown' pada kolom episodes dihapus untuk memastikan hanya data valid yang diproses.

## 2. Konversi Tipe Data:
```
anime_refined['episodes'] = anime_refined['episodes'].astype(int)
```
Kolom episodes, yang sebelumnya dalam bentuk string, diubah menjadi tipe data integer. 

## 3. Menghapus Missing Values:
```
anime_refined.dropna(inplace=True)
```
Baris dengan nilai null dihapus untuk mencegah error saat pelatihan model.

## 4. Kategorisasi Berdasarkan Episode:
Fungsi ``` episode_category ``` digunakan untuk mengubah jumlah episode menjadi kategori seperti "very_short", "short", dst.

## 5. Kategorisasi Berdasarkan Rating:
Fungsi ``` rating_level ``` digunakan untuk mengelompokkan nilai rating menjadi kualitas seperti "terrible", "good", dst.

## 6. Kategorisasi Berdasarkan Popularitas:
Fungsi ``` popularity_segment ``` mengelompokkan anime berdasarkan jumlah anggota yang menontonnya menjadi kategori seperti "low", "moderate", "popular".

## 7. Penggabungan Fitur:
```
anime_refined['combined_features'] = anime_refined['genre'] + ' ' + anime_refined['type'] + ' ' + anime_refined['size_category'] + ' ' + anime_refined['rating_category'] + ' ' + anime_refined['popularity_category']
```
Fitur seperti genre, tipe, kategori ukuran, rating, dan popularitas digabung menjadi satu fitur komposit.

## 8. Menghapus Kolom yang Tidak Dibutuhkan:
```
anime_model_data = anime_refined.drop(['episodes', 'rating', 'members'], axis=1).reset_index(drop=True)
```
Kolom-kolom yang tidak lagi diperlukan setelah dilakukan kategoriasi (seperti episodes, rating, dan members) dihapus dari dataset.

## 9. Menghapus Duplikat:
```
anime.drop_duplicates(inplace=True)
rating.drop_duplicates(inplace=True)
```
Menghapus duplikat dari dataset untuk menghindari bias pada model.

## 10. Mapping User dan Anime:
```
user_map = {uid: idx for idx, uid in enumerate(valid_ratings['user_id'].unique())}
anime_map = {aid: idx for idx, aid in enumerate(valid_ratings['anime_id'].unique())}
```
ID pengguna dan anime dimapping ke bentuk numerik untuk digunakan dalam algoritma berbasis matriks.

## 11. Normalisasi Rating:
```
valid_ratings['normalized'] = valid_ratings['rating'].apply(lambda r: (r - rmin) / (rmax - rmin))
```
Rating dinormalisasi ke rentang 0-1 untuk memperlancar proses pembelajaran model.

## 12. Pemisahan Data Latih dan Uji:
```
X_train, X_test, y_train, y_test = train_test_split(X_matrix, y_vector, test_size=0.2, random_state=42)
```
Dataset dibagi menjadi data pelatihan dan data pengujian (80% untuk pelatihan, 20% untuk pengujian). 

---

# 🤖 Modeling

Pada bagian ini, dua pendekatan sistem rekomendasi yang digunakan adalah:

## 1. Content-Based Filtering (CB)

### Cara Kerja:

Content-Based Filtering merekomendasikan anime berdasarkan kemiripan konten (fitur) antara anime yang pernah disukai/cari pengguna dan anime lain. Model ini fokus pada karakteristik item itu sendiri, bukan perilaku pengguna lain.

### Tahapan:

- Ekstrak fitur konten dari anime (genre, tipe, rating, ukuran, popularitas).

- Gunakan teknik TF-IDF Vectorizer untuk merepresentasikan fitur dalam bentuk numerik.

- Hitung kemiripan antar anime menggunakan Cosine Similarity.

- Rekomendasikan anime yang paling mirip.

### Parameter yang Digunakan:

- **TfidfVectorizer**: Digunakan untuk mengubah teks menjadi representasi numerik.
  - `stop_words='english'`: Menghapus kata-kata umum dalam bahasa Inggris.
- **Cosine Similarity**: Untuk menghitung kemiripan antar anime.

## Hasil Top-5 Rekomendasi untuk Anime "Doraemon":

| No | Name                                          | Genre                                 | Type | Size Category | Rating Category | Popularity Category | Similarity |
|----|-----------------------------------------------|---------------------------------------|------|---------------|-----------------|---------------------|------------|
| 1  | TaoTao Ehonkan Sekai Doubutsu Banashi         | Adventure, Comedy, Fantasy, Kids      | TV   | very_short    | good            | low                 | 0.906548   |
| 2  | Happy☆Lucky Bikkuriman                        | Adventure, Comedy, Fantasy, Kids      | TV   | very_short    | good            | low                 | 0.906548   |
| 3  | Kekkaishi                                     | Adventure, Comedy, Fantasy, Shounen   | TV   | very_short    | good            | low                 | 0.898993   |
| 4  | Doraemon Movie 31: Shin Nobita to Tetsujin Hei... | Adventure, Comedy, Fantasy, Kids, Shounen | Movie | very_short  | good            | low                 | 0.879014   |
| 5  | Doraemon Movie 28: Nobita to Midori no Kyojin Den | Adventure, Comedy, Fantasy, Kids, Shounen | Movie | very_short  | good            | low                 | 0.879014   |


## 2. Collaborative Filtering (CF)

### Deskripsi Cara Kerja:

Collaborative Filtering memberikan rekomendasi berdasarkan interaksi pengguna, dengan cara menemukan pola dari pengguna lain yang memiliki preferensi serupa. Model ini mencari kemiripan antar pengguna berdasarkan anime yang sudah mereka beri rating. Dengan demikian, rekomendasi didasarkan pada apa yang disukai oleh pengguna yang memiliki pola preferensi serupa.

### Tahapan:

1. Setiap pengguna dan anime direpresentasikan dalam bentuk embedding vektor berdimensi 50.
2. Model **RecommenderNet** menghitung prediksi rating antara pengguna dan anime.
3. Rekomendasi diberikan berdasarkan prediksi rating tertinggi untuk anime yang belum pernah diberi rating oleh pengguna.

### Parameter yang Digunakan:

- **Embedding Size** = 50
- **Loss Function** = Mean Squared Error
- **Optimizer** = Adam
- **Batch Size** = 64
- **Epochs** = 5
- **Validation Split** = 0.2

### Hasil Top-10 Rekomendasi untuk User ID 17440:

| Anime ID | Name                                      | Genre                                                         |
|----------|-------------------------------------------|---------------------------------------------------------------|
| 2762     | Igano Kabamaru                           | Adventure, Comedy, Romance, Shounen                           |
| 22693    | Lady Jewelpet                            | Fantasy, Magic, Romance, Shoujo                               |
| 30870    | Ajin Part 3: Shougeki                    | Action, Horror, Mystery, Seinen, Supernatural                 |
| 1116     | Junkers Come Here                        | Drama, Slice of Life                                           |
| 751      | Bomberman Jetters                        | Action, Adventure, Comedy, Mystery, Sci-Fi, Shounen            |
| 1549     | 1000-nen Joou: Queen Millennia           | Adventure, Drama, Fantasy, Sci-Fi                              |
| 30651    | Itoshi no Muco                           | Comedy, Slice of Life                                          |
| 17127    | Chokkyuu Hyoudai Robot Anime: Straight Title | Comedy, Mecha                                                |
| 2727     | Sweet Valerian                           | Action, Comedy, Magic, School, Shoujo                          |
| 4150     | Cosmos Pink Shock                        | Comedy, Parody, Sci-Fi, Space                                  |


# 📈 Evaluation
Metrik evaluasi yang digunakan:

1. Collaborative Filtering:
Evaluasi menggunakan MAE (Mean Absolute Error) dan RMSE (Root Mean Squared Error).

![Image](https://github.com/user-attachments/assets/7c18a5d0-f8fc-4220-b461-7bd46f1bca4e)

Dari gambar diatas menunjukkan hasil yang baik dengan MAE 0.1246 dan RMSE 0.1640, yang menunjukkan bahwa prediksi rating dari model cukup akurat. Model ini dapat digunakan untuk memberikan rekomendasi yang relevan kepada pengguna berdasarkan interaksi mereka sebelumnya.

2. Content-Based Filtering:
Karena CBF tidak menggunakan data eksplisit dari pengguna, maka evaluasi seperti RMSE (Root Mean Squared Error) atau MAE (Mean Absolute Error) tidak bisa digunakan langsung di sini. Sebagai gantinya, evaluasi bisa dilakukan dengan uji coba dan validasi hasil rekomendasi secara manual seperti yang terlihat pada gambar dibawah ini:

![Image](https://github.com/user-attachments/assets/cbcb9104-f76d-4ff6-ae2d-f64295e78b1e)

Output yang dihasilkan adalah daftar anime yang paling mirip dengan anime Doraemon yang sesuai dengan judul yang diberikan. Kemiripan dihitung menggunakan cosine similarity, lalu hasilnya diurutkan dan ditampilkan 5 anime teratas beserta nilai kemiripan mereka. Berdasarkan ouput tersebut maka model dianggap berhasil secara konseptual.
