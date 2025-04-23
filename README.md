# 🎌 Anime Recommendation System

Sistem rekomendasi anime sederhana berbasis **Content-Based Filtering (CBF)** menggunakan **cosine similarity** terhadap genre anime. Proyek ini membantu pengguna menemukan anime yang mirip dengan yang sudah mereka sukai sebelumnya.

---

📺 Sistem Rekomendasi Anime
Sistem ini dirancang untuk merekomendasikan anime kepada pengguna berdasarkan preferensi dan interaksi mereka sebelumnya. Proyek ini mencakup tahapan dari pemahaman bisnis hingga evaluasi model yang digunakan untuk memberikan rekomendasi yang relevan dan personal.

📌 Project Overview
Sistem rekomendasi telah menjadi tulang punggung berbagai platform hiburan modern seperti Netflix, YouTube, dan Spotify. Dalam proyek ini, kami membangun sistem rekomendasi anime menggunakan data dari platform MyAnimeList, memanfaatkan teknik filtering dan analisis data untuk memberikan saran anime kepada pengguna berdasarkan pola interaksi mereka.

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

## 🧠 Algoritma

Sistem ini menggunakan pendekatan **Content-Based Filtering**, yaitu merekomendasikan anime lain yang memiliki **kemiripan konten** (genre) dengan anime pilihan pengguna.

### 🔎 TF-IDF Vectorization

Setiap genre diubah menjadi representasi vektor menggunakan **TF-IDF (Term Frequency-Inverse Document Frequency)**:

- Menangkap pentingnya kata (genre) dalam konteks dokumen (anime).
- Mengabaikan kata-kata umum (stop words) agar hasil lebih relevan.

### 📐 Cosine Similarity

Kemiripan antar anime dihitung menggunakan **cosine similarity**, yang mengukur sudut antar dua vektor.

\[
\text{Cosine Similarity} = \frac{A \cdot B}{\|A\| \|B\|}
\]

Nilai cosine similarity berkisar antara 0 (tidak mirip) hingga 1 (identik).

---

## ⚙️ Instalasi dan Penggunaan

### 1. Clone Repository

```bash
git clone https://github.com/username/anime-recommender.git
cd anime-recommender
