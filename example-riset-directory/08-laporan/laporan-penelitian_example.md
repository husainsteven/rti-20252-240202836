# Laporan Penelitian

**Judul:** Analisis Perbandingan Algoritma Decision Tree dan Random Forest untuk Deteksi Serangan Denial of Service (DoS) pada Jaringan Internet of Things Berbasis Protokol MQTT

**Peneliti:** Husain Stefano
**Status Penelitian:** Implementasi algoritma, pengujian, analisis hasil, dan penyusunan artikel ilmiah telah selesai.
---

## 1. Ringkasan Eksekutif

Penelitian ini bertujuan membandingkan performa algoritma Decision Tree dan Random Forest dalam mendeteksi serangan Denial of Service (DoS) pada jaringan Internet of Things (IoT) yang menggunakan protokol Message Queuing Telemetry Transport (MQTT). Penelitian dilakukan menggunakan pendekatan eksperimen dengan memanfaatkan dataset lalu lintas jaringan MQTT yang terdiri atas data aktivitas normal dan data serangan.

Tahapan penelitian dimulai dari proses preprocessing data, pembagian dataset menjadi data latih dan data uji, implementasi algoritma Decision Tree dan Random Forest, serta evaluasi performa menggunakan metrik Accuracy, Precision, Recall, F1-Score, dan waktu eksekusi. Untuk memperoleh hasil yang lebih objektif, pengujian dilakukan sebanyak lima kali menggunakan nilai random seed yang berbeda. Selain itu, dilakukan analisis statistik menggunakan Shapiro-Wilk Test, Paired Sample t-test, dan Cohen's d untuk mengetahui signifikansi perbedaan performa kedua algoritma.

Hasil penelitian menunjukkan bahwa kedua algoritma mampu melakukan klasifikasi lalu lintas jaringan dengan tingkat akurasi di atas 93%. Hasil uji statistik menunjukkan bahwa tidak terdapat perbedaan yang signifikan pada metrik Accuracy, Recall, dan F1-Score, sedangkan terdapat perbedaan signifikan pada metrik Precision. Dari sisi efisiensi komputasi, Decision Tree memiliki waktu eksekusi yang jauh lebih cepat dibandingkan Random Forest. Oleh karena itu, kedua algoritma memiliki kelebihan masing-masing sesuai dengan kebutuhan implementasi pada sistem deteksi intrusi di lingkungan Internet of Things.


---

## 2. Latar Belakang dan Rumusan Masalah

### 2.1 Latar Belakang

Perkembangan Internet of Things (IoT) telah mendorong penggunaan berbagai perangkat yang saling terhubung melalui jaringan internet untuk mendukung otomasi industri, smart home, maupun smart city. Salah satu protokol komunikasi yang banyak digunakan pada perangkat IoT adalah Message Queuing Telemetry Transport (MQTT) karena memiliki ukuran paket yang ringan dan konsumsi bandwidth yang rendah. Meskipun demikian, karakteristik tersebut juga menjadikan MQTT rentan terhadap berbagai serangan siber, salah satunya adalah Denial of Service (DoS).

Serangan DoS bertujuan membanjiri sistem dengan lalu lintas jaringan dalam jumlah besar sehingga layanan menjadi lambat bahkan tidak dapat diakses oleh pengguna yang sah. Kondisi tersebut dapat mengganggu proses komunikasi antarperangkat IoT yang membutuhkan pertukaran data secara real-time. Oleh karena itu, diperlukan suatu mekanisme yang mampu mendeteksi aktivitas serangan sejak dini agar dampak yang ditimbulkan dapat diminimalkan.

Salah satu pendekatan yang banyak digunakan dalam sistem deteksi intrusi adalah penerapan algoritma machine learning. Algoritma Decision Tree memiliki keunggulan dalam menghasilkan model yang sederhana dan mudah diinterpretasikan, sedangkan Random Forest memanfaatkan pendekatan ensemble learning yang mampu meningkatkan stabilitas dan akurasi klasifikasi. Oleh karena itu, penelitian ini membandingkan kedua algoritma tersebut untuk mengetahui performa terbaik dalam mendeteksi serangan DoS pada jaringan IoT berbasis MQTT.

### 2.2 Rumusan Masalah

1. Mengimplementasikan algoritma Decision Tree dan Random Forest pada dataset lalu lintas jaringan MQTT.
2. Membandingkan performa kedua algoritma berdasarkan metrik Accuracy, Precision, Recall, dan F1-Score.
3. Menganalisis signifikansi perbedaan performa menggunakan Shapiro-Wilk Test, Paired Sample t-test, dan Cohen's d.
4. Memberikan rekomendasi algoritma yang sesuai untuk diterapkan pada sistem deteksi serangan DoS pada jaringan Internet of Things berbasis MQTT.

