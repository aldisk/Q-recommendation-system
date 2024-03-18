# Laporan Proyek Machine Learning - Aldiansyah Satrio Kabisat

## Project Overview

Banyak platform, seperti situs layanan streaming video ingin meningkatkan pengalaman pengguna mereka. Dengan menyediakan rekomendasi yang lebih tepat dan relevan, mereka dapat memperbaiki meningkatkan kepuasan pengguna dan user retention di platform mereka. Sistem rekomendasi yang baik dapat memberikan rekomendasi yang baik kepada pengguna tersebut.

Dengan semakin banyaknya data yang tersedia di internet, banyak dari orang bergantung terhadap sistem rekomendasi untuk menyeleksi informasi yang akan dikonsumsi (Alyari dan Navimipour, 2018). hal tersebut diharapkan dapat meningkatkan kepuasan pengguna sehingga kedepannya dapat meningkatkan performa suatu platform.

## Business Understanding

### Problem Statements

- Apakah dengan menggunakan sistem rekomendasi film memungkinkan pengguna untuk mencari film yang menarik tanpa harus menyeleksi banyak film?
- Sistem rekomendasi mana yang cukup sederhana sehingga dapat dijalankan dengan cukup cepat dan scalable

### Goals

- Mengetahui seberapa berguna sistem rekomendasi kepada pengguna
- Mengetahui jenis sistem rekomendasi mana yang sesuai dengan kondisi memerlukan rekomendasi secara cepat dan scalable

## Data Understanding
Dataset yang digunakan merupakan kumpulan film yang telah memiliki tag genre yang relevan dataset juga memiliki data review untuk sistem rekomendasi kolaboratif, pada kasus ini hanya digunakan data genre film.

Dataset dapat diakses pada laman berikut https://www.kaggle.com/datasets/parasharmanas/movie-recommendation-system

Dataset genre film yang digunakan memiliki beberapa variabel

Variabel-variabel pada Restaurant UCI dataset adalah sebagai berikut:
- movieId : id unik dari setiap film (Integer)
- title : nama dari setiap film (String)
- genres : tag genre dari film (Bar Separated Value)

Untuk memahami data lebih lanjut dilakukan beberapa analisis sebagai berikut : 

