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


---