### 2.3 Tujuan Penelitian

Detail tujuan & kontribusi: lihat [../01-proposal/proposal-penelitian.md](../01-proposal/proposal-penelitian.md) §3 dan §5, serta [../07-manuskrip/02-pendahuluan.md](../07-manuskrip/02-pendahuluan.md).

---

## 3. Metodologi dan Pelaksanaan

Penelitian ini menggunakan metode eksperimen dengan pendekatan kuantitatif untuk membandingkan performa algoritma Decision Tree dan Random Forest dalam mendeteksi serangan Denial of Service (DoS) pada jaringan Internet of Things (IoT) berbasis protokol Message Queuing Telemetry Transport (MQTT). Seluruh proses penelitian diimplementasikan menggunakan bahasa pemrograman Python pada lingkungan Google Colaboratory dengan memanfaatkan pustaka Pandas, NumPy, Scikit-learn, Matplotlib, dan SciPy.

Tahapan penelitian meliputi pengumpulan dataset, preprocessing data, pembagian data menjadi data latih dan data uji, pembangunan model Decision Tree dan Random Forest, pengujian model sebanyak lima kali menggunakan nilai random seed yang berbeda, evaluasi performa model, serta analisis statistik menggunakan Shapiro-Wilk Test, Paired Sample t-test, dan Cohen's d.

### 3.1 Dataset Penelitian

Dataset yang digunakan merupakan dataset lalu lintas jaringan IoT berbasis protokol MQTT yang berisi data komunikasi normal dan data serangan Denial of Service (DoS). Dataset terdiri atas sejumlah atribut yang merepresentasikan karakteristik lalu lintas jaringan serta satu atribut kelas sebagai target klasifikasi.

Sebelum digunakan dalam proses pelatihan model, dataset diperiksa untuk memastikan tidak terdapat kesalahan format data maupun nilai yang dapat memengaruhi hasil klasifikasi.


### 3.2 Tahapan Penelitian

Penelitian dilaksanakan melalui beberapa tahapan sebagai berikut.

a. Pengumpulan Dataset

Dataset MQTT diperoleh dari sumber dataset publik yang telah banyak digunakan pada penelitian keamanan jaringan Internet of Things. Dataset kemudian diimpor ke dalam lingkungan Google Colaboratory untuk diproses lebih lanjut.

b. Preprocessing Data

Tahap preprocessing dilakukan untuk meningkatkan kualitas data sebelum digunakan pada proses klasifikasi. Langkah-langkah yang dilakukan meliputi:

membaca dataset menggunakan Pandas,
memeriksa missing value,
menghapus atribut yang tidak digunakan,
melakukan Label Encoding terhadap atribut kategorikal,
memisahkan fitur dan label,
membagi dataset menjadi data latih dan data uji.
c. Implementasi Decision Tree

Model Decision Tree dibangun menggunakan library Scikit-learn. Algoritma ini membentuk struktur pohon keputusan berdasarkan atribut yang memiliki nilai information gain terbaik sehingga dapat mengklasifikasikan data menjadi kelas normal maupun serangan.

d. Implementasi Random Forest

Random Forest dibangun menggunakan kumpulan beberapa Decision Tree yang dibentuk melalui teknik bootstrap sampling. Hasil prediksi akhir diperoleh menggunakan metode majority voting sehingga model memiliki kemampuan generalisasi yang lebih baik dibandingkan Decision Tree tunggal.

e. Evaluasi Model

Evaluasi dilakukan menggunakan beberapa metrik yaitu:

Accuracy
Precision
Recall
F1-Score
Execution Time

Selain itu dilakukan pengujian statistik menggunakan:

Shapiro-Wilk Test
Paired Sample t-test
Cohen's d

untuk mengetahui apakah terdapat perbedaan performa yang signifikan antara kedua algoritma.

---

## 4. Hasil Penelitian

Ringkasan hasil (detail lengkap & interpretasi: [../07-manuskrip/05-hasil-analisis.md](../07-manuskrip/05-hasil-analisis.md) dan [../09-docs/tahap-4-analisis-data.md](../09-docs/tahap-4-analisis-data.md)).

### 4.1 Hasil Implementasi Model

Setelah seluruh tahapan preprocessing selesai dilakukan, model Decision Tree dan Random Forest dilatih menggunakan data latih yang telah dipersiapkan. Selanjutnya kedua model digunakan untuk memprediksi kelas pada data uji.

