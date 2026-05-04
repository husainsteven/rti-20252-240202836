# WS-04: Research Question & Hypothesis

> **Bab 4 — Research Question, Contribution & Hypothesis**

---

## Ringkasan Materi

### RQ Bukan Pertanyaan Biasa

Research Question yang baik secara implisit mengandung cetak biru eksperimen: subjek, baseline, metrik, domain, dataset.

| Kualitas | Contoh |
|----------|--------|
| **Buruk** | "Bagaimana pengaruh deep learning terhadap deteksi malware?" |
| **Baik** | "Apakah CNN menghasilkan F1-Score lebih tinggi dari RF pada CIC-MalMem-2022?" |

Perbedaan: RQ yang baik menyebutkan **metode spesifik**, **metrik terukur**, **baseline**, dan **dataset**.

### Tiga Jenis RQ

| Jenis | Pola | Kebutuhan |
|-------|------|-----------|
| **Comparison** | A vs B → mana lebih baik? | ≥ 2 metode, metrik sama |
| **Improvement** | A' vs A → modifikasi lebih baik? | Pre/post, bukti perbaikan |
| **Exploratory** | Faktor X₁...Xₙ → pengaruh terhadap Y? | Multi-variabel, korelasi/regresi |

### Contribution Statement

Tiga jenis kontribusi: **Improvement** (metode terbukti lebih baik), **Comparison** (perbandingan sistematis yang belum ada), **Novel Approach** (pendekatan baru). Kontribusi harus terhubung langsung dengan gap — kontribusi tanpa gap = klaim tanpa justifikasi.

### Hypothesis H₀ / H₁

- **H₀** (Null) = Tidak ada perbedaan signifikan — asumsi default, harus dibuktikan salah
- **H₁** (Alternative) = Ada perbedaan signifikan — diterima hanya jika H₀ ditolak
- Harus **falsifiable**, mengandung **metrik terukur**, dirumuskan **SEBELUM eksperimen**

### Rantai Operasionalisasi

```
RQ → Variable → Metric → Data → Analysis
```

Jika rantai ini tidak lengkap, RQ belum mature. Bi-directional: RQ yang tidak bisa jadi hipotesis testable harus direvisi mundur.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan pertanyaan | Apa yang harus dibangun? | Apa yang harus dibuktikan? |
| Bentuk jawaban | Sistem yang berfungsi | Bukti empiris terukur |
| Sukses diukur oleh | User satisfaction, uptime | Signifikansi statistik, effect size |
| Jika gagal | Debug dan perbaiki | Laporkan, analisis mengapa |

### Istilah Penting

- **Research Question (RQ)** — Pertanyaan spesifik: variabel terukur + metrik + konteks
- **Contribution Statement** — Apa yang diketahui setelah riset selesai yang sebelumnya belum ada
- **H₀ / H₁** — Null vs Alternative Hypothesis
- **Falsifiability** — Kondisi hipotesis ditolak harus bisa didefinisikan sebelum eksperimen
- **Operationalization** — Proses mewujudkan konsep abstrak menjadi variabel terukur

---

## Template A.4 — RQ-Contribution-Hypothesis

