# Laporan Proyek Machine Learning - Masahiro Gerarudo Yamazaki

## Project Overview

**Latar Belakang**

Di era digital saat ini, layanan streaming on-demand seperti Netflix, Disney+, dan Amazon Prime Video telah mengubah cara orang menikmati hiburan. Dengan menyediakan ribuan film dan serial dari berbagai genre, bahasa, dan periode produksi, platform ini memberi pengguna kebebasan untuk memilih tontonan sesuai keinginan. Namun, kebebasan ini justru menimbulkan tantangan baru berupa information overload—yakni kondisi di mana terlalu banyak pilihan membuat pengguna kesulitan dalam menentukan pilihan.

Penelitian kualitatif terbaru yang dipublikasikan oleh Springer (2024) mengangkat isu ini dalam konteks penggunaan Netflix. Dari hasil wawancara, terungkap bahwa meskipun fitur personalisasi membantu dalam menyesuaikan konten dengan preferensi pengguna, banyak dari mereka tetap merasa bingung dan terbebani oleh jumlah pilihan yang tersedia di halaman utama.

“Walaupun personalisasi meningkatkan keterlibatan, pengguna tetap merasa kewalahan karena terlalu banyak pilihan.”
— Springer, 2024

Kondisi ini tidak hanya menurunkan kualitas pengalaman pengguna, tetapi juga berdampak pada lamanya interaksi dengan platform, tingkat loyalitas pengguna, hingga keputusan menonton yang terburu-buru atau bahkan batal dilakukan. Dalam perspektif bisnis, hal ini dapat menurunkan tingkat retensi dan efektivitas sistem rekomendasi.

Meski sistem personalisasi sudah menjadi bagian penting dari layanan streaming, studi Springer (2024) menunjukkan bahwa kehadiran terlalu banyak pilihan tetap menjadi kendala. Ini menandakan bahwa keberhasilan sistem rekomendasi bukan hanya soal ketepatan konten, melainkan juga tentang bagaimana menyajikannya dengan cara yang sederhana dan tidak membebani pikiran pengguna.

Selain itu, Zhang dan rekan-rekannya (2019) menyoroti pentingnya sistem rekomendasi dalam platform digital, karena sistem ini membantu pengguna menavigasi pilihan yang melimpah dan menemukan konten yang sesuai dengan minat mereka.

“Sistem rekomendasi memegang peran penting di platform daring karena membantu pengguna menemukan item yang relevan di antara banyaknya pilihan.”
— Zhang et al., 2019

----
Proyek ini bertujuan untuk membangun **sistem rekomendasi film** dengan memanfaatkan dataset **MovieLens 25M**, menggunakan tiga pendekatan model :

1. **Content-Based Filtering (CBF)** – berdasarkan kemiripan konten (genre).
2. **Collaborative Filtering (CF)** – menggunakan embedding dengan neural network untuk memahami hubungan antar pengguna dan item.
3. **Hybrid Model** – menggabungkan kedua pendekatan di atas untuk mendapatkan keseimbangan antara akurasi dan keberagaman.

Setiap pendekatan dievaluasi menggunakan tiga metrik utama:

- **Mean Similarity / Intra-List Similarity (ILS):** sejauh mana item dalam daftar rekomendasi mirip satu sama lain.
- **Diversity:** sejauh mana rekomendasi yang diberikan beragam dan tidak homogen.
- **Coverage:** seberapa besar proporsi item dalam katalog yang direkomendasikan kepada semua pengguna.

---

### Mengapa Masalah Ini Harus Diselesaikan?

- Pengguna menghadapi kebingungan dalam memilih film karena pilihan yang terlalu banyak.
- Platform digital perlu meningkatkan pengalaman pengguna untuk memperpanjang waktu interaksi.
- Solusi personalisasi rekomendasi dapat mendorong **retensi pengguna** dan **loyalitas terhadap platform**.
- Pendekatan hybrid memungkinkan sistem menjadi lebih adaptif dan efisien untuk berbagai jenis pengguna.

Berdasarkan latar belakang tersebut, sistem rekomendasi yang dirancang dalam proyek ini diharapkan tidak hanya memberikan **rekomendasi yang relevan dan personal**, tetapi juga **beragam dan mencakup lebih banyak film** dalam katalog, sehingga mendorong pengguna untuk mengeksplorasi lebih jauh. Oleh karena itu, proyek ini berfokus pada pengembangan sistem rekomendasi yang mampu menyeimbangkan antara akurasi personalisasi dan keragaman pilihan, agar pengguna tidak lagi merasa terjebak dalam choice overload.

---

