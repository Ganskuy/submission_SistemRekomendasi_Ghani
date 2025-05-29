# Laporan Proyek Machine Learning - Muhammad Haifan Ghani (MC012D5Y1825)

## Topik Proyek : Rekomendasi Film

Sistem rekomendasi memainkan peran krusial dalam meningkatkan profitabilitas platform streaming seperti Netflix, dengan mempersonalisasi pengalaman pengguna dan secara signifikan mengurangi tingkat berhentinya pelanggan (churn). Melalui algoritma yang canggih, sistem ini mampu menyajikan konten yang relevan bagi setiap pengguna, yang pada akhirnya mendorong keterlibatan jangka panjang dan loyalitas pelanggan. The recommendation system is estimated to save Netflix about $1 billion per year by reducing churn and improving customer satisfaction [_(Gomez et al., 2021)_](https://dl.acm.org/doi/abs/10.1145/2843948). Dari banyaknya film yang diproduksi membuat calon penonton bingung dan kesulitan untuk mencari dan menentukan film apa yang akan ditonton selanjutnya sehingga menghabiskan waktu lebih banyak dalam mencari film [_(Fajriansyah et al., 2021)_](https://j-ptiik.ub.ac.id/index.php/j-ptiik/article/view/9163). Kutipan sebelumnya membuktikan seberapa pentungnya sistem rekomendasi ini pada aplikasi Streaming seperti Netflix dalam melancarkan bisnisnya dan memberikan pengalaman yang maksimal bagi pengguna.

Pada proyek ini, saya menggunakan film Avatar (2009) untuk dijadikan studi kasus dalam menilai seberapa baik Sistem Rekomendasi, yang dibuat menggunakan pendekatan Content-based Filtering, Cosine Similarity sebagai metrik similaritas, dan Precission@k serta Mean Precission@k sebagai metrik evaluasi. Harapannya proyek ini dapat membuka pengetahuan orang-orang tentang bagaimana Mesin memberikan sebuah rekomendasi kepada user, dan bagaimana perbandingannya dengan rekomendasi yang diberikan oleh manusia.

## Business Understanding

### Problem Statements

Rumusan masalah pada proyek ini adalah :
- Dilansir dari Website [Rotten Tomatoes](https://editorial-rottentomatoes-com.translate.goog/article/highest-grossing-movies-all-time/?_x_tr_sl=en&_x_tr_tl=id&_x_tr_hl=id&_x_tr_pto=tc) tentang film dengan penghasilan tertinggi sepanjang masa, Avatar (2009) merupakan film no. 1 dengan penghasilan tertinggi, dengan metode Content-Based Filtering, film apakah yang akan direkomendasikan untuk film yang mirip dengan Avatar?
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

**Mengecek data duplikat**

Pada sistem rekomendasi, jika pada dataset terdapat data duplikat, kita harus memastikan terlebih dahulu apakah data tersebut memang mungkin sama atau tidak, seperti pada kolom Directed by. Written by, dan Genres yang memungkinkan adanya data yang sama disitu, dan hal itu juga yang akan membantu model dalam mengenali similaritas antar data. Oleh karena itu kita akan mengecek data duplikat dengan berfokus pada fitur Title, karena tidak mungkin dan tidak bileh ada data duplikat disini.

`df['Title'].duplicated().sum()`

Setelah menjalankan code diatas, saya mendapati bahwa terdapat sebanyak 1275 data duplikat pada fitur Title, baris-baris tersebut lah yang akan kita drop


## Data Preprocessing

**Drop kolom yang tidak relevan**

`df = df.drop(['Unnamed: 0', 'Release Date', 'No of Persons Voted', 'Duration'], axis=1)`

**Drop missing values**

`df=df.dropna()`

| Total Rows | Total Columns  |
|------------|----------------|
| 12351      | 6              |

Terlihat bawhwa dimensi pada dataset sudah berubah setelah kita drop kolom irrelevan dan baris yang berisi missing values

**Menangani Data Duplikat**

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

### Ekstraksi Fitur dan Pembobotan Term Menggunakan TF-IDF (TermFrequency-InverseDocumentFrequency)

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

## Membuat Sistem Rekomendasi dengan Pendekatan Content-based Filtering

Content-based Filtering adalah metode rekomendasi yang menyarankan item berdasarkan kesamaan antara fitur item dan preferensi pengguna. Sistem ini menganalisis atribut (seperti genre, deskripsi, atau kategori) dari item yang disukai pengguna, lalu merekomendasikan item lain yang memiliki fitur serupa. Pada proyek ini, kita akan menggunakan Cosine Similarity sebagai metrik similaritas untuk memberikan top-N rekommendation dari film yang menjadi studi kasus kali ini, yaitu Avatar (2009). 

### Cosine Similarity

Cosine Similarity adalah metode untuk mengukur seberapa mirip dua vektor dalam ruang vektor, dengan menghitung nilai cosinus dari sudut di antara keduanya. Nilai cosine similarity berkisar antara -1 hingga 1, di mana 1 berarti kedua vektor identik (arahnya sama). Fitur yang ada pada suatu dokumen yang merupakan dimensi membentuk sebuah vektor, Kedua vektor yang terbentuk dari dua dokumen dapat dicari kemiripannya dengan menghitung jarak antar vektor [_(Fajriansyah et al., 2021)_](https://j-ptiik.ub.ac.id/index.php/j-ptiik/article/view/9163).

**Rumus Cosine Similarity**

![Box Plot Illustration](https://github.com/Ganskuy/submission_SistemRekomendasi_Ghani/blob/main/resource/cosine.png?raw=true)

Penjelasan:
- A . B adalah dot product (perkalian titik) antara vektor A dan B.
- ||A|| dan ||B|| adalah norma (panjang) dari masing-masing vektor.
- Ai dan Bi adalah elemen ke-i dari vektor A dan B.
- Σ Ai * Bi menghitung jumlah hasil kali elemen-elemen yang bersesuaian dari A dan B.
- √(Σ Ai²) dan √(Σ Bi²) menghitung panjang (magnitudo) dari masing-masing vektor.

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

Cosine Similarity bekerja dengan mengukur kemiripan arah antara dua vektor dalam ruang berdimensi tinggi.

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

Precision@K mengukur proporsi dari K item teratas yang relevan bagi pengguna. Nilainya berkisar dari 0 hingga 1. Semakin tinggi nilainya, semakin tepat sistem dalam memberikan rekomendasi yang relevan. 

**Rumus Mean Precission@k**
![Box Plot Illustration](https://github.com/Ganskuy/submission_SistemRekomendasi_Ghani/blob/main/resource/Screenshot%202025-05-28%20at%2017.30.19.png?raw=true)

Rumus ini menghitung rata-rata precision hanya pada posisi di mana item relevan muncul, lalu membaginya dengan jumlah item relevan tersebut. Nilainya menunjukkan seberapa baik sistem dalam menempatkan item relevan di urutan atas.

**Hasil Evaluasi**

| k  | Precision@k |
|----|-------------|
| 1  | 1.000000    |
| 2  | 0.500000    |
| 3  | 0.333333    |
| 4  | 0.250000    |
| 5  | 0.200000    |
| 6  | 0.166667    |
| 7  | 0.142857    |
| 8  | 0.125000    |
| 9  | 0.111111    |
| 10 | 0.100000    |

**Mean Average Precision@10 (MAP@10)**: `0.2929`

## Kesimpulan

Dalam proyek ini, digunakan metrik evaluasi Precision@K untuk mengukur seberapa relevan item-item yang direkomendasikan oleh sistem di antara k item teratas. Nilai relevansi diukur berdasarkan daftar film yang dianggap mirip dengan Avatar (2009), yang diambil dari artikel “10 Movies To Watch If You Love James Cameron’s Avatar” oleh  [CBR.com](https://www.cbr.com/movies-to-watch-if-you-like-avatar/).

Hasil evaluasi menunjukkan bahwa precision tertinggi terjadi pada k=1 dengan nilai 1.0, diikuti oleh penurunan secara bertahap hingga 0.1 pada k=10, yang berarti hanya 1 dari 10 film teratas yang benar-benar relevan menurut ground truth. Rata-rata presisi keseluruhan dihitung melalui Mean Average Precision@10 (MAP@10) dengan hasil 0.29 atau sekitar 29%, menunjukkan bahwa secara umum sistem hanya mampu menangkap sebagian kecil relevansi dari preferensi yang ditentukan secara manual.

Perbedaan ini dapat dijelaskan oleh perbedaan pendekatan antara mesin dan manusia, mesin mengandalkan kesamaan fitur tekstual (seperti genre, sinopsis, dan informasi teknis), sedangkan data ground truth didasarkan pada opini dan konteks naratif dari manusia. Meski begitu, sistem berhasil memberikan rekomendasi yang cukup relevan secara data pada Top-N Recommencation, terutama karena kemiripan genre seperti Action, Adventure, dan Romance. Namun, sistem belum berhasil menangkap genre Science Fiction, yang justru merupakan elemen penting dan khas dari film Avatar.

Dengan demikian, model content-based filtering ini sudah cukup baik, namun masih memiliki ruang untuk ditingkatkan—misalnya dengan mempertimbangkan bobot genre tertentu yang lebih menonjol atau menggabungkan pendekatan hybrid.

## Referensi

Sistem Rekomendasi Film Menggunakan Content Based Filtering [_( Link Jurnal )_](https://j-ptiik.ub.ac.id/index.php/j-ptiik/article/view/9163)

The Netflix Recommender System: Algorithms, Business Value, and Innovation [_( Link Jurnal )_](https://dl.acm.org/doi/abs/10.1145/2843948)

THE 50 HIGHEST-GROSSING MOVIES OF ALL TIME: YOUR TOP BOX OFFICE EARNERS EVER WORLDWIDE [_( Link Websitel )_](https://editorial-rottentomatoes-com.translate.goog/article/highest-grossing-movies-all-time/?_x_tr_sl=en&_x_tr_tl=id&_x_tr_hl=id&_x_tr_pto=tc)

10 Movies To Watch If You Love James Cameron's Avatar [_( Link Websitel )_](https://www.cbr.com/movies-to-watch-if-you-like-avatar/)

# Link Repository GitHub

https://github.com/Ganskuy/submission_SistemRekomendasi_Ghani
