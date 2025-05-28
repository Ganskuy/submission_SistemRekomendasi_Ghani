# Laporan Proyek Machine Learning - Muhammad Haifan Ghani (MC012D5Y1825)

## Topik Proyek : Rekomendasi Film

Sistem rekomendasi memainkan peran krusial dalam meningkatkan profitabilitas platform streaming seperti Netflix, dengan mempersonalisasi pengalaman pengguna dan secara signifikan mengurangi tingkat berhentinya pelanggan (churn). Melalui algoritma yang canggih, sistem ini mampu menyajikan konten yang relevan bagi setiap pengguna, yang pada akhirnya mendorong keterlibatan jangka panjang dan loyalitas pelanggan. The recommendation system is estimated to save Netflix about $1 billion per year by reducing churn and improving customer satisfaction [_(Gomez et al., 2021)_](https://dl.acm.org/doi/abs/10.1145/2843948). Dari banyaknya film yang diproduksi membuat calon penonton bingung dan kesulitan untuk mencari dan menentukan film apa yang akan ditonton selanjutnya sehingga menghabiskan waktu lebih banyak dalam mencari film [_(Fajriansyah et al., 2021)_](https://j-ptiik.ub.ac.id/index.php/j-ptiik/article/view/9163). Kutipan sebelumnya membuktikan seberapa pentungnya sistem rekomendasi ini pada aplikasi Streaming seperti Netflix dalam melancarkan bisnisnya dan memberikan pengalaman yang maksimal bagi pengguna.

Pada proyek ini, saya menggunakan film Avatar (2009) untuk dijadikan studi kasus dalam menilai seberapa baik Sistem Rekomendasi, yang dibuat menggunakan pendekatan Content-based Filtering, Cosine Similarity sebagai metrik similaritas, dan Precission@k serta Mean Precission@k sebagai metrik evaluasi. Harapannya proyek ini dapat membuka pengetahuan orang-orang tentang bagaimana Mesin memberikan sebuah rekomendasi kepada user, dan bagaimana perbandingannya dengan rekomendasi yang diberikan oleh manusia.

## Business Understanding

### Problem Statements

Rumusan masalah pada proyek ini adalah :
- Dilansir dari Website Rotten Tomatoes tentang film dengan penghasilan tertinggi sepanjang masa, Avatar (2009) merupakan film no. 1 dengan penghasilan tertinggi, dengan metode Content-Based Filtering, film apakah yang akan direkomendasikan untuk film yang mirip dengan Avatar?
- Bagaimana perbedaan antara film yang direkomendasikan oleh model dengan film yang direkomendasikan oleh manusia?

### Goals

Tujuan dari menyelesaikan permasalahan diatas adalah :
- Mengetahui rekomendasi lain mengenai film yang mirip dengan Avatar (2009)
- Mengetahui seberapa akurat rekomendasi yang diberikan oleh model, jika dibandingkan dengan rekomendasi yang diberikan oleh manusia

### Solution Statement

- Menggunakan pendekatan Content-based Filtering untuk mengetahui film yang mirip dengan Avatar(2009)
- Menggunakan Metrik Evaluasi Precission@k dan Mean Precission@k untuk melihat perbandingan dari rekomendasi yang diberikan sistem, dengan rekomendasi yang diberikan oleh manusia

## Data Understanding

di website kaggle diketahui bahwa Dataset ini menangkap detail tentang kumpulan film-film dari tahun 1910-2024 dilengkapi dengan data yang mendukung untuk sistem rekomendasi berbasis Content-based Filtering seperi Description, Writer, Director, dan Genres. Dataset ini diambil dari hasil penelitian seseorang. Terdapat total 16290 baris dan 10 kolom. saya mencantumkan detail mengenai fitur-fitur yang ada dari dataset yang digunakan pada tabel dibawah.

**Informasi Dataset**

| **Jenis**     | **Keterangan**                                                                                       |
|---------------|------------------------------------------------------------------------------------------------------|
| **Title**     | 16000+ Movies 1910-2024 (Metacritic)                                                                 |
| **Source**    | [Kaggle](https://www.kaggle.com/datasets/kashifsahil/16000-movies-1910-2024-metacritic)              |
| **Owner**     | [KashifSahil](https://www.kaggle.com/kashifsahil)                                                    |
| **License**   | Database: Open Database, Contents                                                                    |
| **Visibility**| Publik                                                                                               |
| **Tags**      | Movies and TV Shows, Arts and Entertainment, Data Analytics, Data Visualization, Recommender System  |
| **Usability** | 10.00                                                                                                |

### Berikut adalah informasi mengenai variabel-variabel pada dataset yang didapat dari kaggle :

- Title: Judul Film.
- Release Date: Tanggal rilis resmi dari film.
- Description: Sinopsis atau critical overview dari film.
- Rating: Rata-rata metacritic score.
- Number of Persons Voted: Jumlah orang yang melakukan voting untuk film tersebut.
- Directed by: Director dari film terebut.
- Written by: Screenwriter yang menulis cerita dari film tersebut.
- Duration: Durasi dari film.
- Genres: Genre dari film tersebut.

### Exploratory Data Analysis 

| #  | Column              | Non-Null Count | Dtype   |
|----|---------------------|----------------|---------|
| 0  | Unnamed: 0          | 16290          | int64   |
| 1  | Title               | 16290          | object  |
| 2  | Release Date        | 16290          | object  |
| 3  | Description         | 16290          | object  |
| 4  | Rating              | 12846          | float64 |
| 5  | No of Persons Voted | 12829          | object  |
| 6  | Directed by         | 16283          | object  |
| 7  | Written by          | 15327          | object  |
| 8  | Duration            | 16277          | object  |
| 9  | Genres              | 16285          | object  |

dari otuput diatas dapat dilihat bahwa:
- Unnamed: 0 memiliki 16290 data dengan tipe integer
- Title memiliki 16290 data bertipe object
- Release Date memiliki 16290 data bertipe object (seharusnya date time)
- Rating memiliki 12846 data bertipe float
- No of Persons Voted berisi 12829 data bertipe object (seharusnya integer)
- Directed by berisi 16283 data bertipe object
- Written by berisi 15327 data bertipe object
- Duration berisi 16277 data bertipe object
- Genres berisi 16285 data bertipe object

terlihat juga ada beberapa kolom yang berjumlah tidak sama, ini artinya terdapat missing value, maka kita akan mengatasinya pada proses PreProcessing, selanjutnya kita akan melihat data unik pada Genres, Directed by, dan Written by

| Keterangan     | Jumlah |
|----------------|--------|
| Banyak genre   | 1664   |
| Banyak data    | 11788  |
| Banyak data    | 7380   |

dapat disimpulkan bahwa, dalam dataset terdapat 1664 data dengan genre berbeda, 11788 data dengan penulis berbeda, dan 7380 data dengan sutradara yang berbeda.

**Melihat Informasi Statistik pada Rating**

| Statistik | Rating       |
|-----------|--------------|
| Count     | 12846.000000 |
| Mean      | 6.617632     |
| Std       | 1.415272     |
| Min       | 0.300000     |
| 25%       | 5.800000     |
| 50%       | 6.800000     |
| 75%       | 7.600000     |
| Max       | 10.000000    |

Terlihat bahwa rating memiliki nilai maksimum 10, dengan rata-rata 6.6

![Box Plot Illustration](https://github.com/Ganskuy/submission_SistemRekomendasi_Ghani/blob/main/resource/distribusi%20rating%20film.png?raw=true)

terlihat bahwa pada fitur rating, film dengan rating 7 adalah frekuensi terbanyak


## Data Preprocessing

**Mengecek Missing Value pada Data**

Pada proses ini, kita akan mengecek missing value pada data serta menanganinya

`df.isna().sum()`

| Column               | Missing Values |
|----------------------|----------------|
| Title                | 0              |
| Release Date         | 0              |
| Description          | 0              |
| Rating               | 3444           |
| No of Persons Voted  | 3461           |
| Directed by          | 7              |
| Written by           | 963            |
| Duration             | 13             |
| Genres               | 5              |

terlihat bahwa terdapat beberapa kolom yang berisi nilai NaN, yaitu pada kolom:
- Rating
- No of Persons Voted
- Directed by
- Written by
- Duration
- Genres

namun sebelum drop kolom-kolom yang memiliki nilai NaN, kita akan drop terlebih dahulu kolom yang tidak akan digunakan pada model, yaitu Duration, Release Date, No of Persons Voted, dan Unnamed: 0, alasannya antara lain:
- Duration hanya berisi durasi dari film tersebut
- Release Date berisi informasi mengenai kapan film tersebut rilis
- Unnamed: 0 hanya berisi nomor kolom
- No of Persons Voted : berisi jumlah orang yang sudah vote

**Drop kolom yang tidak relevan**

`df = df.drop(['Unnamed: 0', 'Release Date', 'No of Persons Voted', 'Duration'], axis=1)`

**Drop missing values**

`df=df.dropna()`

| Total Rows | Total Columns  |
|------------|----------------|
| 12351      | 6              |

Terlihat bawhwa dimensi pada dataset sudah berubah setelah kita drop kolom irrelevan dan baris yang berisi missing values

**Mengecek dan menangani data duplikat**

Pada sistem rekomendasi, jika pada dataset terdapat data duplikat, kita harus memastikan terlebih dahulu apakah data tersebut memang mungkin sama atau tidak, seperti pada kolom Directed by. Written by, dan Genres yang memungkinkan adanya data yang sama disitu, dan hal itu juga yang akan membantu model dalam mengenali similaritas antar data. Oleh karena itu kita akan mengecek data duplikat dengan berfokus pada fitur Title, karena tidak mungkin dan tidak bileh ada data duplikat disini.

`df['Title'].duplicated().sum()`

Setelah menjalankan code diatas, saya mendapati bahwa terdapat sebanyak 1275 data duplikat pada fitur Title, baris-baris tersebut lah yang akan kita drop

`df = df.drop_duplicates('Title')`

| Total Rows | Total Columns  |
|------------|----------------|
| 11076      | 6              |

Terlihat bahwa jumlah baris telah berkurang menjadi 11076 baris dengan 6 kolom

**Text Preprocessing**
Pada proses ini yang dilakukan adalah:
- Menghilangkan tulisan yang mengandung anomali seperi adanya \n
- Menghilangkan tanda baca dan melakukan case folding pada fitur Description, Directed by, dan Written by
- Menghilangkan tanda baca dan melakukan case folding pada fitur Genres, kemudian mengubahnya menjadi spasi agar memberikan bobot pada setiap genre pada film tersebut pada proses TF-IDF (TermFrequency-InverseDocumentFrequency)
- Lemmatization dan Stop Word Removal pada fitur Description
- Menggabungkan kolom Description, Directed by, Written by, dan Genres yang sudah dilakukan preprocessing, kedalam 1 kolom bernama 'info'
- Drop kolom Description, Directed by, Written by, dan Genres
- Menyimpan hasilnya kedalam variabel baru bernama 'data'

Dibawah ini ditampilkan 5 data acak dari dataset akhir yang disimpan dalam variabel data

| Index | Title                    | Info                                                                 |
|-------|--------------------------|----------------------------------------------------------------------|
| 0     | Dekalog (1988)           | drama krzysztof kieslowski krzysztof kieslowsk...                    |
| 1     | Three Colors: Red        | drama mystery romance krzysztof kieslowski krz...                    |
| 2     | The Conformist           | drama bernardo bertolucci alberto moravia bern...                    |
| 3     | Tokyo Story              | drama yasujirô ozu kôgo noda yasujirô ozu yasu...                    |
| 4     | The Leopard (re-release) | drama history luchino visconti giuseppe tomasi...                   | 

## Membuat Sistem Rekomendasi dengan Pendekatan Content-based Filtering

Content-based Filtering adalah metode rekomendasi yang menyarankan item berdasarkan kesamaan antara fitur item dan preferensi pengguna. Sistem ini menganalisis atribut (seperti genre, deskripsi, atau kategori) dari item yang disukai pengguna, lalu merekomendasikan item lain yang memiliki fitur serupa. Pada proyek ini, kita akan menggunakan Cosine Similarity sebagai metrik similaritas untuk memberikan top-N rekommendation dari film yang menjadi studi kasus kali ini, yaitu Avatar (2009). 

### Cosine Similarity

Cosine Similarity adalah metode untuk mengukur seberapa mirip dua vektor dalam ruang vektor, dengan menghitung nilai cosinus dari sudut di antara keduanya. Nilai cosine similarity berkisar antara -1 hingga 1, di mana 1 berarti kedua vektor identik (arahnya sama). Fitur yang ada pada suatu dokumen yang merupakan dimensi membentuk sebuah vektor, Kedua vektor yang terbentuk dari dua dokumen dapat dicari kemiripannya dengan menghitung jarak antar vektor [_(Fajriansyah et al., 2021)_](https://j-ptiik.ub.ac.id/index.php/j-ptiik/article/view/9163). Akan tetapi sebelum masuk ke tahap Development, variabel info yang berisi informasi tentang film akan diproses terlebih dahulu oleh TF-IDF (TermFrequency-InverseDocumentFrequency).

**Rumus Cosine Similarity**

![Box Plot Illustration](https://github.com/Ganskuy/submission_SistemRekomendasi_Ghani/blob/main/resource/cosine.png?raw=true)

Penjelasan:
- A . B adalah dot product (perkalian titik) antara vektor A dan B.
- ||A|| dan ||B|| adalah norma (panjang) dari masing-masing vektor.
- Ai dan Bi adalah elemen ke-i dari vektor A dan B.
- Σ Ai * Bi menghitung jumlah hasil kali elemen-elemen yang bersesuaian dari A dan B.
- √(Σ Ai²) dan √(Σ Bi²) menghitung panjang (magnitudo) dari masing-masing vektor.

### TF-IDF (TermFrequency-InverseDocumentFrequency)

TF-IDF merupakan angka statistik yang menunjukkan relevansi suatu termdengan beberapa dokumen sehingga termtersebut dapat menjadi kata kunci dari dokumen tertentu. Dari nilai tersebut beberapa dokumen tertentu dapat diidentifikasi atau dikategorikan [_(Fajriansyah et al., 2021)_](https://j-ptiik.ub.ac.id/index.php/j-ptiik/article/view/9163). 

![Box Plot Illustration](https://github.com/Ganskuy/submission_SistemRekomendasi_Ghani/blob/main/resource/Screenshot%202025-05-28%20at%2003.06.31.png?raw=true)

Proses perhitungan TF-IDF dimulai dengan menghitung term frequency (TF), yaitu seberapa sering sebuah term t muncul dalam dokumen d. Nilai ini menunjukkan pentingnya term tersebut dalam konteks satu dokumen. Selanjutnya, nilai tersebut dikalikan dengan inverse document frequency (IDF), yaitu ukuran seberapa jarangnya term tersebut muncul di seluruh kumpulan dokumen. Hasil dari perkalian TF dan IDF menghasilkan bobot akhir, yang menunjukkan seberapa penting suatu kata dalam dokumen tertentu dibandingkan dengan dokumen lainnya. TF-IDF membantu menyoroti kata-kata yang khas dan relevan bagi setiap dokumen.

  1.   ```from sklearn.feature_extraction.text import TfidfVectorizer```

       - Mengimpor class `TfidfVectorizer` dari pustaka scikit-learn.

  2.   ```vectorizer = TfidfVectorizer()```

       - menyimpan class TfidfVectorizer pada variabel vectorizer

  3.   ```tfidf_matrix = vectorizer.fit_transform(data['info'])```

       - Mengubah data pada kolom info menjadi array

  4.   ```tfidf_matrix.todense()```

       - mengubah tfidf_matrix kedalam bentuk matrix dengan fungsi todense()

### Development

  1.   ```from sklearn.metrics.pairwise import cosine_similarity```

       - Mengimpor class `cosine_similarity` dari pustaka scikit-learn.

  2.   ```cosine_sim = cosine_similarity(tfidf_matrix) ```

       - Mengaplikasikan cosine_similarity pada variabel tfid_matrix

  3. Menghasilkan Film Recommendation

```python
def film_recommendations(nama_film, similarity_data=cosine_sim_df, items=data[['Title', 'info']], k=10):
    index = similarity_data.loc[:, nama_film].to_numpy().argpartition(range(-1, -k-1, -1))
    closest = similarity_data.columns[index[-1:-(k+2):-1]]
    closest = closest.drop(nama_film, errors='ignore')

    similarity_scores = similarity_data.loc[closest, nama_film].values

    rekomendasi_df = pd.DataFrame(closest, columns=["Title"])
    rekomendasi_df["Similarity"] = similarity_scores

    return rekomendasi_df.merge(items, on="Title").sort_values(by="Similarity", ascending=False).head(k)
```

- Mengambil data dengan menggunakan argpartition untuk melakukan partisi secara tidak langsung sepanjang sumbu yang diberikan 
- Dataframe diubah menjadi numpy
- Urutan akan turun dari -1 sampai -k, dengan step -1
- Mengambil data dengan similarity terbesar dari index yang ada
- Mengambil skor similarity pada film-film yang direkomendasikan
- Drop nama_film agar nama film yang dicari tidak muncul dalam daftar rekomendasi
- menggunakan k=10, artinya model akan memberikan rekomendasi sebanyak 10 film

### Top-N Recommendation

```film_recommendations('Avatar') ```

Disini kita mencari rekomendasi film yang mirip dengan film Avatar (2009)

| No | Title                                         | Similarity | Info                                                                |
|----|-----------------------------------------------|------------|---------------------------------------------------------------------|
| 0  | Avatar: The Way of Water                      | 0.293632   | action adventure fantasy sci-fi james cameron ...                   |
| 1  | 5 Star Day                                    | 0.226471   | drama romance danny buday danny buday jake gib...                   |
| 2  | To Save a Life                                | 0.209381   | drama brian baugh brian baugh jim britts save ...                   |
| 3  | Miss Peregrine's Home for Peculiar Children   | 0.197852   | adventure drama family fantasy thriller tim bu...                   |
| 4  | Revolver                                      | 0.193388   | action crime drama mystery thriller guy ritchi...                   |
| 5  | Self Reliance                                 | 0.192403   | comedy thriller jake johnson jake johnson tomm...                   |
| 6  | Everybody Wants to Be Italian                 | 0.172340   | comedy romance jason todd ipson jason todd ips...                   |
| 7  | A Kid Like Jake                               | 0.157935   | drama silas howard daniel pearle brooklyn pare...                   |
| 8  | Just My Luck                                  | 0.146301   | comedy fantasy romance donald petrie i marlene...                   |
| 9  | The Girl from the Naked Eye                   | 0.143047   | action crime mystery romance thriller david re...                   |

Hasil diatas merupakan rekomendasi yang diberikan oleh sistem, kemudian kita akan menelusuri seberapa akuratnya rekomendasi yang diberikan jika kita bandingkan dengan Rekomendasi Manusia

## Evaluasi

Pada evaluasi ini, kita akan membandingkan, seberapa akurat model dapat merekomendasikan film, jika dibandingkan rekomendasi yang diberikan oleh manusia, Mterik yang digunakan adalah Precission@k dan Mean Precission@k. Precision@K adalah metrik evaluasi yang mengukur proporsi item relevan dari K hasil teratas yang direkomendasikan, dan Mean Precision@K (MP@K) adalah rata-rata dari Precision@K yang dihitung untuk banyak pengguna atau banyak kasus rekomendasi.

**Rmus Precission@k**
![Box Plot Illustration](https://github.com/Ganskuy/submission_SistemRekomendasi_Ghani/blob/main/resource/precission@k.png?raw=true)

**Rumus Mean Precission@k**
![Box Plot Illustration](https://github.com/Ganskuy/submission_SistemRekomendasi_Ghani/blob/main/resource/precission@k.png?raw=true)




         
- **Cara Kerja Model**:
  - K-Nearest Neighbors bekerja dengan mencari sejumlah tetangga terdekat dalam ruang fitur yang memiliki nilai target serupa. Setelah tetangga ditemukan, model akan menggunakan nilai rata-rata dari tetangga tersebut untuk membuat prediksi nilai target untuk data baru.

---

2. Random Forest
- **Proses Pembangunan Model**:

  1.   ```from sklearn.ensemble import RandomForestRegressor```

       - Mengimpor class `RandomForestRegressor` dari pustaka scikit-learn.

  2.   ```RF = RandomForestRegressor(n_estimators=50, max_depth=16, random_state=55, n_jobs=-1)```

       - Membuat objek dari class `RandomForestRegressor` dengan parameter yang ditentukan.

          - **Penjelasan Parameter**:

            - `n_estimators=50`: jumlah pohon yang dibangun sebanyak 50
            - `max_depth=16`: batas kedalam maksimum tiap pohon adalah 16 level, hal ini dilakukan untuk mencegah overfitting
            - `n_jobs=-1`: Menggunakan seluruh core CPU yang tersedia untuk mempercepat training

  3.   ```RF.fit(X_train, y_train)```

       - Memanggil metode fit untuk melatih model `RandomForestRegressor` menggunakan data pelatihan `X_train` dan `y_train`.

       - `X_train`: Matriks fitur untuk melatih model.

       - `y_train`: Nilai target yang sesuai dengan data fitur dalam `X_train`.
         
- **Cara Kerja Model**:
  - Random Forest bekerja dengan membuat banyak decision tree dari data yang diacak. Hasil prediksi dari semua pohon tersebut kemudian digabungkan — dengan cara voting (untuk klasifikasi) atau rata-rata (untuk regresi).

---

3. Ada Boost
- **Proses Pembangunan Model**:

  1.   ```from sklearn.ensemble import AdaBoostRegressor```

       - Mengimpor class `AdaBoostRegressor` dari pustaka scikit-learn.

  2.   ```boosting = AdaBoostRegressor(learning_rate=0.05, random_state=55)```

       - Membuat objek dari class `AdaBoostRegressor` dengan parameter yang ditentukan.

          - **Penjelasan Parameter**:

            - `learning_rate=0.05`: learning_rate=0.05 berarti, Mengontrol kontribusi masing-masing model lemah. Nilai yang kecil berarti model belajar lebih lambat tapi lebih stabil.
            - `random_state=55`: 55 “seed” digunakan untuk mengatur generator angka acak.

  3.   ```boosting.fit(X_train, y_train)```

       - Memanggil metode fit untuk melatih model `AdaBoostRegressor` menggunakan data pelatihan `X_train` dan `y_train`.

       - `X_train`: Matriks fitur untuk melatih model.

       - `y_train`: Nilai target yang sesuai dengan data fitur dalam `X_train`.
         
- **Cara Kerja Model**:
  - AdaBoost (Adaptive Boosting) bekerja dengan menggabungkan beberapa model lemah secara berurutan. Setiap model fokus pada kesalahan dari model sebelumnya dengan memberi bobot lebih besar pada data yang salah diprediksi. Model akhir adalah gabungan dari semua model lemah dengan bobot sesuai akurasinya.

---

**Kelebihan dan Kekurangan Dari Setiap Model**
| **Model**             | **Kelebihan**                                                                                                                                     | **Kekurangan**                                                                                                                              |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| **K-Nearest Neighbor**| - Mudah diimplementasikan  <br> - Cocok untuk data kecil dan non-linear                                                                           | - Lambat saat prediksi pada data besar  <br> - Sensitif terhadap data noisy & skala fitur  <br> - Tidak efektif pada data berdimensi tinggi |
| **Random Forest**     | - Akurasi tinggi & tahan overfitting  <br> - Bisa handle data numerik & kategorikal  <br> - Memberikan informasi feature importance               | - Kompleks & sulit diinterpretasikan  <br> - Butuh memori & waktu lebih banyak  <br> - Kurang cocok untuk prediksi real-time                |
| **Adaptive Boosting** | - Tingkatkan akurasi model sederhana  <br> - Fokus pada data yang sulit diklasifikasi  <br> - Risiko overfitting rendah jika dituning dengan baik | - Sensitif terhadap outlier  <br> - Performa buruk jika base model terlalu kompleks  <br> - Membutuhkan tuning parameter yang tepat         |

**Ketiga Model tersebut Cocok Karena**.

- K-Nearest Neighbor (KNN) model non-parametrik yang sederhana dan efektif. Ia mengklasifikasikan atau melakukan regresi berdasarkan kedekatan ke tetangga terdekat, sehingga sangat intuitif. KNN juga cocok digunakan karena dataset yang kita gunakan ini terbilang memiliki dimensi yang cukup kecil, sehingga cocok dengan algoritma KNN yang sederhana

- Random Forest Regressor menggabungkan hasil dari banyak pohon keputusan untuk menghasilkan prediksi yang lebih akurat dan robust. Ia juga memiliki mekanisme built-in untuk menangani overfitting dan dapat menangani data yang hilang dengan baik.

- AdaBoostRegressor Cocok digunakan karena dataset ini bersih, dan boosting bisa memperbaiki kesalahan iteratif, meningkatkan akurasi secara progresif.

## Evaluation

Dalam analisis ini digunakan metrik Mean Squared Error (MSE) untuk mengevaluasi kinerja model. MSE mengukur seberapa jauh prediksi model menyimpang dari nilai sebenarnya, dengan cara menghitung selisih antara nilai prediksi dan nilai aktual, mengkuadratkannya, lalu mengambil rata-rata dari seluruh selisih kuadrat tersebut. Nilai MSE yang lebih kecil menunjukkan bahwa prediksi model lebih mendekati nilai sebenarnya.

**Formula Mean Squared Error :**

![Box Plot Illustration](https://github.com/Ganskuy/Submission_PredictiveAnalytics/blob/main/resources/mse.jpg?raw=true)

**Cara Kerja MSE**
---

1. **Hitung Error**
   - Untuk setiap data, hitung selisih antara nilai aktual dan nilai prediksi:
     
     Errorᵢ = Yᵢ -  Ŷᵢ

2. **Kuadratkan Error**
   - Agar semua error bernilai positif dan memberi penalti lebih besar pada error yang besar:
     
     (Errorᵢ)²

3. **Hitung Rata-rata**
   - Jumlahkan semua error yang telah dikuadratkan dan bagi dengan jumlah data:
     
     MSE = (1/n) × ∑(i=1 to n) (Errorᵢ)²

---

**Evaluasi Model Machine Learning**

| Model     | Train MSE | Test MSE |
|-----------|-----------|----------|
| KNN       | 0.008466  | 0.011335 |
| RF        | 0.000731  | 0.004887 |
| Boosting  | 0.007777  | 0.009028 |

![Box Plot Illustration](https://github.com/Ganskuy/Submission_PredictiveAnalytics/blob/main/resources/train-test.png?raw=true)

| Index | y_true | prediksi_KNN  | prediksi_RF | prediksi_Boosting  |
|-------|--------|---------------|-------------|--------------------|
| 720   | 78.9   | 79.7          | 81.3        | 80.7               |

Dari tabel di atas, dapat dilihat bahwa setiap model menghasilkan prediksi yang bervariasi untuk setiap nilai aktual (y_true). Model K-Nearest Neighbor (KNN) menunjukan hasil yang paling mendekati nilai aktual, sementara Random Forest dan Ada Boost menunjukkan hasil yang kompetitif. Secara keseluruhan, performa model dapat bervariasi tergantung pada karakteristik data dan hubungan yang ada antara fitur dan target.

## Kesimpulan

Dari hasil analisis dan evaluasi yang dilakukan, dapat disimpulkan bahwa model yang dibangun berhasil menjawab dua rumusan masalah utama yang diajukan. Pertama, melalui analisis multivariat dan proses pelatihan model, dapat diidentifikasi bahwa sejumlah fitur seperti GDP, Schooling, BMI, dan Health Expenditure merupakan faktor yang paling signifikan memengaruhi angka harapan hidup (Life Expectancy). Fitur-fitur ini menunjukkan korelasi yang kuat dan relevan dalam menentukan seberapa tinggi tingkat kesehatan dan kesejahteraan populasi di suatu negara. Kedua, model regresi yang dikembangkan juga terbukti mampu memprediksi angka harapan hidup secara akurat berdasarkan fitur-fitur input. Dari evaluasi menggunakan metrik Mean Squared Error (MSE), diketahui bahwa Model Random Forest dan Boosting (AdaBoost) menunjukkan performa terbaik, dengan nilai MSE yang paling rendah baik pada data pelatihan maupun pengujian. Hal ini menandakan bahwa kedua model tersebut sangat efektif dalam menangkap hubungan kompleks antar variabel untuk memprediksi nilai target. Secara khusus, Random Forest unggul dalam menjaga keseimbangan antara akurasi dan generalisasi. prediksi pada indeks 720 menunjukkan bahwa seluruh model memberikan hasil yang cukup mendekati nilai aktual. Hal ini menunjukkan tingkat keandalan prediksi yang baik, terutama oleh KNN dan Boosting, meskipun RF menghasilkan nilai prediksi tertinggi.

## Referensi

Life Expectancy, Healthy Life Expectancy, and Burden of Disease in Older People in The Americas, 1990–2019: a Population-based Study [_( Link Jurnal )_](https://pmc.ncbi.nlm.nih.gov/articles/PMC8489742/)

# Link Repository GitHub

https://github.com/Ganskuy/Submission_PredictiveAnalytics