- Pengecekan data duplikat
  Data dengan nama dan tahun yang sama besar kemungkinan merupakan sebuah duplikat. Didapatkan beberapa data duplikat yang terdeteksi. Beberapa contoh data duplikat dapat dilihat sebagai berikut :
  
  ![image](https://github.com/aldisk/Q-recommendation-system/assets/95540779/4ffc0070-2f35-4600-a537-0aefecbe2188)

- Pengecekan film dengan genre yang tidak sesuai
  Terdapat beberapa data yang tidak memiliki tag, hal ini dapat berakibat pada film tersebut tidak pernah direkomendasikan atau mempersulit proses rekomendasi. Berikut beberapa contoh data tersebut :

  ![image](https://github.com/aldisk/Q-recommendation-system/assets/95540779/2f6d4458-b685-4b6f-a41d-3d1c4df4f3fd)

- Pengecekan frekuensi genre film
  Frekuensi sebuah genre film dapat mempengaruhi kemudahan dan ketepatan rekomendasi. Genre dengan frekuensi tinggi akan lebih mudah mendapatkan rekomendasi dan rekomendasi film terkait sedangkan untuk genre dengan frekuensi rendah berlaku kebalikannya.

  ![image](https://github.com/aldisk/Q-recommendation-system/assets/95540779/ad858e0c-32fe-43ca-bdd4-500f3928b32d)

  Dari grafik diatas dapat disimpulkan bahwa genre seperti Drama akan mudah mendapatkan rekomendasi dan rekomendasi film terkait, sedangkan film dengan genre seperti Musical dan Film-Noir akan lebih sulit.


## Data Preparation
Pada bagian Data preparation, dilakukan pemrosesan data sesuai dengan kebutuhan sistem dan permasalahan yang ditemukan pada saat eksplorasi dan analisis data. Data Preparation meliputi : 

- Menghilangkan data duplikat
  Data duplikat perlu dihilangkan agar tidak terjadi rekomendasi ganda dan self-recommendation. Proses ini dilakukan dengan menghapus salah satu data yang duplikat menggunakan method drop_duplicate dari pandas

- Menghapus data yang tidak memiliki tag
  Film tanpa tag dapat berakibat pada film tersebut tidak pernah direkomendasikan atau mempersulit proses rekomendasi. Proses ini dilakukan dengan menggunakan seleksi kondisi pada dataframe dengan tag 'no genres listed' dan kemudian menghapus hasil seleksi tersebut dari dataframe utama

- Encoding data genre
  genre pada data asli direpresentasikan dalam bentuk Bar Separated Value. Untuk mempermudah perhitungan dan pembacaan genre, data genre diubah menjadi list

## Modeling
Sistem rekomendasi yang dibuat akan menggunakan index Jaccard Similiarity untuk menentukan kemiripan sebuah film dengan film lainnya kemudian film dengan kemiripan tertinggi akan direkomendasikan. Hal ini memiliki beberapa keuntungan seperti komputasi yang sangat cepat dibandingkan metode lainnya dan scalable.

Berikut contoh rekomendasi yang diberikan oleh sistem pada data ujicoba

```
Base Data : Toy Story (1995) - Adventure,Animation,Children,Comedy,Fantasy

Recommendation Result :
Movie Similiar to Toy Story (1995):
Antz (1998) - 1.0
Toy Story 2 (1999) - 1.0
Adventures of Rocky and Bullwinkle, The (2000) - 1.0
Emperor's New Groove, The (2000) - 1.0
Monsters, Inc. (2001) - 1.0
DuckTales: The Movie - Treasure of the Lost Lamp (1990) - 1.0
Wild, The (2006) - 1.0
Shrek the Third (2007) - 1.0
Tale of Despereaux, The (2008) - 1.0
Asterix and the Vikings (Astérix et les Vikings) (2006) - 1.0
```

Dapat dilihat bahwa sistem secara umum memberikan rekomendasi dengan tag paling mirip. Hal ini dapat berarti rekomendasi yang diberikan sudah baik, namun juga dapat berakibat pada rekomendasi yang diberikan tidak memiliki nilai "Surprise" yang dapat menghambat proses eksplorasi pengguna

## Evaluation
Metrik evaluasi untuk metode ini akan menggunakan precision @K. Untuk performa kecepatan akan diukur secara empiris berdasarkan ujicoba dari proyek lainnya

Persamaan precision @K dapat dinyatakan sebagai berikut : 

$$
\text{Precision @K} = \frac{\text{Relevant Recommendation}}{\text{Total Recommendation}}
$$

Mengingat arti "relevan" akan cukup sulit diukur tanpa menggunakan ujicoba langsung pada pengguna, maka relevansi hasil pencarian akan didasarkan pada penandaan secara manual. Pada data ujicoba diatas memiliki presisi 1 dikarenakan keseluruhan hasil rekomendasi adalah relevan dikarenakan rekomendasi menghasilkan sekuel film, film dari studio yang sama, dan keseluruhan memberikan rekomendasi film kartun anak.

Jika diujikan pada data lain seperti berikut

```
Movie Similiar to Captain America: The First Avenger (2011):
X-Men: First Class (2011) - 1.0
Jurassic Park (1993) - 0.8
Independence Day (a.k.a. ID4) (1996) - 0.8
Escape from L.A. (1996) - 0.8
Abyss, The (1989) - 0.8
Escape from New York (1981) - 0.8
Star Trek: First Contact (1996) - 0.8
Star Trek II: The Wrath of Khan (1982) - 0.8
Lost World: Jurassic Park, The (1997) - 0.8
Spawn (1997) - 0.8
```

Hasil Rekomendasi juga cukup relevan mengingat semua film merupakan film aksi dan terdapat pula film X-Men yang merupakan cerita dalam satu franchise Captain America.

Secara umum presisi akhir yang didapatkan dari model mendekati 1.

Untuk perhitungan performa dari metode yang digunakan akan dibandingkan secara empiris terhadap Cosine Similarity dengan hasil yang dapat dilihat pada grafik berikut :

![image](https://github.com/aldisk/Q-recommendation-system/assets/95540779/1ef15db0-25e3-4697-b75b-53ab0bed34d8)
 *dikutip dari (Karthikeyan, 2022)*

Dapat dilihat bahwa secara empiris penggunaan Jaccard Similarity jauh lebih cepat dibandingkan dengan metode lainnya yang menggunakan Cosine Similarity dengan tetap memberhatikan kualitas rekomendasi yang dihasilkan

## Reference
- Alyari, F., & Jafari Navimipour, N. (2018). Recommender systems: A systematic review of the state of the art literature and suggestions for future research. Kybernetes, 47(5), 985-1017.
- Karthikeyan, Kiran. (2022). Time complexity for document similarity measures. Kaggle. https://www.kaggle.com/code/kirankarthikeyan/time-complexity-for-document-similarity-measures/notebook#All-pair-similarity

