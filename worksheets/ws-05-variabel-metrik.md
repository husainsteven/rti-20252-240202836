# WS-05: Variabel & Metrik

> **Bab 5 — Metric, Measurement & Data**

---

## Ringkasan Materi

### Measurement Alignment Model

Setiap pengukuran yang valid harus bisa ditelusuri melalui rantai ini tanpa lompatan logis:

```
Problem → Concept → Variable → Metric → Data → Result
```

### Operationalization = Keputusan Desain

Menerjemahkan konsep abstrak menjadi variabel terukur bukan proses mekanis. "Code quality" yang diukur via SonarQube code smells membawa asumsi implisit. Setiap operasionalisasi harus didokumentasikan dan dijustifikasi.

### Empat Tipe Data (NOIR)

| Tipe | Ciri | Contoh | Operasi Valid |
|------|------|--------|---------------|
| **Nominal** | Kategori, tanpa urutan | Jenis algoritma (RF, SVM, CNN) | Modus, chi-square |
| **Ordinal** | Urutan, interval tidak sama | Skala Likert (1-5) | Median, Spearman |
| **Interval** | Jarak bermakna, tanpa nol absolut | Suhu Celsius | Mean, Pearson, t-test |
| **Ratio** | Jarak bermakna + nol absolut | Waktu eksekusi (ms) | Semua operasi |

Tipe data menentukan uji statistik yang valid. Kebanyakan metrik performa TI = ratio; persepsi pengguna = ordinal.

### Kriteria Pemilihan Metrik

- **Representative** — Mewakili konsep yang diteliti
- **Sensitive** — Cukup peka menangkap perbedaan bermakna (hindari ceiling effect)
- **Feasible** — Bisa dikumpulkan dalam batasan waktu dan biaya

### Pre-registration

Metrik harus ditentukan **sebelum** eksperimen. Memilih metrik setelah melihat data = **p-hacking**. Metrik tambahan yang ditemukan kemudian dilaporkan sebagai *exploratory*, bukan *confirmatory*.

### Primary vs Secondary Metric

- **Primary Metric** — Langsung terikat ke hipotesis, menentukan kesimpulan
- **Secondary Metric** — Pendukung, dilaporkan di samping primary; statusnya suplementer

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Pemilihan metrik | Berdasarkan kebiasaan/tool yang ada | Berdasarkan construct validity |
| Anomali | Dihapus untuk laporan bersih | Diinvestigasi — bisa jadi temuan |
| Kapan dipilih | Setelah sistem jadi (monitoring) | Sebelum eksperimen (by design) |

### Istilah Penting

- **Operationalization** — Transformasi konsep abstrak menjadi variabel terukur
- **Construct Validity** — Sejauh mana pengukuran benar-benar mengukur konsep yang dimaksud
- **Measurement Scale** — Klasifikasi data (NOIR) yang menentukan analisis valid
- **Multi-metric Evaluation** — Menggunakan beberapa metrik untuk menangkap konsep kompleks

---

## Template A.5 — Definisi Variabel, Metrik & Justifikasi

