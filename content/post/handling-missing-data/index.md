---
title: "Handling_Missing_Data"
date: 2023-08-02T03:13:32+07:00
author: admin
description: Handling Missing Data Value in r
tags:
  - Wrangling
---

## Handling Missing Data in r

Dalam data science, seringkali kita harus menghadapi data yang hilang.
Untuk mengatasi masalah ini dalam pemrograman R, ada beberapa cara yang
bisa kita lakukan. Pertama, kita bisa menghapus baris atau kolom yang
mengandung data yang hilang. Cara lain adalah dengan menggantikan data
yang hilang menggunakan nilai yang diestimasi berdasarkan data yang
tersedia. Misalnya, kita bisa mengisi nilai yang hilang dengan rata-rata
atau median dari data yang sejenis.


### Data yang Hilang

Dalam R, kita menggunakan simbol NA untuk menunjukkan nilai yang hilang,
dan simbol NAN digunakan untuk operasi yang tidak mungkin seperti
membagi dengan nol. Secara sederhana, NA atau NAN merepresentasikan data
yang tidak tersedia dalam R.

Misalkan seorang guru memasukkan nilai semua muridnya ke dalam sebuah
spreadsheet. Sayangnya, ia lupa memasukkan data dari satu siswa di
kelasnya. Inilah yang disebut sebagai data yang hilang secara praktis.

### Menemukan Data yang Hilang di R

Fungsi bawaan di R, yaitu `is.na()`, digunakan untuk menemukan nilai
yang hilang (NA) dalam suatu dataset. Fungsi ini mengembalikan vektor
berisi nilai logika (True atau False). Jika nilai dalam vektor adalah
NA, maka nilai yang sesuai dalam vektor logika akan menjadi True; jika
bukan NA, nilai logika akan menjadi False. Dengan menggunakan fungsi
`is.na()`, kita dapat dengan mudah mengidentifikasi nilai-nilai yang
hilang dalam dataset.

***Contoh :***

``` r
data(penguins)
dim(penguins)
```

    ## [1] 344   8

melihat banyaknya nilai `NA`

``` r
sum(is.na(penguins))
```

    ## [1] 19

memilih baris-baris yang mengandung nilai `NA`

``` r
which(is.na(penguins))
```

    ##  [1]  692  960 1036 1304 1380 1648 1724 1992 2068 2073 2074 2075 2076 2112 2243
    ## [16] 2283 2321 2333 2336

Fungsi bawaan di R, yaitu `is.nan()`, digunakan untuk menemukan nilai
yang disebut “bukan angka” (NAN) dalam suatu dataset. Fungsi ini
mengembalikan vektor berisi nilai logika (True atau False). Jika ada
nilai NAN dalam vektor, nilai logika yang sesuai dalam vektor tersebut
akan menjadi True; jika tidak, nilai logika akan menjadi False. Dengan
menggunakan fungsi `is.nan()`, kita dapat dengan mudah mengidentifikasi
nilai-nilai NAN dalam dataset.

***Contoh***

``` r
myVector <- c(NA, 100, 241, NA, 0 / 0, 101, 0 / 0)
is.nan(myVector)
```

    ## [1] FALSE FALSE FALSE FALSE  TRUE FALSE  TRUE

### Menghapus Data/Nilai yang Hilang

Mari kita pertimbangkan sebuah skenario di mana kita ingin memfilter
nilai kecuali nilai yang hilang. Dalam R, kita memiliki dua cara untuk
menghapus nilai yang hilang. Metode-metode ini dijelaskan di bawah ini -

Fungsi-fungsi pemodelan di R memiliki parameter na.action yang
memungkinkan kita untuk menentukan bagaimana mengatasi nilai-nilai yang
hilang dalam dataset. Berikut adalah beberapa opsi yang dapat digunakan:

- `na.omit`: Fungsi ini akan menghapus seluruh baris yang mengandung
  nilai yang hilang dari dataset. Baris yang memiliki nilai NA akan
  diabaikan dan tidak dimasukkan ke dalam analisis atau pemodelan.

- `na.exclude`: Fungsi ini akan mengabaikan baris yang mengandung
  setidaknya satu nilai yang hilang dalam analisis atau pemodelan. Baris
  tersebut akan tetap ada dalam dataset, tetapi nilai NA dianggap
  sebagai missing value yang akan diabaikan dalam perhitungan.

- `na.pass`: Fungsi ini tidak melakukan tindakan apa pun terhadap nilai
  yang hilang. Jika ada nilai NA dalam dataset, fungsi akan membiarkan
  nilai tersebut dan membiarkan analisis atau pemodelan berjalan seperti
  biasa. Ini dapat digunakan jika kita ingin menangani nilai-nilai yang
  hilang secara manual.

- `na.fail`: Fungsi ini akan menghentikan eksekusi atau memberikan error
  jika ada nilai yang hilang dalam dataset. Hal ini memastikan bahwa
  kita sadar akan keberadaan nilai NA dalam dataset dan memaksa kita
  untuk menangani nilai-nilai tersebut secara eksplisit sebelum
  melanjutkan analisis atau pemodelan.

Pilihan `na.omit` dan `na.exclude` sering digunakan untuk menghapus atau
mengabaikan baris yang mengandung nilai NA sehingga kita bisa
melanjutkan analisis atau pemodelan dengan data yang lengkap.

***Contoh***

``` r
Vector <- c(NA, "TP", 4, 6.7, 'c', NA, 12)
na.exclude(Vector)
```

    ## [1] "TP"  "4"   "6.7" "c"   "12" 
    ## attr(,"na.action")
    ## [1] 1 6
    ## attr(,"class")
    ## [1] "exclude"

