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

### 1. Penyaringan Data:
```
anime_refined = anime[anime['episodes'] != 'Unknown'].copy()
```
Data yang memiliki nilai 'Unknown' pada kolom episodes dihapus. Ini bertujuan untuk memastikan bahwa hanya data yang valid dan memiliki nilai numerik pada kolom episodes yang akan diproses lebih lanjut.

2. Konversi Tipe Data:
anime_refined['episodes'] = anime_refined['episodes'].astype(int)
Kolom episodes, yang sebelumnya mungkin dalam bentuk string, diubah menjadi tipe data integer. Hal ini dilakukan agar data dapat digunakan dalam analisis atau pemodelan numerik.

3. Menghapus Missing Values:
```
anime_refined.dropna(inplace=True)
```
Baris yang mengandung nilai null dihapus. Ini penting untuk memastikan bahwa tidak ada data yang hilang dalam proses pelatihan model, karena sebagian besar algoritma machine learning tidak dapat menangani nilai yang hilang.

4. Penyusunan Kategorisasi Berdasarkan Episode:
Fungsi episode_category digunakan untuk mengklasifikasikan jumlah episode anime menjadi kategori seperti "very_short", "short", dan seterusnya berdasarkan rentang tertentu. Ini memungkinkan data numerik yang luas (jumlah episode) diubah menjadi kategori yang lebih mudah dianalisis dan dimanfaatkan dalam model.

5. Penyusunan Kategorisasi Berdasarkan Rating:
Fungsi rating_level mengklasifikasikan nilai rating anime ke dalam beberapa level kualitas, seperti "terrible", "good", dan sebagainya, berdasarkan rentang nilai yang ditentukan. Ini mengubah data rating numerik menjadi kategori.

6. Penyusunan Kategorisasi Berdasarkan Popularitas:
Fungsi popularity_segment mengkategorikan anime berdasarkan jumlah anggota yang menonton anime tersebut. Data numerik jumlah anggota ini dikelompokkan menjadi kategori seperti "low", "moderate", "popular", dan sebagainya.

7. Penggabungan Fitur:
anime_refined['combined_features'] = anime_refined['genre'] + ' ' + anime_refined['type'] + ' ' + anime_refined['size_category'] + ' ' + anime_refined['rating_category'] + ' ' + anime_refined['popularity_category']
Fitur-fitur yang relevan seperti genre, tipe, kategori ukuran, rating, dan popularitas digabungkan menjadi satu kolom combined_features. Ini dapat digunakan untuk analisis lebih lanjut atau sebagai input ke dalam model rekomendasi berbasis konten.

8. Menghapus Kolom yang Tidak Dibutuhkan:
anime_model_data = anime_refined.drop(['episodes', 'rating', 'members'], axis=1).reset_index(drop=True)
Kolom-kolom yang tidak lagi diperlukan setelah dilakukan kategoriasi (seperti episodes, rating, dan members) dihapus dari dataset. Ini bertujuan untuk mengurangi dimensi data dan memfokuskan hanya pada fitur yang relevan untuk pemodelan.

9. Menghapus Duplikat:
anime.drop_duplicates(inplace=True) dan rating.drop_duplicates(inplace=True)
Duplikat pada dataset anime dan rating dihapus untuk menghindari pengaruh yang tidak diinginkan pada model. Duplikat dapat memperkenalkan bias dan menyebabkan model belajar hal yang salah.

10. Penanganan Kode Pengguna dan Anime:
user_map = {uid: idx for idx, uid in enumerate(valid_ratings['user_id'].unique())} dan anime_map = {aid: idx for idx, aid in enumerate(valid_ratings['anime_id'].unique())}
Pengguna dan anime di-mapping ke ID numerik untuk memungkinkan pemodelan berbasis indeks numerik daripada ID asli, yang lebih mudah digunakan dalam algoritma berbasis matriks seperti collaborative filtering.

11. Normalisasi Rating:
valid_ratings['normalized'] = valid_ratings['rating'].apply(lambda r: (r - rmin) / (rmax - rmin))
Normalisasi dilakukan pada rating sehingga berada dalam rentang 0 hingga 1. Ini diperlukan agar model dapat mengolah data dengan cara yang lebih terstandarisasi dan tidak ada nilai yang terlalu besar atau kecil yang bisa mendistorsi pembelajaran model.

12. Pemisahan Data Latih dan Uji:
X_train, X_test, y_train, y_test = train_test_split(X_matrix, y_vector, test_size=0.2, random_state=42)
Dataset dibagi menjadi data pelatihan dan data pengujian (80% untuk pelatihan, 20% untuk pengujian). Pembagian ini penting untuk evaluasi model yang objektif dan untuk mencegah overfitting.