```
VARIABLE & METRIC DEFINITION

Research Question: Apakah XGBoost+PSO menghasilkan akurasi, F1-Score per kelas, dan waktu inferensi yang lebih baik secara statistik dibandingkan Logistic Regression+RFE dan Random Forest dalam mendeteksi serangan DoS pada dataset MQTT lokal?____________________

| Variabel | Tipe | Konsep | Metrik | Skala | Satuan | Cara Mengukur | Justifikasi |
|----------|------|--------|--------|-------|--------|---------------|-------------|
| Jenis algoritma klasifikasi         | IV   | Pendekatan machine learning yang digunakan untuk mengklasifikasikan trafik MQTT       | Kategorikal: XGBoost+PSO / LR+RFE / RF       | Nominal      | —       | Di-toggle via config.yaml — hanya satu baris yang berubah antar kondisi eksperimen               | IV harus nominal karena merupakan kategori diskrit tanpa urutan numerik yang bermakna. Pemilihan algoritma inilah yang menjadi faktor pembeda antar kondisi eksperimen            |
| Akurasi deteksi keseluruhan         | DV   | Proporsi prediksi yang benar dari seluruh prediksi yang dibuat model       | Accuracy = (TP+TN)/(TP+TN+FP+FN)        | Ratio       | % (0–100)       | Dihitung otomatis dari confusion matrix setiap fold oleh sklearn              | Dipilih sebagai metrik pendukung karena memberikan gambaran umum performa model. Namun tidak digunakan sebagai primary metric karena bisa menyesatkan pada dataset yang tidak seimbang seperti dataset ini            |
| Dataset yang digunakan         | CV   | Sumber data trafik MQTT yang digunakan untuk melatih dan menguji semua algoritma       | Dataset MQTT lokal 1.054.817 rekaman (fixed, sama untuk semua kondisi)       | Nominal      | —       | Dikunci di config.yaml — tidak berubah antar kondisi eksperimen              | Dikontrol agar perbedaan hasil antar algoritma murni karena perbedaan algoritmanya, bukan karena datanya berbeda. Ini adalah syarat dasar perbandingan yang adil sesuai prinsip Variable Isolation di WS-06            |

Alignment Check:
  RQ → Concept → Variable → Metric → Data → Result
  [ X ] Setiap langkah terdokumentasi
  [ X ] Tidak ada "lompatan logis"
  [ X ] Metrik mengukur apa yang dimaksud (construct validity)
```

---

## Latihan 1 — Operationalization Chain

Gunakan RQ dari WS-04. Definisikan variabel dan metriknya.

**RQ:** Apakah XGBoost+PSO menghasilkan akurasi, F1-Score, dan waktu inferensi yang lebih baik dibanding Logistic Regression+RFE dan Random Forest dalam mendeteksi DoS pada dataset MQTT lokal?__________________________________________________

| Variabel | Tipe | Konsep Abstrak | Metrik Konkret | Skala (NOIR) | Satuan |
|----------|------|---------------|----------------|-------------|--------|
| Jenis algoritma | IV | Pendekatan klasifikasi yang digunakan | Kategorikal: XGBoost+PSO / LR+RFE / RF | Kategorikal: XGBoost+PSO / LR+RFE / RF | — |
| Akurasi deteksi | DV | Ketepatan klasifikasi keseluruhan | Accuracy = (TP+TN)/(Total) | Ratio | % |
| Dataset | CV | Sumber data yang dikontrol | Dataset MQTT lokal 1.054.817 rekaman (fixed) | Nominal | — |

**Apakah ada lompatan logis dalam rantai?** [ ] Ya / [ X ] Tidak
> Jika ya, di mana?  Setiap konsep abstrak sudah diterjemahkan ke metrik konkret yang bisa langsung dikumpulkan dari eksperimen. Tidak ada konsep yang tersisa sebagai asumsi implisit.____________________________________

---

## Latihan 2 — Evaluasi Metrik

Evaluasi metrik DV yang dipilih di Latihan 1 menggunakan 3 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Representative | 5 | F1-Score per kelas langsung mengukur kemampuan deteksi serangan pada data imbalanced — jauh lebih representatif dari accuracy saja karena mempertimbangkan false negative pada kelas minoritas |
| Sensitive | 4 | F1-Score cukup sensitif menangkap perbedaan performa antar model, terutama pada kelas MQTT Attack yang minoritas. Skor 4 bukan 5 karena pada kelas mayoritas (Normal) perbedaan antar model cenderung kecil dan bisa terkena ceiling effect |
| Feasible | 5 | F1-Score dihitung otomatis oleh sklearn classification_report setiap fold. Tidak memerlukan alat tambahan, tidak ada biaya akuisisi data |

**Apakah perlu secondary metric?** [ X ] Ya / [ ] Tidak
> Jika ya, apa dan mengapa? Waktu inferensi per fold dijadikan secondary metric karena langsung menjawab pertanyaan praktis: apakah model cukup ringan untuk IoT? Tanpa metrik ini, penelitian hanya menjawab "mana yang lebih akurat" — padahal root cause dari WS-02 justru pada keterbatasan sumber daya perangkat IoT, bukan sekadar akurasi._____________________________