```
RQ-CONTRIBUTION-HYPOTHESIS

Gap Statement  : Belum ada studi yang membuktikan bahwa algoritma XGBoost dengan seleksi fitur berbasis PSO dapat menghasilkan deteksi serangan DoS pada trafik MQTT yang sekaligus akurat (F1-Score tinggi per kelas serangan) dan efisien secara komputasi (waktu inferensi rendah) dibandingkan metode baseline yang umum digunakan, khususnya menggunakan dataset simulasi trafik MQTT lokal yang lebih representatif.____________________

Research Question:
  Tipe         : [ X ] Comparison  [ ] Improvement  [ ] Exploratory
  Formulasi    : Apakah algoritma XGBoost dengan seleksi fitur berbasis PSO menghasilkan akurasi, F1-Score per kelas, dan waktu inferensi yang lebih baik dibandingkan Logistic Regression+RFE dan Random Forest tanpa seleksi fitur dalam mendeteksi serangan DoS pada dataset trafik MQTT hasil simulasi jaringan IoT lokal?____________________
  Variabel IV  : Algoritma klasifikasi: XGBoost+PSO (metode utama), Logistic Regression+RFE (baseline 1), Random Forest tanpa seleksi fitur (baseline 2)____________________
  Variabel DV  : Akurasi, F1-Score per kelas (Normal, SYN Flood, MQTT Attack), dan waktu inferensi per fold____________________
  Metrik       : Accuracy (%), F1-Score (%), Waktu inferensi (detik per fold)____________________
  Dataset      : Dataset trafik MQTT simulasi jaringan IoT lokal: 1.054.817 rekaman (Normal, SYN Flood, MQTT Attack) — direkam menggunakan Wireshark____________________
  Baseline     : Logistic Regression + RFE (Primadya et al., 2024) dan Random Forest tanpa seleksi fitur (Akbar, 2023)____________________

Quality Check RQ:
  [ X ] Variabel spesifik
  [ X ] Metrik jelas
  [ X ] Baseline ada
  [ X ] Konteks disebutkan
  [ X ] Memerlukan eksperimen (bukan hanya survei literatur)

Contribution Statement:
  Apa yang baru diketahui : Apakah kombinasi XGBoost dan seleksi fitur PSO dapat menjadi solusi yang menyeimbangkan akurasi tinggi dan efisiensi komputasi rendah untuk deteksi DoS pada trafik MQTT IoT — sesuatu yang belum dibuktikan secara sistematis oleh studi sebelumnya.____________________
  Jenis kontribusi        : [ ] Improvement  [ X ] Comparison  [ ] Novel approach
  Gap yang diisi          : Performance Gap (trade-off akurasi vs efisiensi belum terpecahkan) dan Data Gap (dataset simulasi MQTT lokal dengan variasi serangan yang lebih representatif)____________________

Hypothesis Pair:
  H₀ : Tidak ada perbedaan yang signifikan secara statistik antara F1-Score dan waktu inferensi yang dihasilkan oleh XGBoost+PSO dibandingkan Logistic Regression+RFE dan Random Forest tanpa seleksi fitur dalam mendeteksi serangan DoS pada dataset trafik MQTT lokal.____________________
  H₁ : XGBoost+PSO menghasilkan F1-Score yang lebih tinggi dan waktu inferensi yang lebih rendah secara signifikan secara statistik dibandingkan Logistic Regression+RFE dan Random Forest tanpa seleksi fitur dalam mendeteksi serangan DoS pada dataset trafik MQTT lokal.____________________
  Threshold              : p-value < 0,05 (uji Wilcoxon Signed-Rank, non-parametrik) dan Cohen's d > 0,5 (effect size medium)____________________
  Justifikasi threshold  : p < 0,05 adalah standar yang paling umum diterima dalam penelitian informatika. Uji Wilcoxon dipilih karena data hasil K-Fold tidak dapat diasumsikan berdistribusi normal. Cohen's d > 0,5 memastikan perbedaan yang ditemukan tidak hanya signifikan secara statistik tapi juga bermakna secara praktis untuk implementasi nyata di perangkat IoT.____________________
```

---

## Latihan 1 — Dari Gap ke RQ

Gunakan gap yang ditemukan di WS-03. Transformasikan menjadi Research Question.

**Gap dari WS-03:** Performance Gap + Data Gap: belum ada metode yang terbukti sekaligus akurat dan efisien untuk deteksi DoS pada trafik MQTT lokal dengan variasi serangan yang memadai.____________________________________

**RQ versi pertama (tulis bebas):**
> Apakah XGBoost dengan PSO lebih baik dari metode lain untuk deteksi serangan pada jaringan IoT yang pakai MQTT?___________________________________________________

**Evaluasi RQ:**

| Komponen | Ada? | Isi |
|----------|------|-----|
| Metode spesifik | Ada | XGBoost + PSO disebutkan, tapi baseline belum spesifik |
| Metrik terukur | Belum | Kata "lebih baik" terlalu umum, tidak ada metrik konkret |
| Baseline | Belum | "Metode lain" tidak spesifik, tidak bisa dibandingkan secara fair |
| Dataset/konteks | Belum | Tidak disebutkan dataset apa yang digunakan |

**Tipe RQ:** [ X ] Comparison / [ ] Improvement / [ ] Exploratory

**RQ versi revisi (setelah evaluasi):**
> Apakah algoritma XGBoost dengan seleksi fitur berbasis PSO menghasilkan akurasi, F1-Score per kelas, dan waktu inferensi yang lebih baik secara statistik dibandingkan Logistic Regression+RFE dan Random Forest tanpa seleksi fitur, dalam mendeteksi serangan DoS pada dataset trafik MQTT hasil simulasi jaringan IoT lokal menggunakan Stratified K-Fold Cross Validation dengan k=10?___________________________________________________

---

