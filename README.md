# E-Commerce Shipping Data

![ecommerce-shipping-1](https://user-images.githubusercontent.com/89932073/218234363-9b211cc4-900e-457a-ac9f-0286129bc86d.png)

Dataset yang digunakan memiliki 10.999 baris dari 12 fitur yang tersedia.

Data memiliki beberapa informasi sebagai berikut:
- ID : Nomor ID pelanggan.
- Warehouse_block : Warehouse (gudang) dari perusahaan yang dibagi menjadi beberapa blok, yaitu A, B, C, D, dan E.
- Mode of Shipment : Cara pengiriman produk, yang meliputi jalur laut (Ship), jalur udara (Flight), dan jalur darat (Road).
- Customer_care_calls : Jumlah panggilan yang dilakukan untuk pelacakan pengiriman.
- Customer_rating : Rating/penilaian dari pelanggan, dimulai dari 1 terendah (terburuk) sampai 5 tertinggi (terbaik).
- Cost_of_the_Product : Biaya produk (dalam US Dollars).
- Prior_purchases : Jumlah pembelian sebelumnya.
- Product_importance : Kategori produk, dari Low (rendah), Medium (sedang), dan High (Tinggi).
- Gender : Jenis kelamin, Male (Laki-laki) dan Female (Perempuan).
- Discount_offered : Banyak diskon yang ditawarkan untuk produk tertentu.
- Weight_in_gms : Berat produk (dalam satuan gram)
- Reached.on.Time_Y.N : Ketepatan produk tiba ke destinasi pelanggan, yaitu angka **1** menunjukkan produk **tidak sampai tepat waktu** dan angka **0** menunjukkan produk **sampai tepat waktu**.

## Prerequisites

1. Download data [here](https://www.kaggle.com/datasets/prachi13/customer-analytics).

2. Instalasi packages
```code
pip install pandas
pip install numpy
pip install matplotlib
pip install seaborn
pip install scipy
```

## Exploratory Data Analysis
### Statistical Summary
- Dari fitur yang tersedia, tidak terdapat **missing values**.
- Semua tipe data telah sesuai, hanya saja pada kolom warehouse_block, data yang semestinya adalah blok A, B, C, D, **E** menjadi A, B, C, D, **F**.
- Kemungkinan fitur **Prior_purchases** dan **Discount_offered** memiliki data yang akan **outlier** berdasarkan perhitungan IQR karena masing-masing memiliki **nilai max** yang melewati **batas max** pada IQR, yaitu Q3 + (1.5 x IQR).
- Berdasarkan pada perbedaan nilai mean dan median (mean > median), **Discount_offered** dan **Prior_purchases** diprediksi akan memiliki distribusi **positively skewed**.
- Pada fitur kategorikal:
    1. Kategori Warehouse_block dari 5 blok, blok **F** mendominasi sekitar **33%**. 
    2. Kategori Mode_of_shipment dari 3 jenis armada pengiriman, jalur **laut (Ship)** mendominasi sekitar **67,8%**.
    3. Kategori Product_importance dari 3 kategori, kategori **Low** mendominasi sekitar **48%**.
    4. Walaupun kategori **Gender** didominasi oleh **Female**, perbedaan jumlahnya **tidak signifikan** dengan **Male**. Oleh sebab itu, fitur **Gender tidak berpengaruh signifikan** terhadap model yang akan dibuat.
    5. Berdasarkan fitur Reached.on.Time_Y.N dari jumlah total **10.999** produk, sebanyak **6563** produk tidak sampai tepat waktu


### Univariate Analysis
- Pada visualisasi fitur numerikal diperoleh beberapa insight:
    1. Pada fitur **Prior_purchase** terdapat **outlier** yang mencolok dan **tidak terlihat garis median** di dalam kotak IQR.
    2. Pada fitur **Discount_offered** terdapat **banyak sekali outlier** dan berpotensi memiliki probabilitas distribusi **right-skewed**.
    3. Pada fitur **Weight_in_gms** memiliki **potensi** probabilitas distribusi yang **skewed**.
    4. Distribusi pada fitur **Discount_offered** yang **right-skewed** terkonfirmasi.
    5. Pada fitur **Weight_in_gms** terdapat **dua puncak** (pada range 0-2000 dan 4000-6000).
- Pada visualisasi fitur kategorikal diperoleh beberapa insight:
    1. Dalam barplot **Warehouse_block** terdapat penumpukan barang pada gudang **F**.
    2. Dalam barplot **Mode_of_Shipment**, pelanggan lebih banyak memilih moda transportasi pengiriman **kapal**.
    3. Dalam barplot **Product_importance**, pelanggan lebih memilih prioritas **low & medium**.
 
## Multivariate Analysis
- Pada heatmap korelasi diperoleh beberapa insight:
    1. **Reached_on_time** memiliki **korelasi positif** dengan **Discount_offered** sebesar **0.40**, sehingga kemungkinan semakin tinggi discount yang diberikan akan semakin memungkinkan untuk paket datang terlambat.
    2. **Reached_on_time** memiliki **korelasi negatif** dengan **Weight_in_gms** sebesar **-0.27**, sehingga kemungkinan semakin berat beban yang diangkut maka semakin memungkinkan untuk paket datang tepat waktu
    3. **Weight_in_gms** memiliki **korelasi negatif** dengan **Discount_offered** sebesar **-0.38**, sehingga kemungkinan semakin tinggi discount yang diberikan, maka semakin ringan beban barang yang diangkut.
    4. **Customer_care_calls** memiliki **korelasi positif** dengan **Cost_of_the_product** sebesar **0.32**, sehingga kemungkinan semakin mahal barang yang diantar, maka semakin banyak juga panggilan yang diterima.
    5. **Weight_in_gms** memiliki **korelasi negatif** dengan **Customer_care_calls** sebesar **-0.28**, sehingga kemungkinan semakin berat beban barang, maka semakin sedikit panggilan yang diterima.
- Pada visualisasi beberapa fitur yang tersedia di dataset diperoleh beberapa insight:
    1. Hubungan antara **Reached.on.time** dengan **Discount_offered**, terlihat **sebagian besar data** berada pada nilai Reached.on.time 1 yang artinya **terlambat** dan **hanya** barang yang di **discount dibawah sekitar 10%** yang **tidak** mengalami keterlambatan.
    2. **Weight_in_gms** dengan **Discount_offered**, dimana terlihat barang-barang yang diberi **discount kebanyakan **dibawah** beban **4000 gram**, dan beban pada **4001-6000 gram** hanya mendapat **discount maksimal 10%** padahal memiliki frekuensi (jumlah barang) yang paling tinggi.
    3. **Weight_in_gms**, **Reached.on.time**, dan **Cost_of_the_Product**. Pada area sekitar **median Weight_in_gms** dimana memiliki **frekuensi terendah**, data Reached.on.time tercatat **terlambat semua** dan data **Cost_of_the_Product relatif sangat tinggi**.