**Contoh kasus ceiling effect untuk metrik ini:**
> Pada kelas Normal yang sangat mayoritas (jumlah sampel jauh lebih besar), hampir semua model akan menghasilkan F1-Score mendekati 100% sehingga perbedaan antar model tidak terlihat di kelas ini. Itulah mengapa F1-Score dilaporkan per kelas, bukan hanya macro average — agar perbedaan yang nyata pada kelas minoritas (SYN Flood dan MQTT Attack) tetap terlihat dan tidak tertutup oleh skor kelas mayoritas.___________________________________________________

---

## Latihan 3 — Data Quality Check

Bayangkan data yang akan dikumpulkan dari eksperimen. Evaluasi 4 dimensi kualitas data.

| Dimensi | Pertanyaan | Jawaban | Strategi Mitigasi |
|---------|-----------|---------|------------------|
| Completeness | Apakah semua data point terkumpul? | Dataset sudah lengkap — 1.054.817 rekaman dari simulasi Wireshark, tidak ada nilai kosong (missing value) karena data dihasilkan dari capture langsung | Lakukan pengecekan df.isnull().sum() sebelum eksperimen dimulai. Jika ada missing value, terapkan imputation atau hapus baris yang bermasalah dengan dokumentasi yang jelas |
| Consistency | Apakah ada kontradiksi internal? | Ada potensi inkonsistensi label — paket yang sama bisa terlabel berbeda jika proses capture tidak konsisten. Distribusi kelas yang sangat tidak seimbang (MQTT Attack 1.040.417 vs Normal 7.200) juga berpotensi menyebabkan bias | Verifikasi distribusi kelas dengan value_counts(). Gunakan Stratified K-Fold agar setiap fold memiliki proporsi kelas yang sama dengan dataset keseluruhan |
| Validity | Apakah benar-benar mengukur yang dimaksud? | Dataset berasal dari simulasi jaringan MQTT lokal yang direkam menggunakan Wireshark — lebih representatif dari dataset publik luar negeri. Namun tetap terbatas pada 2 jenis serangan saja | Dokumentasikan kondisi simulasi secara lengkap (topologi jaringan, tool yang digunakan, durasi capture). Akui keterbatasan ini sebagai limitasi penelitian di bagian Discussion |
| Representativeness | Apakah sampel mewakili populasi target? | Dataset hanya mencakup 2 jenis serangan DoS (SYN Flood dan MQTT Attack) dari satu lingkungan simulasi lokal. Belum mewakili seluruh variasi serangan DoS yang mungkin terjadi di jaringan IoT nyata yang lebih beragam | Akui sebagai Data Gap yang sudah diidentifikasi di WS-03. Sarankan pengembangan ke dataset lebih luas sebagai future work. Hindari overgeneralisasi kesimpulan ke luar konteks MQTT lokal. |

---

## Refleksi

> Mengapa memilih metrik setelah melihat data dianggap p-hacking? Apa bedanya dengan eksplorasi data yang sah?

**Jawaban:**
> Memilih metrik setelah melihat data adalah p-hacking karena peneliti bisa secara tidak sadar — atau bahkan sengaja — memilih metrik yang kebetulan menghasilkan hasil yang signifikan. Misalnya, kalau accuracy tidak signifikan, lalu ganti ke F1-Score, lalu ke AUC sampai ketemu yang "bagus". Ini melanggar prinsip falsifiability karena kondisi penolakan hipotesis tidak ditentukan sebelum eksperimen — melainkan dicari-cari setelahnya.___________________________________________________
> Bedanya dengan eksplorasi data yang sah adalah soal niat dan transparansi. Eksplorasi data (exploratory analysis) sah dilakukan asalkan hasilnya dilabeli secara jelas sebagai "eksploratori" dan tidak diklaim sebagai bukti konfirmatori. Misalnya: "Kami juga mengamati bahwa model X menunjukkan pola menarik pada metrik Y — ini akan menjadi hipotesis untuk penelitian selanjutnya." Klaim seperti itu tidak menipu karena statusnya transparan. Yang tidak boleh adalah mengeksplorasi berbagai metrik lalu melaporkan hanya yang signifikan seolah-olah itu memang metrik yang direncanakan sejak awal.___________________________________________________