### Referensi:
- Springer. (2024). Choice overload and personalization in streaming services: A qualitative study on Netflix users.(https://link.springer.com/article/10.1007/s12646-024-00807-0)
- Zhang, S., Yao, L., Sun, A., & Tay, Y. (2019). Deep learning based recommender system: A survey and new perspectives. ACM Computing Surveys (CSUR), 52(1), Article 5, 1–38. (https://doi.org/10.1145/3285029)


## Business Understanding

Bagian laporan ini mencakup:

### Problem Statements

Menjelaskan pernyataan masalah latar belakang:
1. **Bagaimana merekomendasikan film yang relevan untuk pengguna berdasarkan riwayat film yang mereka tonton?**
    
   Banyak pengguna tidak tahu harus menonton apa selanjutnya, meskipun telah menonton banyak film sebelumnya. Dengan memahami preferensi pengguna melalui riwayat tontonan mereka, sistem rekomendasi diharapkan dapat memberikan saran film yang sesuai dengan minat dan kebiasaan pengguna.

2. **Bagaimana menjaga keragaman dan jangkauan dari hasil rekomendasi agar tidak hanya terbatas pada film populer?**  

   Sistem rekomendasi konvensional sering kali bias terhadap film populer dan trending, yang menyebabkan hasil rekomendasi menjadi homogen. Hal ini dapat mengurangi kepuasan pengguna jangka panjang karena kurangnya variasi konten. Penting untuk mengembangkan sistem yang juga mengeksplorasi item yang kurang dikenal namun tetap relevan.

3. **Bagaimana memberikan rekomendasi film yang tidak hanya relevan, tetapi juga memiliki unsur kebaruan (novelty)?**

   Tanpa unsur kebaruan, pengguna mungkin hanya akan menerima saran film yang sudah umum atau telah diketahui. Hal ini bisa mengurangi eksplorasi terhadap film-film baru yang sebenarnya bisa menarik. Sistem rekomendasi perlu memasukkan elemen novelty agar pengguna dapat menemukan film unik dan tidak mainstream yang tetap sesuai dengan minat mereka.

4. **Bagaimana merekomendasikan film yang rendah interaksi oleh pengguna namun tetap berdasarkan prefrensi pengguna ?**

   Banyak film dalam katalog memiliki sedikit data interaksi, seperti rating, ulasan, atau tontonan, sehingga sulit untuk direkomendasikan menggunakan metode tradisional yang bergantung pada popularitas atau data kolaboratif. Tantangannya adalah mengembangkan sistem rekomendasi yang mampu mengangkat film-film tersebut agar tidak terabaikan, sekaligus tetap relevan dengan preferensi pengguna. Dengan demikian, sistem dapat memperluas jangkauan rekomendasi, meningkatkan eksposur film-film kurang dikenal, dan memberikan pengalaman yang lebih variatif bagi pengguna.

### Goals
Untuk menjawab permasalahan yang ada, proyek ini bertujuan untuk:
- Memberikan rekomendasi film yang sesuai dengan kebiasaan menonton pengguna berdasarkan data historis mereka.
- Memperluas cakupan rekomendasi agar tidak terbatas pada film-film yang sudah populer, melainkan juga mencakup judul-judul yang kurang dikenal.
- Menyisipkan elemen kebaruan (novelty) dalam hasil rekomendasi dengan menyarankan film yang belum pernah ditonton sebelumnya oleh pengguna.
- Merancang sistem rekomendasi yang mampu mengenali dan merekomendasikan film dengan tingkat interaksi yang rendah namun tetap relevan, guna meningkatkan keberagaman tontonan dan visibilitas konten tersembunyi tanpa mengurangi kualitas maupun kepuasan pengguna.



#### **“Solution Statement”**
Untuk mewujudkan tujuan proyek, pendekatan yang digunakan adalah sebagai berikut:

1. Menggunakan TF-IDF dari genre film dan cosine similarity untuk merekomendasikan film yang mirip dengan yang sudah ditonton pengguna.
2. Membangun model **Content Based Filtering** yang fokus pada kontent tertentu.
3. Membangun mode **Collaborative Filtering (CF)** menggunakan embedding neural network (RecommenderNet) untuk mempelajari pola interaksi pengguna terhadap film berdasarkan histori rating.
4. Membangun model **Hybrid** untk optimalisasi model gabungan.
5. Menggunakan metrik evaluasi:  
      - **Mean Similarity (ILS):** Menilai homogenitas antar film dalam daftar rekomendasi.
      - **Diversity:** Mengukur variasi genre dan konten antar film yang direkomendasikan.
      - **Coverage:** Menghitung proporsi film dari katalog yang berhasil direkomendasikan ke minimal satu pengguna.
6. Menambahkan **penalti popularitas** pada film populer untuk meningkatkan eksplorasi terhadap film kurang dikenal yang tetap relevan.
7. Melakukan **reranking** dengan penalti genre serupa untuk meningkatkan diversity dalam daftar rekomendasi, sehingga pengguna mendapatkan kombinasi film yang tidak hanya relevan, tapi juga segar dan bervariasi, memperkaya pengalaman eksplorasi konten.


## Data Understanding

Dataset untuk project ini digunakan dari sumber berikut: https://www.kaggle.com/datasets/parasharmanas/movie-recommendation-system. Dataset merupakan adaptasi dari MovieLens 25M yang terdiri dari, 
- movies.csv: 62.423 film.
- ratings.csv: 25.000.095 rating dari 162.541 user.

### movies.csv

Berisi informasi film yang tersedia dalam sistem, dengan fitur sebagai berikut:

| Fitur    | Deskripsi                                                                 |
|----------|---------------------------------------------------------------------------|
| movieId  | ID unik setiap film                                                       |
| title    | Judul lengkap film beserta tahun rilis                                    |
| genres   | Daftar genre film, dipisahkan dengan tanda pipe (`|`), contoh: `Action|Comedy` |

### ratings.csv

Berisi data rating yang diberikan oleh pengguna untuk setiap film dengan fitur sebagai berikut:

| Fitur     | Deskripsi                                                                 |
|-----------|---------------------------------------------------------------------------|
| userId    | ID unik setiap pengguna                                                   |
| movieId   | ID film yang diberi rating (relasi dengan `movies.csv`)                  |
| rating    | Nilai rating film yang diberikan oleh pengguna (format desimal)          |
| timestamp | Waktu ketika rating diberikan, dalam format UNIX epoch                   |

### EDA dan Visualisasi Data
EDA dilakukan menggunakan visualisasi distribusi, korelasi antar fitur, dan deteksi outlier. Berdasarkan tahapan EDA didapatkan informasi:
- Tidak ditemukan baris duplikat di movies_df maupun ratings_df. Ini memastikan tidak ada pengaruh bias dari data ganda saat training model.
- Semua kolom tidak memiliki nilai yang hilang baik data movies maupun ratings, sehingga tidak diperlukan proses imputasi atau pembersihan lanjutan.
- Nilai rating 0.5 dan 1.0 terdeteksi sebagai outlier secara statistik. Namun, karena rating ini valid secara domain (termasuk dalam skala rating resmi), maka data ini tetap dipertahankan. Rating rendah tetap penting dalam pendekatan collaborative filtering karena merepresentasikan preferensi pengguna.

### Distribusi Data

**Univariate Analysis**

- Distribusi Rating Film
  - Analisis terhadap kolom `rating` menunjukkan bahwa skor bervariasi antara 0.5 hingga 5.0 dengan interval 0.5. Statistik deskriptifnya sebagai berikut:

  Distribusi frekuensi nilai rating:

  | Rating | Jumlah     |
  |--------|------------|
  | 0.5    | 393,068    |
  | 1.0    | 776,815    |
  | 1.5    | 399,490    |
  | 2.0    | 1,640,868  |
  | 2.5    | 1,262,797  |
  | 3.0    | 4,896,928  |
  | 3.5    | 3,177,318  |
  | 4.0    | 6,639,798  |
  | 4.5    | 2,200,539  |
  | 5.0    | 3,612,474  |

![distribusi_rating](https://github.com/user-attachments/assets/8dfa1b82-1055-4b3c-b6c5-2bdf324bef52)

  - Grafik menunjukkan distribusi rating film dari dataset. Beberapa poin penting yang dapat diamati:
    - Rentang rating berkisar antara 0 hingga 5.
    - Distribusi cenderung menumpuk pada rating tinggi seperti 4 dan 5, menunjukkan bahwa banyak film menerima penilaian positif dari pengguna.
    - Terdapat fluktuasi yang tajam pada kurva KDE (Kernel Density Estimation), kemungkinan disebabkan oleh jumlah data yang sangat besar dan outlier.
    - Nilai puncak tertinggi berada pada rating 4, yang mengindikasikan bahwa mayoritas pengguna memberikan rating yang baik.

    **Insight** : Secara umum, film dalam dataset ini dinilai cukup baik oleh pengguna. Namun, diperlukan pemeriksaan lebih lanjut apakah ada bias pada sistem penilaian (misalnya pengguna cenderung hanya memberi rating tinggi).

- Jumlah Rating per Film

  Analisis dilakukan dengan menghitung jumlah rating untuk setiap `movieId`.

  - Terdapat ketimpangan dalam interaksi pengguna, di mana beberapa film memperoleh jumlah rating yang jauh lebih besar dari yang lain.
  - **Top 3 film dengan jumlah rating terbanyak:**

    | Judul Film                     | Jumlah Rating |
    |--------------------------------|---------------|
    | Forrest Gump (1994)            | 81,491        |
    | The Shawshank Redemption (1994)| 81,482        |
    | Pulp Fiction (1994)            | 79,672        |

  - Insight ini penting karena film-film tersebut cenderung lebih dikenal dan memiliki paparan lebih besar terhadap pengguna.

- Statistik Rating Rata-rata per Film

  - Dihitung nilai rata-rata rating dan jumlah rating per film untuk melihat popularitas dan persepsi kualitas film:

  | Judul Film                       | Average Rating | Jumlah Rating |
  |----------------------------------|----------------|----------------|
  | The Shawshank Redemption (1994)  | 4.41           | 81,482         |
  | Schindler's List (1993)          | 4.24           | 60,411         |
  | Fight Club (1999)                | 4.22           | 58,773         |
  | The Matrix (1999)                | 4.15           | 72,674         |
  | Pulp Fiction (1994)              | 4.18           | 79,672         |

  - Rating rata-rata yang tinggi **dan** jumlah rating yang besar menjadi indikasi kuat bahwa film tersebut **populer dan berkualitas**. Ini dapat dimanfaatkan dalam sistem rekomendasi dengan pendekatan **popularity-based hybrid filtering**.

- Distribusi Genre Film

- Genre film diekstrak dari kolom `genres` dan dihitung frekuensinya setelah eksplosi nilai multigenre.
- Genre paling umum:
    - **Drama**
    - **Comedy**
    - **Action**

- Grafik batang menunjukkan sebaran jumlah film berdasarkan genre. Berikut adalah temuan utama:
      - Drama merupakan genre dengan jumlah film terbanyak, diikuti oleh Comedy, Thriller, dan Romance.
      - Genre seperti Musical, Film-Noir, dan IMAX memiliki jumlah film yang jauh lebih sedikit.
      - Terdapat kategori “(no genres listed)” yang menunjukkan adanya film tanpa label genre — ini perlu ditangani dalam tahap preprocessing.

- ✅ Insight Keseluruhan
  - Rating cenderung positif dengan dominasi skor 3.0–4.0.
  - Sebagian kecil film mendominasi dalam jumlah rating.
  - Film populer umumnya juga memiliki rating rata-rata tinggi.
  - Genre film tidak merata; drama menjadi genre paling umum.

  Insight ini sangat berguna untuk membangun sistem rekomendasi yang **terpersonalisasi**, baik berbasis **interaksi pengguna** maupun **konten film**.

![distribusi_genre_film](https://github.com/user-attachments/assets/b48daf79-bafb-483e-a578-00b6f192ef3e)


**Multivariate Analysis**
Beberapa hal yang dilihat pada tahap ini :
- Analisis untuk melihat hubungan antara **popularitas sebuah film** (diukur dari jumlah rating) dengan **persepsi kualitas** (diukur dari rata-rata rating).
- Plot persebaran rata-rata rating terhadap jumlah rating setiap film.

Interpretasi Statistik:
- Rata-rata rating film populer cenderung stabil di sekitar 4.0.
- Film dengan sedikit rating → tidak cukup representatif → tidak dapat dipercaya sepenuhnya kualitasnya.

## Data Preparation & Preprosesing
Pada tahap ini, dilakukan beberapa proses penting untuk menyiapkan data sebelum dilakukan pemodelan, meliputi penggabungan dataset, pengecekan missing value dan duplikasi, sampling data, transformasi fitur, pengambilan fitur, dan genre yang "(no genres listed)" menjadi string kosong, dan encoding dan normalisasi untuk kebutuhan model.

1. Penggabungan Data

    Data ratings_df dan movies_df digabungkan berdasarkan kolom movieId menggunakan metode merge dengan join tipe left. Tujuannya agar setiap rating yang diberikan oleh pengguna dapat dihubungkan dengan informasi film seperti judul dan genre.

 ```python
merged_df = pd.merge(ratings_df, movies_df, on='movieId', how='left')
 ```

2. Pengecekan Missing Value dan Duplikasi

    Dari pengecekan dapat dilihat tidak ada missing value dan duplikasi data terutama pada fitur userId dan movieId yang nantinya diutamakan untuk pembuatan model.

 ```python
missing_percentage = merged_df.isnull().mean() * 100
merged_duplicates = merged_df.duplicated().sum()
duplicate_ratings = merged_df.duplicated(subset=['userId', 'movieId']).sum()
 ```

3. Sampling Data

    Dataset asli memiliki ukuran yang sangat besar, sehingga untuk efisiensi komputasi dan keterbatasan memori (RAM) di lingkungan pengolahan seperti Google Colab ataupun kaggle, dilakukan sampling acak sebanyak 200.000. Sampling ini dilakukan untuk menjaga agar proses pelatihan dan pengujian model tetap berjalan lancar dan efisien tanpa mengorbankan representasi data secara signifikan. Sampel ini cukup besar untuk mewakili variasi data asli.

 ```python
sample_df = merged_df.sample(n=200_000, random_state=42).copy()
 ```

4. Transformasi Fitur Genre

    Kolom genres yang berisi genre film dalam bentuk string dengan pemisah | diubah menjadi list menggunakan metode split, kemudian dipecah menjadi baris terpisah (explode). Langkah ini memudahkan analisis per genre dan pembuatan fitur berbasis genre.

 ```python
sample_df['genres'] = sample_df['genres'].str.split('|')
genre_exploded = sample_df.explode('genres')
 ```

5. Visualisasi Heatmap Distribusi Rating per Genre

    Visualisasi ini baru dapat dibuat dengan data sejumlah 200.000 yang sebelumnya error jika menggunakan keseluruhan data. Untuk melihat distribusi rating per genre, dilakukan kategorisasi rating ke dalam rentang (bins) dan dibuat pivot table untuk jumlah rating per kategori rating dan genre. Visualisasi heatmap membantu melihat pola distribusi.

 ```python
  genre_exploded['rating_category'] = pd.cut(
      genre_exploded['rating'],
      bins=[0, 1, 2, 3, 4, 5],
      labels=['0-1', '1-2', '2-3', '3-4', '4-5'],
      include_lowest=True
  )
 ```

 ```python
  rating_pivot = filtered_genre.pivot_table(
      index='genres',
      columns='rating_category',
      values='rating',
      aggfunc='count',
      fill_value=0,
      observed=True
  )
 ```

6. Persiapan Data untuk Model Content-Based

    Untuk model content-based, hanya diperlukan kolom movieId, title, dan genres. Genre yang berlabel "(no genres listed)" diganti dengan string kosong agar tidak mengganggu proses analisis.

 ```python
movie_content = movies_df[['movieId', 'title', 'genres']].copy()
movie_content['genres'] = movie_content['genres'].replace("(no genres listed)", "")
 ```

7. Encoding untuk Model Collaborative Filtering

    Model collaborative filtering membutuhkan data numerik untuk representasi pengguna dan film, juga userId dan movieId berupa angka besar (contoh: 125678, 90231). Model embedding membutuhkan input berupa integer mulai dari 0 hingga n. Oleh karena itu, userId dan movieId di-encode menjadi angka berurutan mulai dari 0. Hal ini agar model dapat memproses input dalam bentuk discrete integer index, yang merupakan format yang dibutuhkan oleh embedding layer dalam neural networks.

 ```python
user_to_encoded = {x: i for i, x in enumerate(user_ids)}
movie_to_encoded = {x: i for i, x in enumerate(movie_ids)}
 ```

8. Normalisasi Rating
   
    Rating dinormalisasi ke rentang 0 sampai 1 agar proses pelatihan model lebih stabil dan range rating lebih konsisten.

 ```python
 min_rating = sample_df['rating'].min()
 max_rating = sample_df['rating'].max()
 sample_df['norm_rating'] = sample_df['rating'].apply(lambda x: (x - min_rating) / (max_rating - min_rating))
 ```

## Modeling
Tahapan ini menjelaskan tiga pendekatan model rekomendasi yang dibangun untuk menghasilkan output Top-N Recommendation, yaitu:
- Content-Based Filtering menggunakan representasi fitur konten film (genre).
- Collaborative Filtering berbasis pembelajaran mendalam (deep learning).
- Hybrid Model yang menggabungkan keduanya untuk hasil lebih optimal.

| Model                   | Pendekatan                        | Komponen Kunci                                                               |
| ----------------------- | --------------------------------- | ---------------------------------------------------------------------------- |
| Content-Based Filtering | TF-IDF + Cosine Similarity        | TF-IDF `text_features` (genre, cast, director), cosine similarity antar film |
| Collaborative Filtering | Neural Embedding (RecommenderNet) | Embedding 150 dimensi, dot product, user & movie bias, dropout               |
| Hybrid Model            | Kombinasi CBF + CF                | Skor rata-rata dari normalisasi skor CBF dan CF, penalti popularitas         |

Tujuan penggunaan ketiga pendekatan model ini adalah untuk melakukan evaluasi dan perbandingan efektivitas dalam merekomendasikan film berdasarkan dua pendekatan utama, yaitu Content-Based Filtering (CBF) yang memanfaatkan informasi konten seperti genre film, dan Collaborative Filtering (CF) yang mempelajari pola interaksi pengguna dengan item lainnya, serta model hybrid yang menggabungkan keduanya. Hal ini memungkinkan pemahaman yang lebih baik terkait kekuatan dan kelemahan masing-masing model dalam konteks dataset yang digunakan yang dapat dilihat melalui evaluasi Mean Similiarity, Diversity dan Coverage. Dari ketiga model ini juga diharapkan dapat menghasilkan keputusan model terbaik yang dapat digunakan kedepannya.


**Algoritma yang digunakan**

1. **Content-Based Filtering (CBF)**

    Content-Based Filtering (CBF) adalah salah satu pendekatan sistem rekomendasi yang menyarankan item kepada pengguna berdasarkan kemiripan konten item (misalnya rating film oleh pengguna) untuk menemukan pola kesamaan preferensi antar pengguna. Pendekatan dilakukan dengan melihat kemiripan konten (genre + judul) dengan film yang sudah ditonton dan disukai pengguna, yang dibuat dengan teknik **TF-IDF (Term Frequency-Inverse Document Frequency)**, dan kemudian mengukur kemiripannya dengan **cosine similarity**.

    **Arsitektur Model**
    - **TF-IDF Vectorizer** : Untuk mengubah fitur teks (genre + judul film) menjadi representasi numerik.
    - **Cosine Similarity** : Untuk mengukur kemiripan antar film berdasarkan vektor TF-IDF.

    **Proses Detail**

   1. Membangun User-Item Matrix (TfidfVectorizer)
    
      Fitur teks dari film kemudian dikonversi menjadi vektor numerik menggunakan TF-IDF (Term Frequency-Inverse Document Frequency). Teknik ini menekankan kata-kata yang penting dalam konteks dokumen (film) tertentu namun jarang muncul secara global.

      ```python
      fidf = TfidfVectorizer(token_pattern=r'[^| ]+', stop_words='english')
      tfidf_matrix = tfidf.fit_transform(filtered_movies['text_features'])
      print("Ukuran TF-IDF Matrix:", tfidf_matrix.shape)
      ```
      TF-IDF matrix yang terbentuk memiliki dimensi **[jumlah_film x jumlah_kata_unik]**, di mana setiap nilai menunjukkan pentingnya suatu kata dalam mendeskripsikan film tertentu.

   2. Menghitung Kemiripan Antar Pengguna
      
      Cosine similarity menghitung sudut antar dua vektor (semakin kecil sudut, semakin mirip). Jika dua film memiliki genre yang sangat mirip, maka nilai cosine similarity-nya mendekati 1. Matriks cosine_sim berbentuk **[jumlah_film x jumlah_film]** → setiap nilai menunjukkan tingkat kemiripan antar dua film. Untuk setiap pengguna, sistem mencari film yang paling mirip dengan film-film yang telah mereka tonton.

      ```python
      cosine_sim = cosine_similarity(tfidf_matrix)
      cosine_sim_df = pd.DataFrame(
      cosine_sim,
      index=filtered_movies['title'],
      columns=filtered_movies['title']
      )
      ```
   3.  Output (Rekomendasi CBF)

      ```python

      **Alur Hasil Rekomendasi**
      +----------------------+
      | Mulai                |
      +----------+-----------+
                |
                v
      +------------------------------+
      | Pilih film utama secara acak |
      +------------------------------+
                |
                v
      +---------------------------------------------+
      | Ambil genre dan indeks film utama           |
      +---------------------------------------------+
                |
                v
      +---------------------------------------------+
      | Hitung popularitas tiap film (popularity)   |
      +---------------------------------------------+
                |
                v
      +------------------------------------------------------+
      | Normalisasi popularitas (pop_norm)                   |
      +------------------------------------------------------+
                |
                v
      +---------------------------------------------------------+
      | Hitung skor similarity film dengan film utama          |
      | Dikurangi penalti popularitas:                           |
      | score = similarity - alpha * pop_norm                    |
      +---------------------------------------------------------+
                |
                v
      +----------------------------------------------+
      | Pilih kandidat film (kecuali film utama)     |
      +----------------------------------------------+
                |
                v
      +-------------------------------------------------------+
      | Lakukan reranking dengan penalti genre untuk diversity|
      | - Hitung penalti genre berdasarkan overlap genre       |
      | - score akhir = score - lambda * genre_penalty         |
      | - Pilih film dengan score tertinggi iterasi per iterasi|
      +-------------------------------------------------------+
                |
                v
      +-------------------------------+
      | Pilih Top-k film rekomendasi  |
      +-------------------------------+
                |
                v
      +-------------------------------+
      | Tampilkan rekomendasi film     |
      | Beserta similarity & diversity|
      +-------------------------------+
                |
                v
      +-------------------------------+
      | Tampilkan statistik rating     |
      +-------------------------------+
                |
                v
      +----------------------+
      | Selesai              |
      +----------------------+

      ```
      - Penalti Popularitas untuk Novelty

        Penalti popularitas ini bertujuan untuk mengurangi dominasi film yang sangat populer agar rekomendasi tidak didominasi film mainstream saja. Skor prediksi similarity antar film dikurangi dengan nilai popularitas yang sudah dinormalisasi, dikalikan faktor alpha (misalnya 0.6). Sehingga film yang jarang ditonton atau kurang populer tetap punya peluang muncul dalam daftar rekomendasi. Ini meningkatkan nilai novelty karena rekomendasi menjadi lebih berisi film yang mungkin belum banyak diketahui pengguna namun tetap relevan.

      - Reranking dengan Penalti Genre untuk Diversity

        Setelah mendapatkan kandidat rekomendasi berdasarkan similarity dan penalti popularitas, dilakukan proses reranking untuk menjaga keberagaman genre. Penalti genre dihitung dengan menghitung tumpang tindih genre film kandidat dengan film-film yang sudah terpilih di rekomendasi. Faktor penalti lam (lambda, misalnya 0.5) digunakan sebagai bobot untuk mengurangi skor kandidat yang genre-nya terlalu mirip dengan film yang sudah ada, sehingga mendorong masuknya film dengan genre berbeda. Ini menghasilkan daftar rekomendasi yang lebih beragam secara genre, meningkatkan diversity.

      - Hasil TOP N Rekomendasi CBF
        
        | No | Judul Film                              | Genre                         | Similarity ke Film Utama | Diversity Score (1 - Similarity) |
        | -- | --------------------------------------- | ----------------------------- | ------------------------ | -------------------------------- |
        | 1  | Hot Tub Time Machine 2 (2015)           | Comedy, Sci-Fi                | 0.0900                   | 0.9100                           |
        | 2  | True Crime (1999)                       | Crime, Thriller               | 0.0819                   | 0.9181                           |
        | 3  | Dead Man (1995)                         | Drama, Mystery, Western       | 0.1212                   | 0.8788                           |
        | 4  | Mission: Impossible III (2006)          | Action, Adventure, Thriller   | 0.0737                   | 0.9263                           |
        | 5  | Carrie (2002)                           | Drama, Horror, Thriller       | 0.0644                   | 0.9356                           |
        | 6  | Mission: Impossible - Fallout (2018)    | Action, Adventure, Thriller   | 0.0737                   | 0.9263                           |
        | 7  | Gladiator (1992)                        | Action, Drama                 | 0.1137                   | 0.8863                           |
        | 8  | Mission: Impossible - Rogue Nation (2015) | Action, Adventure, Thriller | 0.0737                   | 0.9263                           |
        | 9  | Sabrina (1954)                          | Comedy, Romance               | 0.1680                   | 0.8320                           |
        | 10 | Beauty Shop (2005)                      | Comedy                        | 0.0832                   | 0.9168                           |


   Film utama yang dipilih adalah Twin Peaks (1989) dengan genre Drama dan Mystery. Rekomendasi film untuk user ini berhasil menyajikan film dari genre yang sangat bervariasi mulai dari Comedy, Thriller, Horror, Action, Documentary, hingga Musical. Setiap film yang direkomendasikan memiliki skor similarity yang cukup tinggi untuk memastikan relevansi dengan film utama, tetapi juga diimbangi dengan penalti genre agar film yang muncul tidak terlalu homogen.

      - Statistik Rating Film Rekomendasi

        Selain similarity dan diversity, daftar rekomendasi ini juga memperhatikan kualitas rating rata-rata dan jumlah rating untuk menjaga rekomendasi yang tidak hanya unik tapi juga berkualitas.

        | movieId | avg\_rating | num\_ratings | title                   |
        | ------- | ----------- | ------------ | ----------------------- |
        | 135296  | 4.5         | 1            | The Twin (1984)         |
        | 963     | 4.0         | 1            | Inspector General, The (1949)  |
        | 95856   | 4.0         | 2            | Knick Knack (1989)      |
        | 5606    | 3.5         | 1            | Society (1989)          |
        | 2725    | 2.9         | 5            | Twin Falls Idaho (1999) |
        | 4636    | 2.375       | 8            | Punisher, The (1989)    |
        | 3338    | 2.0         | 1            | For All Mankind (1989)  |
        | 4279    | 2.0         | 2            | True Believer (1989)    |
        | 130808  | 2.0         | 1            | River of Death (1989)   |
        | 34189   | 1.5         | 1            | Twin Sitters (1994)     |


    Hasil yang direkomendasikan :
    - Film rekomendasi dipilih tidak hanya berdasarkan similarity atau rating tinggi, tapi juga mempertimbangkan novelty (film kurang populer tapi relevan) dan diversity (genre bervariasi).
    - Penalti popularitas membuat rekomendasi tidak didominasi film-film mainstream.
    - Penalti genre membuat rekomendasi beragam secara tema dan genre, sehingga user mendapat pilihan film yang kaya dan berwarna, tidak monoton.
    - Output menampilkan keseimbangan antara kedekatan film utama dan keberagaman rekomendasi.


    Kelebihan CBF
    - Personal dan spesifik: Rekomendasi sepenuhnya disesuaikan dengan preferensi unik pengguna.
    - Tidak butuh data pengguna lain: Model tetap bekerja meskipun pengguna adalah satu-satunya di sistem.
    - Transparan: Dapat dijelaskan kenapa sebuah item direkomendasikan (berdasarkan kesamaan genre atau judul).

    Kekurangan CBF
    - Over-specialization: Cenderung merekomendasikan film yang terlalu mirip, sehingga sulit mengeksplorasi genre baru.
    - Cold-start untuk item baru: Jika item tidak memiliki deskripsi konten yang cukup, tidak dapat direkomendasikan.
    - Terbatas pada informasi yang tersedia: Jika data film kurang lengkap atau tidak representatif, hasil rekomendasi bisa kurang optimal.

2. **Collaborative Filtering (CF)**
   
    Model Collaborative Filtering yang dibangun pada proyek ini menggunakan pendekatan berbasis pembelajaran mendalam (Deep Learning), tepatnya melalui teknik Matrix Factorization dengan Embedding Layer pada framework TensorFlow. Tujuan utamanya adalah mempelajari hubungan implisit antara interaksi pengguna dengan item (film), untuk memprediksi rating atau preferensi pengguna terhadap film yang belum ditonton.

    **Arsitektur Model**
    
   Model ini menggunakan arsitektu **RecommenderNet** dengan pendekatan neural collaborative filtering, dengan komponen utama sebagai berikut:
      - **embedding** : Mengubah ID pengguna dan ID film menjadi representasi vektor berdimensi 150 (dalam space laten). Vektor ini mencerminkan karakteristik laten pengguna dan film, yang dipelajari dari data rating.
      - **user bias dan movie bias** : Untuk menangkap kecenderungan pengguna (suka memberi rating tinggi/rendah) dan film (umumnya disukai atau tidak). Komponen bias ini membantu memperhalus prediksi.
      - **dot product** : Mengukur kecocokan antara pengguna dan film dengan menjumlahkan hasil perkalian elemen-elemen embedding (user_vector * movie_vector). Semakin tinggi dot product, semakin besar kemungkinan film itu disukai pengguna.
      - **Dropout Layer** : Digunakan untuk mencegah overfitting, terutama pada embedding.
      - **Activation Function: sigmoid** : Karena rating dinormalisasi ke [0,1], maka sigmoid sangat cocok digunakan agar output tetap dalam rentang tersebut.

    **Proses Detail**

    1. Split Data
       
      80% data latih (training set) digunakan untuk: Melatih model dan menyesuaikan bobot (belajar dari data).

      20% data validasi (validation set) digunakan untuk:
      - Mengevaluasi performa model selama pelatihan tanpa ikut melatih.
      - Memantau overfitting (ketika model terlalu menghafal data latih dan tidak generalisasi dengan baik).

      ```python
      x = sample_df[['user', 'movie']].values
      y = sample_df['norm_rating'].values
      x_train, x_val, y_train, y_val = train_test_split(x, y, test_size=0.2, random_state=42)
      ```

      Pembagian 80:20 dengan mempertimbangkan jumlah sampel data yang digunakan yatu 200.000 data yang mana termasuk dalam skala data Sedang (100.000 - 1 juta rating) sehingga cukup baik dengan menggunakan perbandingan ini.

    2. User Embedding & Bias
       
      Setiap pengguna dan film direpresentasikan sebagai vektor embedding berdimensi 150. Embedding ini menangkap karakteristik tersembunyi berdasarkan pola interaksi historis dan memetakan setiap film ke dalam vektor representasi.

      ```python
      self.user_embedding = layers.Embedding(
            input_dim=num_users,
            output_dim=embedding_size,
            embeddings_initializer='he_normal',
            embeddings_regularizer=tf.keras.regularizers.l2(1e-6)
        )

       self.movie_embedding = layers.Embedding(
            input_dim=num_movies,
            output_dim=embedding_size,
            embeddings_initializer='he_normal',
            embeddings_regularizer=tf.keras.regularizers.l2(1e-6)
        )
      ```

    3. User Bias dan Movie Bias
       
      Setiap pengguna dan film memiliki bias yang merepresentasikan kecenderungan rating umum yang mereka berikan (user bias) dan rating yang mereka terima (movie bias). Bias ini ditambahkan agar prediksi rating lebih akurat. Bobot bias per film untuk mengatasi perbedaan popularitas atau kualitas film.

      ```python
      self.user_bias = layers.Embedding(input_dim=num_users, output_dim=1)
      self.movie_bias = layers.Embedding(input_dim=num_movies, output_dim=1)
      ```
      
    4. Prediksi Rating dengan Dot Product
       
      Prediksi rating dihitung dengan melakukan dot product antara vektor embedding pengguna dan film, lalu ditambahkan bias masing-masing.

      ```python
      def call(self, inputs):
        user_vector = self.dropout(self.user_embedding(inputs[:, 0]))
        movie_vector = self.dropout(self.movie_embedding(inputs[:, 1]))

        user_bias = self.user_bias(inputs[:, 0])
        movie_bias = self.movie_bias(inputs[:, 1])

        dot_product = tf.reduce_sum(user_vector * movie_vector, axis=1, keepdims=True)
        x = dot_product + user_bias + movie_bias

        return tf.nn.sigmoid(x)
      ```
      Formula :

      ![dot_product](https://github.com/user-attachments/assets/c6bae9fa-db57-4684-8a6d-851c158c7c75)

      - Hasil ini adalah prediksi skor preferensi pengguna \( u \) terhadap film \( m \).
      
    5. Compile Model
       
      Model ini dilatih menggunakan fungsi loss binary_crossentropy, dengan optimizer Adam dan learning rate 0.0005 untuk meminimalkan kesalahan prediksi dalam bentuk probabilitas rating yang telah dinormalisasi ke rentang [0,1].

      - Loss function adalah metrik yang digunakan untuk mengukur seberapa jauh prediksi model berbeda dari nilai target (rating sebenarnya yang sudah dinormalisasi). Loss function ini memberikan “sinyal” kepada model agar dapat memperbaiki prediksinya selama proses pelatihan.
      - Fungsi binary_crossentropy dipilih karena output model berupa probabilitas (nilai antara 0 dan 1), sehingga cocok untuk mengukur perbedaan distribusi probabilitas antara prediksi dan target.
      - Metrik Root Mean Squared Error (RMSE) untuk mengevaluasi performa model secara numerik. RMSE adalah akar dari rata-rata kuadrat selisih antara nilai prediksi dengan nilai sebenarnya.
        Formula :

      ![RSME](https://github.com/user-attachments/assets/b98db509-a651-45bb-9e76-0e75cbd46451)

      RMSE digunakan karena:
           - Kuadrat selisih memastikan bahwa error negatif dan positif sama-sama dihitung sebagai nilai positif.
           - Memberikan penalti lebih besar pada error yang besar, sehingga model terdorong untuk meminimalkan kesalahan prediksi signifikan.
           - Cocok untuk mengukur kualitas prediksi dalam masalah regresi seperti prediksi rating film.
           - Optimizer Adam bertugas memperbarui bobot model (embedding dan bias) secara adaptif agar nilai loss berangsur-angsur mengecil dan model dapat belajar dengan efisien.

      ```python
      model = RecommenderNet(num_users, num_movies, embedding_size=150, dropout_rate=0.2)
      model.compile(
      loss='binary_crossentropy',
      optimizer=tf.keras.optimizers.Adam(learning_rate=0.0005),
      metrics=[tf.keras.metrics.RootMeanSquaredError()]
      )
      ```

    6. Training Model dengan Early Stopping
       
    Model dilatih hingga 50 epoch, namun akan berhenti lebih awal dengan fungsi dari early_stop sehingga pelatihan menjadi lebih efisien karena tidak membuang waktu melakukan training saat model sudah tidak menunjukkan peningkatan performa, sekaligus mengurangi risiko overfitting pada data training.

      ```python
      early_stop = EarlyStopping(monitor='val_loss', patience=5, restore_best_weights=True)

      history = model.fit(
      x=x_train,
      y=y_train,
      batch_size=128,
      epochs=50,
      validation_data=(x_val, y_val),
      callbacks=[early_stop]
      )
      ```

    7. Evaluasi Metrik

    - Grafik menunjukkan penurunan RMSE yang stabil pada data training dan validation.
    - Train RMSE menurun signifikan dari awal hingga epoch 8, menunjukkan model belajar dengan baik dari data.
    - Val RMSE juga menurun, meskipun lebih lambat, tanpa indikasi overfitting (tidak ada kenaikan drastis pada Val RMSE).
    - Gap Train-Val RMSE relatif kecil, menunjukkan model cukup stabil dan generalizable untuk data baru.
    - Secara keseluruhan, model Collaborative Filtering ini menunjukkan performa yang baik dan tidak overfit terhadap data training.
    
    ![model_metrik](https://github.com/user-attachments/assets/19cd3163-d90f-450e-b38d-f9ab7918ffa1)

    ![RSME_metrik](https://github.com/user-attachments/assets/e131a1cc-a03f-41ec-aec4-a7dcbde11af6)

    8. Output (Rekomendasi CF)
  
    ```python
    **Alur Hasil Rekomendasi**
    +--------------------------+
    | Mulai: Pilih user acak   |
    +--------------------------+
              |
              v
    +--------------------------+
    | Ambil daftar film yang   |
    | sudah ditonton user      |
    +--------------------------+
              |
              v
    +--------------------------+
    | Identifikasi film yang   |
    | belum ditonton           |
    +--------------------------+
              |
              v
    +--------------------------+
    | Prediksi rating film     |
    | menggunakan model CF     |
    +--------------------------+
              |
              v
    +--------------------------+
    | Hitung penalti popularitas|
    | dan hitung skor novelty  |
    +--------------------------+
              |
              v
    +--------------------------+
    | Reranking film untuk     |
    | meningkatkan diversity   |
    | (penalti genre tumpang-  |
    | tindih)                 |
    +--------------------------+
              |
              v
    +--------------------------+
    | Hasil rekomendasi Top-N  |
    +--------------------------+
              |
              v
    +--------------------------+
    | Tampilkan hasil ke user  |
    +--------------------------+
    ```
    
    - Penalti Popularitas untuk Novelty
    
      Mengukur seberapa banyak rekomendasi terdiri dari item yang baru, tidak umum, atau jarang diketahui oleh pengguna.
      Dengan penalti popularitas, rekomendasi berfokus tidak hanya pada film populer, tetapi juga film dengan interaksi rendah yang tetap relevan. Dengan implementasi skor prediksi rating dikurangi dengan nilai normalisasi popularitas film (pop_norm) dikalikan faktor penalti (misal 0.3) film dengan popularitas tinggi mendapatkan penalti sehingga film kurang populer yang relevan berpeluang muncul lebih banyak, meningkatkan novelty (kebaruan).

    - Reranking dengan Penalti Genre untuk Diversity

      Mengukur variasi karakteristik (genre, tema, style) film dalam daftar rekomendasi. Reranking dengan penalti genre mencegah dominasi genre tertentu, sehingga daftar rekomendasi menyajikan pilihan yang lebih kaya dan beragam. Dengan parameter Lambda (λ)  sebagai faktor pengali penalti genre (misal 0.5) untuk menyeimbangkan antara skor prediksi dan keberagaman genre dan Top-N sebagai variabel Jumlah film yang direkomendasikan (misal 10), mendorong masuknya film dari genre yang berbeda sehingga meningkatkan diversity (keberagaman) dalam rekomendasi.

    - Hasil TOP N Rekomendasi CF

      | No | Judul Film                  | Genre                                         | Mean Similarity | Diversity Score |
      | -- | --------------------------- | --------------------------------------------- | --------------- | --------------- |
      | 1  | North by Northwest (1959)   | Action, Adventure, Mystery, Romance, Thriller | 0.1875          | 0.8125          |
      | 2  | Sting, The (1973)           | Comedy, Crime                                 | 0.1211          | 0.8789          |
      | 3  | Hotel Rwanda (2004)         | Drama, War                                    | 0.1458          | 0.8542          |
      | 4  | Hoop Dreams (1994)          | Documentary                                   | 0.1198          | 0.8802          |
      | 5  | Brazil (1985)               | Fantasy, Sci-Fi                               | 0.1743          | 0.8257          |
      | 6  | The Hateful Eight (2015)    | Western                                       | 0.0781          | 0.9219          |
      | 7  | Get Out (2017)              | Horror                                        | 0.0777          | 0.9223          |
      | 8  | Prince of Egypt, The (1998) | Animation, Musical                            | 0.1796          | 0.8204          |
      | 9  | Hereditary (2018)           | (no genres listed)                            | 0.0613          | 0.9387          |
      | 10 | Kiss Me Deadly (1955)       | Film-Noir                                     | -0.1037         | 1.1037          |

    Model ini menghasilkan Top-10 rekomendasi film untuk user 57215, yang sebelumnya telah menonton Notting Hill (1999) yang memiliki genre Comedy dan Romance. Rekomendasi mencakup film dari berbagai genre seperti Horror, Western, Fantasy, dan Documentary untuk memastikan variasi dan relevansi yang tinggi. Setiap film disertai dengan skor similarity antar film, serta diversity score untuk menunjukkan keberagaman konten rekomendasi.

    Film yang masuk ke daftar rekomendasi:
    - Memiliki prediksi rating tinggi (dari model CF).
    - Tidak terlalu populer (penalti novelty).
    - Genre-nya bervariasi dan tidak saling tumpang tindih.

    Kelebihan
    - Menangkap hubungan kompleks antara pengguna dan film yang tidak tergambar hanya dari data yang besar.
    - Dapat mempelajari pola implisit dari interaksi rating dalam skala besar.
    - Dukungan fleksibilitas tinggi dengan optimasi dan teknik regularisasi modern.

    Kekurangan
    - Cold-start problem: Tidak dapat merekomendasikan dengan baik untuk pengguna/film baru yang belum punya interaksi. Karena prediksinya berbasis interaksi pengguna lain.
    - Butuh banyak data: Performanya tinggi jika data interaksi pengguna cukup besar.
    - Kompleksitas komputasi: Lebih berat dibanding model tradisional seperti CBF sederhana.

4. **Hybrid Model**

  Hybrid recommendation menggabungkan dua pendekatan utama dalam sistem rekomendasi yaitu Content-Based Filtering (CBF) dan Collaborative Filtering (CF). Tujuannya adalah memanfaatkan kelebihan kedua metode agar menghasilkan rekomendasi yang lebih akurat, relevan, dan beragam.

  **Arsitektur Model Hybrid**
  
  1. 40%  Content-Based Filtering (CBF) Module:
      - Menggunakan matriks kemiripan (cosine similarity) antar film berdasarkan fitur konten (judul, genre).
      - Menghitung skor kemiripan film terhadap film yang sudah ditonton pengguna.
      - Mengurangi skor film dengan penalti popularitas untuk meningkatkan novelty.

  2. 60% Collaborative Filtering (CF) Module:
      - Menggunakan model deep learning yang memprediksi rating film berdasarkan pola preferensi pengguna (user embedding + item embedding).
      - Penalti popularitas juga diterapkan agar tidak terlalu merekomendasikan film populer secara berlebihan.

  3. Hybrid Fusion :
      - Menggabungkan skor dari CBF dan CF dengan bobot tertentu (weight_cbf=40).
      - Normalisasi skor kedua metode agar setara.
      - Menghasilkan ranking film akhir sebagai hasil rekomendasi.

  **Note :
  
  Pemilihan proporsi 40% Content-Based Filtering (CBF) dan 60% Collaborative Filtering (CF) pada model Hybrid dilakukan dengan pertimbangan untuk meningkatkan relevansi dan personalisasi rekomendasi, sambil tetap menjaga diversity dan coverage yang baik. Proporsi ini memberikan penekanan lebih pada CF, agar hasil rekomendasi tetap personal dan relevan secara statistik. Namun tidak menghilangkan peran CBF, yang mampu menjembatani cold-start item dan meningkatkan eksplorasi film baru.

  **Alur Modeling**
  
  - Content-Based Filtering (CBF)
    
    Menghitung skor kemiripan film berdasarkan fitur konten (genre, judul, dsb) menggunakan cosine similarity. Skor ini juga dikurangi dengan penalti popularitas untuk menghindari dominasi film populer.

  - Collaborative Filtering (CF)
    
    Menggunakan model prediksi berbasis interaksi pengguna-film (misal model neural network) untuk memperkirakan rating film yang belum ditonton pengguna. Skor prediksi ini juga dikenai penalti popularitas.

  - Penggabungan Skor
    
    Skor CBF dan CF yang sudah dinormalisasi kemudian digabungkan dengan bobot tertentu (misal weight_cbf=0.5) untuk menghasilkan skor akhir rekomendasi film.

  **Output TOP N Rekomendasi  Hybrid**

| No | Title                                     | #Times Recommended | Mean ILS | Diversity Score | Genre                           | Feature Dominance |
| -- | ----------------------------------------- | ------------------ | -------- | --------------- | ------------------------------- | ----------------- |
| 1  | Hot Tub Time Machine 2 (2015)             | 2                  | 0.0900   | 0.9100          | Comedy \| Sci-Fi                | CF                |
| 2  | True Crime (1999)                         | 2                  | 0.0819   | 0.9181          | Crime \| Thriller               | CF                |
| 3  | Dead Man (1995)                           | 2                  | 0.1212   | 0.8788          | Drama \| Mystery \| Western     | CF                |
| 4  | Mission: Impossible III (2006)            | 2                  | 0.0737   | 0.9263          | Action \| Adventure \| Thriller | CF                |
| 5  | Carrie (2002)                             | 2                  | 0.0644   | 0.9356          | Drama \| Horror \| Thriller     | CF                |
| 6  | Mission: Impossible - Fallout (2018)      | 2                  | 0.0737   | 0.9263          | Action \| Adventure \| Thriller | CF                |
| 7  | Gladiator (1992)                          | 2                  | 0.1137   | 0.8863          | Action \| Drama                 | CF                |
| 8  | Mission: Impossible - Rogue Nation (2015) | 2                  | 0.0737   | 0.9263          | Action \| Adventure \| Thriller | CF                |
| 9  | Sabrina (1954)                            | 2                  | 0.1680   | 0.8320          | Comedy \| Romance               | CF                |
| 10 | Beauty Shop (2005)                        | 1                  | 0.0832   | 0.9168          | Comedy                          | CF                |


  Model ini mengumpulkan dan menganalisis hasil rekomendasi film dari model hybrid (gabungan CBF dan CF). Setiap film yang direkomendasikan dievaluasi dengan metrik:
  - ILS (Intra-List Similarity) untuk mengukur kemiripan antar film dalam daftar rekomendasi (nilai rendah = rekomendasi lebih beragam).
  - Skor CBF dan CF untuk melihat kontribusi masing-masing metode pada rekomendasi film.
  - Diversity Score dihitung dari 1 - Mean ILS, menandakan keragaman rekomendasi.
  - Feature Dominance menunjukkan apakah CBF atau CF yang lebih dominan dalam merekomendasikan film tersebut.

## Evaluation

Pada proyek sistem rekomendasi film ini, saya menggunakan tiga model utama: Content-Based Filtering (CBF), Collaborative Filtering (CF), dan Hybrid Model yang menggabungkan keduanya. Untuk mengevaluasi performa model, saya memilih metrik evaluasi berikut:

- **Mean Intra-List Similarity (ILS)**

  Mean Intra-List Similarity (ILS) digunakan untuk mengukur diversitas rekomendasi yang diberikan kepada pengguna. Metrik ini menghitung rata-rata kemiripan antar item (film) dalam satu daftar rekomendasi. Semakin tinggi ILS, maka film yang direkomendasikan semakin mirip satu sama lain → artinya kurang beragam. Sebaliknya, semakin rendah ILS, berarti sistem memberikan rekomendasi yang lebih bervariasi.
  
  Formula:  
  ![ILS](https://github.com/user-attachments/assets/53d40082-f44a-426d-8747-ca597b3e97ef)

  Keterangan:
    - 𝐿 = daftar rekomendasi (list film yang diberikan)
    - sim(i,j) = cosine similarity antara film ke-i dan ke-j

  Interpretasi:
    - Semakin tinggi ILS → film-film yang direkomendasikan semakin mirip satu sama lain → rekomendasi kurang beragam.
    - Semakin rendah ILS → film yang direkomendasikan lebih beragam, berasal dari genre atau konten yang berbeda-beda.

- **Diversity Score**
  
  Mengukur keragaman daftar rekomendasi, dihitung sebagai 1 - Mean Similarity. Semakin tinggi nilai ini, rekomendasi semakin bervariasi.

  Formula :
  ![diversity](https://github.com/user-attachments/assets/ebd9d6a2-e3e0-425b-a166-faeaf825dd4a)

  Semakin tinggi nilai diversity, semakin beragam item yang direkomendasikan, yang berarti pengguna akan mendapatkan rekomendasi yang lebih bervariasi (baru).

  Interpretasi:
    - Nilai diversity tinggi → rekomendasi sangat beragam
    - Nilai diversity rendah → rekomendasi cenderung mirip satu sama lain
    - 
<br>
  Contoh hubungan ILS dan Diversity

  | **Mean Similarity (ILS)** | **Diversity Score (1 - ILS)** |
  | ------------------------- | ----------------------------- |
  | 1.00                      | 0.00                          |
  | 0.80                      | 0.20                          |
  | 0.50                      | 0.50                          |
  | 0.20                      | 0.80                          |
  | 0.00                      | 1.00                          |

  contoh code :

  ```python
  mean_ils = np.mean(ils_list)
  diversity_score = 1 - mean_ils
  ```

- **Coverage**

  Coverage mengukur sejauh mana sistem mengeksplorasi seluruh ruang item (film). Dengan kata lain, metrik ini menunjukkan proporsi film yang pernah direkomendasikan setidaknya satu kali dibanding total jumlah film yang tersedia.

  Formula:  
  ![coverage](https://github.com/user-attachments/assets/a4b1a9ca-51ce-4c67-9295-9f2f28850e10)

  Keterangan:
    - 𝑅𝑢 = daftar rekomendasi untuk user 
    - 𝐼 = total item/film dalam dataset

  Semakin tinggi coverage, berarti sistem tidak hanya merekomendasikan film-film populer atau yang itu-itu saja, tetapi mengeksplorasi banyak item berbeda.

### Kenapa Menggunakan ILS, Diversity Score dan Coverage ?
- Mean Intra-List Similarity (ILS)

  ILS digunakan untuk mengukur seberapa mirip film-film dalam satu daftar rekomendasi. Nilai ILS yang tinggi menunjukkan bahwa rekomendasi cenderung homogen (kurang bervariasi), sedangkan nilai rendah menunjukkan rekomendasi yang lebih beragam.
  → Tujuan: menghindari rekomendasi yang terlalu mirip agar pengguna tidak bosan.

- Diversity Score

  Diversity dihitung sebagai 1 - ILS, sehingga semakin tinggi nilainya, rekomendasi semakin bervariasi.
  → Tujuan: memastikan pengguna mendapat saran film dari genre atau konten yang berbeda-beda.

- Coverage

  Coverage mengukur seberapa banyak film dalam katalog yang pernah direkomendasikan. Nilai coverage yang tinggi menunjukkan sistem menjelajah lebih luas, tidak hanya merekomendasikan film populer saja.
  → Tujuan: meningkatkan eksposur terhadap film-film yang kurang dikenal.


**Hasil Evaluasi Tiap Model**

| Model  | Mean Similarity (ILS) ↓ | Diversity Score ↑    | Coverage ↑          |
| ------ | ----------------------- | -------------------- | ------------------- |
| CF     | 0.0213 ✅ (terendah)     | 0.9787 ✅ (tertinggi) | 0.5261 ❌ (terendah) |
| CBF    | 0.1261 ❌ (tertinggi)    | 0.8739 ❌             | 0.7428 ✅            |
| Hybrid | 0.0894 ⚖️ (sedang)      | 0.9106 ⚖️            | 0.7472 ✅            |


**Intepretasi**
- CBF memberikan cakupan rekomendasi yang luas, tetapi filmnya cenderung mirip satu sama lain.
- CF menghasilkan rekomendasi paling beragam, tapi hanya untuk sebagian kecil film.
- Hybrid memberi keseimbangan yang baik antara keberagaman dan cakupan, menjadikannya model yang paling optimal secara keseluruhan.
- Hybrid menjadi model terbaik untuk rekomendasi ✅, karena :
  - Model Hybrid menggabungkan kekuatan dari dua pendekatan utama:
    - Collaborative Filtering (CF): Belajar dari pola interaksi pengguna (siapa menyukai apa).
    - Content-Based Filtering (CBF): Memanfaatkan informasi konten (seperti genre, judul, tahun) untuk menilai kesamaan antar film.
  - Dalam Hybrid, coverage adalah 0.7472 (≈75%), menunjukkan model bisa menjangkau dan merekomendasikan film-film yang jarang atau bahkan belum pernah populer sebelumnya (long-tail items).
  - keunggulan Hybrid berdasarkan score tersebut :
    - Relevansi (ILS) tetap cukup tinggi → rekomendasi masih relevan dengan minat pengguna.
    - Diversity sangat baik (0.9106) → film yang disarankan sangat bervariasi, membuat eksplorasi menarik.
    - Coverage paling luas (0.7472) → banyak film jarang ditonton bisa ikut direkomendasikan.
  - 📌 Secara strategis, Hybrid model:
    - Menghindari bias terhadap film populer
    - Menyajikan opsi baru yang belum diketahui pengguna
    - Memaksimalkan eksposur katalog film

### **INSIGHT berdasarkan BUSSINES UNDERSTANDING**


**Menjawab Problem Statement**

1. **Bagaimana merekomendasikan film yang relevan untuk pengguna berdasarkan riwayat film yang mereka tonton?**
   
  Hybrid model menggabungkan pendekatan Content-Based Filtering (CBF) yang memanfaatkan fitur konten film (genre dan judul) dengan Collaborative Filtering (CF) yang mempelajari pola interaksi pengguna. Dengan demikian, model dapat memberikan rekomendasi yang relevan baik dari sisi konten yang serupa (didapat dari model CBF dengan teknik **TF-IDF (Term Frequency-Inverse Document Frequency)** **cosine similarity**) dengan film favorit pengguna maupun dari pola interaksi komunitas pengguna yang mirip (didapat dari model CF yang menggunakan arsitektur RecommenderNet).  

2. **Bagaimana menjaga keragaman dan jangkauan dari hasil rekomendasi agar tidak hanya terbatas pada film populer?**
   
  Model hybrid mampu menjaga keseimbangan diversity tetap tinggi (menghindari rekomendasi monoton) dan coverage lebih luas (menjangkau lebih banyak film, termasuk yang kurang terkenal), kemudian menggunakan penalti popularitas dan reranking genre, sehingga mencegah dominasi film populer. Pendekatan ini memastikan rekomendasi mencakup film-film dari berbagai genre dan tingkat popularitas, menghasilkan daftar rekomendasi yang lebih bervariasi dan menarik. 

3. **Bagaimana memberikan rekomendasi film yang tidak hanya relevan, tetapi juga memiliki unsur kebaruan (novelty)?**
   
  Hybrid Model menyeimbangkan relevansi dan novelty. Relevansi tetap terjaga karena mempertimbangkan film serupa yang sudah ditonton, Novelty tercapai karena model juga menyarankan film yang mungkin belum diketahui pengguna, tetapi populer di kalangan pengguna dengan selera serupa. Reranking berbasis penalti genre dan popularitas juga membantu menonjolkan film baru yang tetap relevan namun belum umum diketahui.

4. **Bagaimana merekomendasikan film yang rendah interaksi oleh pengguna namun tetap berdasarkan prefrensi pengguna ?**
   
  Hybrid Model mengatasi cold-start item dengan baik. Teknik ini menggabungkan kekuatan kedua pendekatan dengan :
  
  Skor Gabungan (Fusion): Setiap film diberi skor gabungan dengan α sebagai parameter pengontrol (misal: 0.5 untuk seimbang).

  ![Fusion](https://github.com/user-attachments/assets/b12c2ed9-74db-4341-9d13-dd4f2eeb79dc)

  Jika CF tidak bisa memberi skor (karena film cold-start), sistem fallback ke skor CBF. Setelah skor gabungan dihitung, sistem melakukan ranking dan memilih top-N rekomendasi. Hasilnya Film dengan interaksi rendah tetap punya peluang muncul di daftar rekomendasi jika mirip secara konten dengan preferensi user (CBF), atau punya embedding yang cukup dari interaksi terbatas (CF), atau kombinasi keduanya. Dengan reranking dan penalty juga film populer diturunkan sedikit peringkatnya, sehingga film cold-start yang relevan punya peluang lebih tinggi muncul.

**Mencapai Goals yang Diharapkan**

Goal 1: Menyajikan rekomendasi film yang relevan berdasarkan riwayat tontonan pengguna.

Model hybrid berhasil memberikan rekomendasi yang relevan dengan preferensi pengguna, memanfaatkan keunggulan Content-Based Filtering untuk memahami kesamaan konten film (genre, judul) dan Collaborative Filtering untuk mempelajari pola interaksi pengguna. Model ini menghasilkan rekomendasi yang personal dan relevan, mendukung pengguna dalam menemukan film sesuai minat mereka.

Goal 2: Memperluas jangkauan rekomendasi agar tidak hanya mencakup film populer, namun juga film yang kurang dikenal.

Hybrid model dengan penalti popularitas berhasil meningkatkan novelty rekomendasi dengan memberikan peluang lebih besar bagi film non populer untuk muncul dalam daftar rekomendasi. Film non-populer tetap mendapat kesempatan muncul di daftar rekomendasi, dapat dilihat dari score coverage meningkat ke 0.7472. Ini membantu platform mengekspos lebih banyak konten dari katalog.

Goal 3: Menghadirkan unsur kebaruan (novelty) dalam rekomendasi untuk menawarkan film baru (belum pernah di tonton pengguna).

Melalui proses reranking dengan penalti genre, sistem rekomendasi mampu mengurangi dominasi genre tertentu dan menyajikan daftar rekomendasi yang lebih bervariasi. Diversity score yang tinggi (0.9106) pada model hybrid menunjukkan bahwa pengguna menerima rekomendasi dari berbagai genre dan tema, sehingga pengalaman eksplorasi film menjadi lebih menarik dan tidak monoton.

Goal 4: Mengembangkan mekanisme rekomendasi yang mampu mengidentifikasi dan menyarankan film-film dengan tingkat interaksi rendah namun relevan bagi pengguna,...

Hybrid model berhasil memadukan keunggulan CBF (berfungsi optimal pada item cold-start) dan CF (memberikan konteks pola pengguna lain) untuk memberikan rekomendasi pada film-film dengan data interaksi rendah (long-tail items) yang relevan dengan minat pengguna. Evaluasi menggunakan metrik Mean Similarity, Diversity, dan Coverage menunjukkan bahwa Hybrid model menunjukkan keseimbangan antara relevansi, keragaman, dan cakupan—kriteria utama dalam recommender system yang kuat dan adaptif terhadap konteks bisnis.

**Dampak Solusi Statement**

Untuk mewujudkan tujuan proyek, pendekatan yang digunakan adalah sebagai berikut:

- Menggunakan TF-IDF dari genre film dan cosine similarity untuk merekomendasikan film yang mirip dengan yang sudah ditonton pengguna.
  1. Personalisasi yang Lebih Akurat: Menghasilkan rekomendasi yang sesuai dengan preferensi konten pengguna, terutama berdasarkan genre.
  2. Mengatasi Cold-Start Item: Film baru (yang belum memiliki banyak interaksi) tetap bisa direkomendasikan selama informasi genre tersedia.

- Membangun model **Content Based Filtering** yang fokus pada kontent tertentu.
  Menghasilkan model yang fokus pada preferensi spesifik konten. Memungkinkan sistem untuk menyarankan film yang secara konten mirip dengan film yang disukai pengguna sebelumnya.

- Membangun mode **Collaborative Filtering (CF)** menggunakan embedding neural network (RecommenderNet) untuk mempelajari pola interaksi pengguna terhadap film berdasarkan histori rating.
  Menghasilkan model yang mampu menangkap preferensi tersembunyi (Latent Preference) berdasarkan interaksi. CF memungkinkan sistem mengenali pola kesukaan pengguna berdasarkan perilaku kolektif pengguna lain.

- Membangun model **Hybrid** untk optimalisasi model gabungan
  Model ini tidak hanya mengulang rekomendasi yang mirip atau populer, tapi juga mendorong penemuan konten baru yang sesuai. Optimalisasi model yang ada.

- Menggunakan metrik evaluasi:  
  - **Mean Similarity (ILS):** Menilai homogenitas antar film dalam daftar rekomendasi.
  - **Diversity:** Mengukur variasi genre dan konten antar film yang direkomendasikan.
  - **Coverage:** Menghitung proporsi film dari katalog yang berhasil direkomendasikan ke minimal satu pengguna.
    
      1. Menunjukkan seberapa seragam daftar rekomendasi; digunakan untuk mengontrol homogenitas.
      2. Menilai keragaman konten; model dengan diversity tinggi mencegah daftar rekomendasi yang monoton.
      3. Mengukur seberapa luas sistem menjelajahi katalog film. Coverage tinggi berarti lebih banyak film berpeluang direkomendasikan, bukan hanya yang populer.

- Menambahkan **penalti popularitas** pada film populer untuk meningkatkan eksplorasi terhadap film kurang dikenal yang tetap relevan.
  Mengurangi Bias ke Film Populer dan meningkatkan ekplorasi. Memperkenalkan film long-tail yang mungkin relevan tapi jarang muncul karena jumlah interaksinya sedikit.

- Melakukan **reranking** dengan penalti genre serupa untuk meningkatkan diversity dalam daftar rekomendasi, sehingga pengguna mendapatkan kombinasi film yang tidak hanya relevan, tapi juga segar dan bervariasi, memperkaya pengalaman eksplorasi konten.
      1. Menghindari dominasi genre tertentu dalam rekomendasi.
      2. Pengguna mendapatkan daftar film dari genre yang beragam namun tetap relevan, mendorong eksplorasi lebih luas.
      3. Memberikan kombinasi rekomendasi yang bersifat personal, segar, dan tidak repetitif.
      4. Membuat pengguna lebih betah menjelajah katalog, karena rekomendasi terasa dinamis dan tidak kaku.


## Kesimpulan
  1. Model rekomendasi hybrid yang dibangun berhasil menggabungkan keunggulan Content-Based Filtering dan Collaborative Filtering untuk menghasilkan rekomendasi yang personal, relevan, dan variatif.
  2. Penerapan TF-IDF dan cosine similarity memungkinkan sistem memahami konten film (genre), sehingga tetap dapat merekomendasikan item baru (cold-start) meskipun belum ada interaksi pengguna.
  3. Embedding neural network (RecommenderNet) pada model CF memungkinkan sistem mengenali pola interaksi pengguna secara lebih dalam, meningkatkan akurasi dan fleksibilitas.
  4. Evaluasi menggunakan metrik Mean Similarity, Diversity, dan Coverage membuktikan bahwa model mampu:
    Menjaga keseimbangan homogenitas dan keberagaman rekomendasi.
    Menjangkau item long-tail dalam katalog film.
  5. Penambahan penalti popularitas dan reranking genre meningkatkan novelty dan diversity, membuat daftar rekomendasi tidak hanya relevan tapi juga menarik dan tidak monoton.
  6. Secara keseluruhan, model ini efektif dalam mendukung pengguna menemukan film baru yang sesuai minat mereka, sekaligus memperkenalkan konten yang kurang populer namun relevan, meningkatkan user engagement dan eksplorasi katalog secara menyeluruh.