``` r
Vector <- c(NA, "TP", 4, 6.7, 'c', NA, 12)
na.omit(Vector)
```

    ## [1] "TP"  "4"   "6.7" "c"   "12" 
    ## attr(,"na.action")
    ## [1] 1 6
    ## attr(,"class")
    ## [1] "omit"

Untuk memilih nilai-nilai yang tidak hilang (non-NA atau non-NAN), kita
perlu membuat vektor logika yang berisi nilai True untuk nilai yang
hilang (NA atau NAN) dan nilai False untuk nilai lain dalam vektor yang
diberikan. Fungsi is.nan() digunakan untuk memeriksa nilai NAN dalam
vektor. Fungsi ini menghasilkan vektor logika dengan nilai True untuk
setiap nilai NAN dalam vektor dan nilai False untuk nilai lainnya.
Dengan demikian, kita dapat menggunakan vektor logika ini untuk memilih
nilai-nilai yang bukan NA atau NAN dari vektor awal.

***Contoh***

``` r
vektor <- c(100, 200, NA, NA, NA, 49, NA, 190)
print(vektor)
```

    ## [1] 100 200  NA  NA  NA  49  NA 190

``` r
na_vektor <- is.na(vektor)
new_vektor <- vektor[!na_vektor]
print(new_vektor)
```

    ## [1] 100 200  49 190

Menerapkan fungsi `is.nan()`

``` r
vektor1 <- c(100, 200, 300, 400, 0/0, 0/0, 0/0, 500, 0/0)
print(vektor1)
```

    ## [1] 100 200 300 400 NaN NaN NaN 500 NaN

``` r
na_vektor1 <- is.nan(vektor1)
new_vektor1 <- vektor1[!na_vektor1]
print(new_vektor1)
```

    ## [1] 100 200 300 400 500

Seperti yang dapat Anda lihat pada output, nilai yang hilang dari tipe
NA dan NAN telah berhasil dihapus dari variabel vektor

### Mengisi Nilai yang Hilang dengan Mean atau Median

Pada bagian ini, kita akan belajar cara mengisi nilai yang hilang dalam
dataset menggunakan rata-rata dan median. Untuk melakukannya, kita akan
menggunakan metode “apply” untuk menghitung rata-rata dan median dari
setiap kolom yang memiliki nilai hilang.

Langkah pertama yang harus dilakukan adalah mendapatkan daftar
kolom-kolom yang memiliki setidaknya satu nilai yang hilang (NA). Dengan
begitu, kita bisa fokus pada kolom-kolom yang memerlukan pengisian nilai
hilang.

***Contoh***

``` r
dataframe <- data.frame(
   Name = c("name1", "name2", "name3", "name4"),
   Physics = c(87, 67, 98, 90),
   Chemistry = c(NA, 84, 93, 87),
   Mathematics = c(91, 86, NA, NA)
   )
dataframe
```

    ##    Name Physics Chemistry Mathematics
    ## 1 name1      87        NA          91
    ## 2 name2      67        84          86
    ## 3 name3      98        93          NA
    ## 4 name4      90        87          NA

Mari kita cetak nama kolom yang memiliki setidaknya satu nilai NA.

``` r
listMissingColumn <- colnames(dataframe)[apply(dataframe, 2, anyNA)]
print(listMissingColumn)
```

    ## [1] "Chemistry"   "Mathematics"

Dalam dataframe kita, terdapat dua kolom yang memiliki nilai yang hilang
(NA).

Selanjutnya, pada langkah kedua, kita perlu menghitung nilai rata-rata
(mean) dan nilai tengah (median) dari kolom-kolom tersebut. Karena kita
harus mengabaikan nilai-nilai yang hilang dalam perhitungan, kita akan
menggunakan argumen `"na.rm = TRUE"` saat menggunakan fungsi `apply()`.
Argumen ini akan memastikan bahwa nilai-nilai NA diabaikan saat
menghitung mean dan median untuk kolom-kolom tersebut.

``` r
meanMissing <- apply(dataframe[, colnames(dataframe) %in% listMissingColumn], 2, mean, na.rm = TRUE)
print(meanMissing)
```

    ##   Chemistry Mathematics 
    ##        88.0        88.5

Nilai rata-rata Kolom Kimia adalah 88,0 dan Matematika adalah 88,5.

``` r
medianMissing <- apply(dataframe[, colnames(dataframe) %in% listMissingColumn], 2, median, na.rm = TRUE)
print(medianMissing)
```

    ##   Chemistry Mathematics 
    ##        87.0        88.5

Nilai tengah kolom kimia adalah 87.0 dan matematika adalah 88.5

Langkah 3 - Sekarang nilai rata-rata dan median dari kolom-kolom yang
sesuai sudah siap. Pada langkah ini, kita akan mengganti nilai NA dengan
mean dan median menggunakan fungsi mutate() yang didefinisikan dalam
paket “dplyr”.

``` r
library(dplyr)
```

``` r
newDataFrame <- dataframe %>%
  mutate(Chemistry = ifelse(is.na(Chemistry), meanMissing[1], Chemistry),
         Mathematics = ifelse(is.na(Mathematics), meanMissing[2], Mathematics))

newDataFrame
```

    ##    Name Physics Chemistry Mathematics
    ## 1 name1      87        88        91.0
    ## 2 name2      67        84        86.0
    ## 3 name3      98        93        88.5
    ## 4 name4      90        87        88.5