## Latihan 2 — Hypothesis Pair

Rumuskan pasangan hipotesis dari RQ di Latihan 1.

| Komponen | Isi |
|----------|-----|
| H₀ | Tidak ada perbedaan signifikan (p > 0,05) antara F1-Score dan waktu inferensi XGBoost+PSO dibandingkan Logistic Regression+RFE dan Random Forest dalam deteksi DoS pada dataset MQTT lokal |
| H₁ | XGBoost+PSO menghasilkan F1-Score lebih tinggi dan waktu inferensi lebih rendah secara signifikan (p < 0,05) dibandingkan kedua baseline pada dataset MQTT lokal yang sama |
| Metrik | F1-Score per kelas (primary), Accuracy dan waktu inferensi per fold (secondary) |
| Threshold | p-value < 0,05 dan Cohen's d > 0,5 |
| Justifikasi threshold | p < 0,05 adalah konvensi standar yang diakui luas. Cohen's d > 0,5 memastikan perbedaan bermakna secara praktis, bukan hanya karena sampel besar. Uji Wilcoxon Signed-Rank dipilih karena data K-Fold tidak dapat diasumsikan normal. |

**Apakah hipotesis ini falsifiable?** [ X ] Ya / [ ] Tidak
> Bagaimana cara membuktikannya salah? Hipotesis ini bisa dibuktikan salah jika: hasil uji Wilcoxon menunjukkan p > 0,05 (tidak signifikan), atau jika F1-Score XGBoost+PSO tidak lebih tinggi dari salah satu baseline, atau jika waktu inferensinya justru lebih lambat. Kondisi penolakan H₁ sudah didefinisikan sejak awal sebelum eksperimen dijalankan.___________________

---

## Latihan 3 — Rantai Operasionalisasi

Lengkapi rantai dari RQ hingga metode analisis.

| Tahap | Isi |
|-------|-----|
| RQ | Apakah XGBoost+PSO menghasilkan F1-Score lebih tinggi dan waktu inferensi lebih rendah dibanding baseline pada dataset MQTT lokal? |
| Variable (IV) | Jenis algoritma klasifikasi: XGBoost+PSO vs Logistic Regression+RFE vs Random Forest|
| Variable (DV) | Akurasi deteksi, F1-Score per kelas serangan, dan waktu inferensi per fold yang dihasilkan masing-masing algoritma |
| Metric | Accuracy (%), F1-Score per kelas (%), Waktu inferensi (detik/fold) — diukur dari confusion matrix dan timer sistem |
| Data source | Dataset MQTT lokal 1.054.817 rekaman — dibagi menggunakan Stratified K-Fold k=10 agar distribusi kelas terjaga di setiap fold |
| Analysis method | Uji Wilcoxon Signed-Rank untuk signifikansi statistik, Cohen's d untuk effect size, tabel perbandingan metrik antar kondisi |

**Apakah rantai lengkap?** [ X ] Ya / [ ] Tidak
> Jika tidak, tahap mana yang perlu direvisi? Setiap tahap terhubung tanpa lompatan logis. RQ mengarah ke variabel yang jelas, variabel mengarah ke metrik yang terukur, metrik bisa dikumpulkan dari data yang sudah tersedia, dan metode analisis sudah dipilih sesuai tipe data (ratio scale untuk semua metrik DV).______________

---

## Refleksi

> Ambil satu judul skripsi/paper yang pernah dibaca. Coba ekstrak RQ-nya. Apakah RQ tersebut memenuhi semua komponen (metode, metrik, baseline, konteks)? Jika tidak, apa yang hilang?

**Judul:** Studi Perbandingan Deteksi Intrusi Jaringan Menggunakan Machine Learning: Metode SVM dan ANN — Tan et al. (2023), JATI UNIKOM_____________________________________________
**RQ yang diekstrak:** "Metode mana yang lebih baik antara SVM dan ANN untuk deteksi intrusi jaringan?"__________________________________
**Komponen yang hilang:** RQ paper itu tidak menyebutkan metrik secara spesifik di bagian rumusan masalahnya — kata "lebih baik" tidak langsung dioperasionalisasi menjadi metrik seperti F1-Score atau waktu inferensi. Selain itu, dataset yang digunakan (KDD Cup 99) tidak disebutkan dalam RQ-nya sehingga konteks eksperimennya tidak terlihat dari pertanyaan penelitian. Ini yang membuat klaim "99,87% akurat" sulit diverifikasi tanpa membaca metodologi secara lengkap terlebih dahulu._______________________________
