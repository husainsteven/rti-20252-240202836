# Proposal Penelitian

## Latar Belakang

Perkembangan Internet of Things (IoT) menyebabkan penggunaan protokol MQTT semakin luas karena ringan dan efisien. Namun MQTT rentan terhadap serangan Denial of Service (DoS) yang dapat menurunkan ketersediaan layanan. Berbagai penelitian telah menggunakan algoritma machine learning untuk mendeteksi serangan tersebut, tetapi masih memiliki kelemahan pada efisiensi komputasi dan pemilihan fitur. Oleh karena itu penelitian ini mengusulkan optimasi XGBoost menggunakan Particle Swarm Optimization (PSO) agar menghasilkan deteksi yang lebih akurat dan efisien.

## Rumusan Masalah

1. Bagaimana performa XGBoost dalam mendeteksi serangan MQTT?
2. Bagaimana pengaruh PSO terhadap peningkatan performa XGBoost?
3. Apakah XGBoost + PSO memberikan hasil yang lebih baik dibanding Random Forest dan Logistic Regression?

## Tujuan

1. Mengembangkan model deteksi serangan MQTT menggunakan XGBoost + PSO.
2. Membandingkan performa model dengan Random Forest dan Logistic Regression.
3. Mengevaluasi performa menggunakan Accuracy, Precision, Recall, F1-Score, dan Training Time.

## Urgensi Penelitian

Penelitian ini diharapkan menghasilkan model deteksi serangan IoT yang memiliki akurasi tinggi sekaligus waktu komputasi yang efisien sehingga dapat diterapkan pada perangkat IoT dengan sumber daya terbatas.