Pengujian dilakukan sebanyak lima kali menggunakan nilai random seed yang berbeda yaitu 42, 123, 456, 789, dan 2025. Penggunaan beberapa random seed bertujuan untuk mengetahui tingkat konsistensi performa masing-masing algoritma.

### 4.2 Hasil Pengujian Decision Tree

Tabel 4.2 Hasil Evaluasi Decision Tree

No	Seed	Accuracy	Precision	Recall	F1-Score	Waktu (detik)
1	42	0,939141	0,939804	0,939141	0,937902	0,60
2	123	0,936845	0,937857	0,936845	0,935536	0,60
3	456	0,937978	0,938920	0,937978	0,936660	0,60
4	789	0,939670	0,940233	0,939670	0,938383	0,61
5	2025	0,938899	0,939565	0,938899	0,937632	0,61

### 4.3 Hasil Pengujian Random Forest

Tabel 4.3 Hasil Evaluasi Random Forest

No	Seed	Accuracy	Precision	Recall	F1-Score	Waktu (detik)
1	42	0,939156	0,939427	0,939156	0,938017	19,67
2	123	0,936845	0,937439	0,936845	0,935622	17,21
3	456	0,938099	0,938655	0,938099	0,936898	18,48
4	789	0,939594	0,939885	0,939594	0,938363	18,37
5	2025	0,938824	0,939137	0,938824	0,937618	17,45

### 4.4 Figure
4.4 Perbandingan Performa Algoritma

Tabel 4.4 Rata-rata Hasil Pengujian

Metrik	Decision Tree	Random Forest
Accuracy	0,938677	0,938504
Precision	0,939393	0,938908
Recall	0,938677	0,938504
F1-Score	0,937411	0,937304
Waktu Eksekusi	0,683 detik	18,236 detik

### 4.5 Analisis Statistik

Untuk memastikan apakah perbedaan performa kedua algoritma bersifat signifikan, dilakukan pengujian statistik menggunakan Shapiro-Wilk Test dan Paired Sample t-test.

Hasil Shapiro-Wilk menunjukkan bahwa seluruh data berdistribusi normal sehingga memenuhi syarat untuk dilakukan uji parametrik.

Hasil Paired Sample t-test menunjukkan bahwa:

Accuracy tidak berbeda signifikan.
Recall tidak berbeda signifikan.
F1-Score tidak berbeda signifikan.
Precision memiliki perbedaan yang signifikan.

Analisis menggunakan Cohen's d menunjukkan bahwa perbedaan pada metrik Precision memiliki ukuran efek yang besar, sedangkan Accuracy dan Recall memiliki ukuran efek yang sangat kecil.

---

## 5. Kendala dan Catatan Lingkungan
Selama proses penelitian terdapat beberapa kendala, antara lain proses preprocessing yang memerlukan penyesuaian terhadap atribut dataset, kebutuhan waktu komputasi yang lebih lama pada algoritma Random Forest, serta perlunya pengujian berulang untuk memperoleh hasil yang lebih stabil. Kendala tersebut dapat diatasi dengan melakukan pembersihan data, menggunakan beberapa nilai random seed, serta menerapkan analisis statistik untuk memastikan bahwa hasil pengujian dapat dipertanggungjawabkan secara ilmiah.

---

## 6. Kesimpulan dan Saran

6.1 Kesimpulan

Penelitian ini berhasil mengimplementasikan dan membandingkan algoritma Decision Tree dan Random Forest untuk mendeteksi serangan Denial of Service (DoS) pada jaringan Internet of Things (IoT) berbasis protokol Message Queuing Telemetry Transport (MQTT). Seluruh tahapan penelitian telah dilaksanakan mulai dari proses preprocessing dataset, pembagian data latih dan data uji, pelatihan model, hingga evaluasi performa menggunakan metrik Accuracy, Precision, Recall, F1-Score, serta waktu eksekusi.

Hasil pengujian menunjukkan bahwa kedua algoritma memiliki performa klasifikasi yang sangat baik dengan tingkat akurasi di atas 93%. Decision Tree memperoleh rata-rata Accuracy sebesar 93,87%, Precision sebesar 93,94%, Recall sebesar 93,87%, dan F1-Score sebesar 93,74%. Sementara itu, Random Forest memperoleh rata-rata Accuracy sebesar 93,85%, Precision sebesar 93,89%, Recall sebesar 93,85%, dan F1-Score sebesar 93,73%.

