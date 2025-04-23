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
Solusi 1: Content-Based Filtering
Merekomendasikan anime berdasarkan kesamaan genre, studio, atau fitur lain dengan anime yang disukai pengguna.

Solusi 2: Collaborative Filtering
Menggunakan informasi rating pengguna lain yang memiliki kesamaan preferensi untuk memberikan rekomendasi.

Solusi 3: Hybrid System
Menggabungkan content-based dan collaborative untuk mengatasi kelemahan masing-masing pendekatan.

---

## 📁 Dataset

Dataset yang digunakan berisi informasi mengenai anime dan genre-nya. Format CSV diharapkan memiliki kolom minimal berikut:

- `anime_id`: ID unik anime
- `name`: Nama anime
- `genre`: Daftar genre yang dipisahkan koma

Contoh isi:

| anime_id | name       | genre                               |
|----------|------------|-------------------------------------|
| 1        | Naruto     | Action, Adventure, Martial Arts     |
| 2        | One Piece  | Action, Adventure, Comedy           |

---

