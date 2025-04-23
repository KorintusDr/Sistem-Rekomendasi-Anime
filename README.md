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

Dataset **Anime.csv** berisi informasi mengenai berbagai anime yang ada di platform myanimelist.net. Berdasarkan hasil pengecekan data, beberapa hal yang perlu diperhatikan mengenai kondisi data anime adalah:

- **Missing Values**: Terdapat nilai yang hilang pada beberapa kolom, terutama pada kolom `type`, yang berarti jenis anime tidak diketahui untuk beberapa entri.
- **Duplikat**: Data anime bebas dari duplikat, sehingga setiap anime diidentifikasi secara unik dengan `anime_id`.
- **Distribusi Rating**: Mayoritas anime memiliki rating yang cukup baik (rata-rata sekitar 6.47). Namun, ada beberapa anime dengan rating rendah (di bawah 4) dan sangat tinggi (di atas 9).
- **Anggota**: Kolom `members` menunjukkan distribusi yang tidak merata, dengan beberapa anime memiliki jumlah anggota yang sangat besar, sementara sebagian besar anime lainnya memiliki jumlah anggota yang jauh lebih sedikit.

### 2. Data Rating

Dataset **Rating.csv** mencatat rating yang diberikan oleh pengguna untuk anime tertentu. Kondisi data rating sebagai berikut:

- **Missing Values**: Terdapat satu nilai missing pada kolom `rating`, yang perlu diperhatikan saat analisis lebih lanjut.
- **Nilai Rating -1**: Sebanyak 633,459 entri memiliki rating -1.0, yang menunjukkan bahwa pengguna belum memberikan rating pada anime tersebut. Data ini perlu dihapus agar tidak mengganggu proses model rekomendasi.
- **Duplikat**: Terdapat beberapa entri duplikat pada data rating, yang perlu dibersihkan agar data lebih akurat dan tidak terjadi pengulangan informasi.
- **Rentang Rating**: Rating anime berada pada rentang antara -1 hingga 10, dengan mayoritas pengguna memberikan rating antara 7 dan 9. Sebagian besar pengguna cenderung memberikan rating positif terhadap anime yang mereka tonton.

### 3. Statistik Deskriptif

- **Anime**: Dataset anime terdiri dari 12,294 entri dengan kolom numerik utama berupa `anime_id`, `rating`, dan `members`. Rata-rata rating anime adalah 6.47, dan kolom `members` menunjukkan ketimpangan besar, dengan sebagian anime memiliki jumlah pengikut sangat tinggi, sementara yang lainnya jauh lebih sedikit.
- **Rating**: Dataset rating terdiri dari 1.607.823 entri. Rata-rata rating pengguna adalah 6.14, dengan banyak nilai yang mendekati 7.00. Namun, standar deviasi cukup tinggi, mengindikasikan variasi yang signifikan dalam penilaian anime.

### 4. Jenis Anime

Berdasarkan jenis anime, dapat disimpulkan bahwa:

- **TV** adalah jenis anime yang paling banyak, mencerminkan preferensi pengguna untuk menonton serial anime.
- **OVA** dan **Movie** juga cukup populer, meskipun jumlahnya lebih sedikit dibandingkan dengan TV.
- **ONA** dan **Music** memiliki jumlah yang jauh lebih kecil, menunjukkan bahwa meskipun ada peminatnya, keduanya tidak sebanyak jenis anime lainnya.

### 5. Genre Anime

Dari segi genre, data menunjukkan bahwa:

- **Comedy** adalah genre paling populer, diikuti oleh **Action** dan **Adventure**.
- Genre seperti **Fantasy**, **Sci-Fi**, **Drama**, dan **Shounen** juga cukup diminati.
- Beberapa genre seperti **Romance** dan **School** menunjukkan minat yang lebih rendah, namun tetap memiliki penggemar setia.

### 6. Rata-Rata Rating Berdasarkan Genre

Berdasarkan analisis rata-rata rating berdasarkan genre:

- **Josei** mencatatkan rating tertinggi di antara genre lainnya, diikuti oleh **Thriller** dan **Mystery**, yang memiliki rating rata-rata tinggi.
- Genre **Shounen**, **Police**, **Psychological**, dan **Military** juga menunjukkan rating yang cukup baik, meskipun sedikit lebih rendah dibandingkan dengan genre lainnya.
- **Supernatural** dan **Romance** berada di posisi tengah dengan rating yang hampir serupa.

Secara keseluruhan, dataset menunjukkan ketimpangan dalam hal popularitas anime, dengan beberapa anime yang sangat populer dibandingkan dengan yang lainnya. Data juga memiliki beberapa masalah terkait dengan missing values, rating yang belum diberikan, dan entri duplikat yang perlu dibersihkan lebih lanjut sebelum membangun model sistem rekomendasi.

---

