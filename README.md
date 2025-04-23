# 📺 Project Overview
Industri hiburan Jepang, khususnya anime, mengalami pertumbuhan signifikan dalam dekade terakhir. Dengan ribuan judul yang tersedia dan terus bertambah setiap tahun, pengguna sering kali kesulitan untuk menemukan anime yang sesuai dengan preferensi mereka. Sistem rekomendasi dapat membantu pengguna menemukan anime yang relevan secara personal, meningkatkan pengalaman pengguna dan memperpanjang waktu keterlibatan mereka dalam platform.

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