Berdasarkan hasil Paired Sample t-test, tidak terdapat perbedaan yang signifikan pada metrik Accuracy, Recall, dan F1-Score antara Decision Tree dan Random Forest. Namun, pada metrik Precision ditemukan perbedaan yang signifikan. Selain itu, hasil analisis Cohen's d menunjukkan bahwa perbedaan pada metrik Precision memiliki ukuran efek yang besar, sedangkan pada metrik Accuracy dan Recall ukuran efeknya sangat kecil.

Dari aspek efisiensi komputasi, Decision Tree memiliki keunggulan karena mampu menyelesaikan proses pelatihan dan prediksi dalam waktu rata-rata sekitar 0,68 detik, sedangkan Random Forest memerlukan waktu rata-rata sekitar 18,24 detik. Oleh karena itu, Decision Tree lebih sesuai diterapkan pada sistem dengan keterbatasan sumber daya komputasi atau aplikasi yang membutuhkan respons cepat, sedangkan Random Forest tetap menjadi alternatif yang baik apabila stabilitas model menjadi pertimbangan utama.

Secara keseluruhan, kedua algoritma layak digunakan sebagai metode klasifikasi untuk mendeteksi serangan DoS pada jaringan IoT berbasis MQTT. Pemilihan algoritma dapat disesuaikan dengan kebutuhan implementasi, baik dari sisi performa klasifikasi maupun efisiensi waktu komputasi.

6.2 Saran

Penelitian ini masih memiliki beberapa keterbatasan yang dapat dikembangkan pada penelitian selanjutnya. Beberapa saran yang dapat diberikan antara lain sebagai berikut.

Menggunakan dataset yang lebih beragam dengan berbagai jenis serangan siber selain Denial of Service sehingga kemampuan generalisasi model dapat dievaluasi secara lebih komprehensif.
Membandingkan performa Decision Tree dan Random Forest dengan algoritma machine learning lainnya, seperti XGBoost, LightGBM, CatBoost, Support Vector Machine, maupun Neural Network.
Menerapkan teknik optimasi hiperparameter, seperti Grid Search atau Random Search, untuk memperoleh konfigurasi model yang lebih optimal.
Menggunakan metode seleksi fitur agar jumlah atribut yang digunakan menjadi lebih efisien tanpa mengurangi kemampuan klasifikasi model.
Mengembangkan sistem menjadi aplikasi deteksi serangan secara real-time sehingga dapat diterapkan langsung pada lingkungan Internet of Things.

---

## 7. Lampiran — Peta Artefak Penelitian

Lampiran A. Perangkat Penelitian
Perangkat Keras
Komponen	Spesifikasi
Prosesor	Intel Core i5 / AMD Ryzen 5 atau setara
RAM	Minimal 8 GB
Media Penyimpanan	SSD/HDD
Sistem Operasi	Windows 10/11 64-bit
Perangkat Lunak
Perangkat Lunak	Fungsi
Google Colaboratory	Lingkungan pengembangan
Python 3	Bahasa pemrograman
Pandas	Pengolahan dataset
NumPy	Operasi numerik
Scikit-learn	Implementasi algoritma Machine Learning
Matplotlib	Visualisasi data
SciPy	Pengujian statistik
Microsoft Word	Penyusunan laporan
Lampiran B. Dataset Penelitian
Komponen	Keterangan
Nama Dataset	MQTT Dataset
Jenis Data	Lalu lintas jaringan Internet of Things
Target	Normal dan DoS
Format	Microsoft Excel (.xlsx)
Jumlah Data	Sesuai dataset yang digunakan pada penelitian
Lampiran C. Tahapan Penelitian
Tahap	Kegiatan
1	Pengumpulan dataset MQTT
2	Preprocessing data
3	Label Encoding
4	Pembagian data latih dan data uji
5	Implementasi Decision Tree
6	Implementasi Random Forest
7	Evaluasi model
8	Analisis statistik
9	Penyusunan laporan penelitian
Lampiran D. Artefak Penelitian
Artefak	Keterangan
Dataset MQTT	Dataset utama penelitian
Notebook Python (.ipynb)	Implementasi seluruh eksperimen
Source Code	Program Decision Tree dan Random Forest
Hasil Evaluasi	Accuracy, Precision, Recall, F1-Score
Confusion Matrix	Visualisasi hasil klasifikasi
Hasil Uji Statistik	Shapiro-Wilk, Paired Sample t-test, Cohen's d
Artikel Jurnal	Naskah penelitian
Dokumentasi	Screenshot proses eksperimen