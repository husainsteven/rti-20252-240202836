# WS-07: Experimental Design & Validity

> **Bab 7 — Experimental Design & Validity**

---

## Ringkasan Materi

### Correlation ≠ Causality

Kausalitas membutuhkan 3 syarat:
1. **Covariance** — X dan Y bergerak bersama
2. **Temporal precedence** — X berubah sebelum Y
3. **Elimination of alternatives** — Tidak ada faktor lain yang menjelaskan Y

Controlled experiment adalah satu-satunya metode yang bisa membuktikan kausalitas.

### Empat Jenis Validitas

| Jenis | Pertanyaan | Ancaman Umum |
|-------|-----------|-------------|
| **Internal** | Apakah hubungan IV→DV nyata? | Confounding variable, selection bias |
| **External** | Apakah bisa digeneralisasi? | Dataset terlalu spesifik |
| **Construct** | Apakah mengukur konsep yang benar? | Metrik tidak sesuai |
| **Conclusion** | Apakah kesimpulan statistik valid? | Sample size kecil, uji salah |

Internal dan external validity sering berkonflik: semakin terkontrol (internal kuat) → semakin artificial (external lemah).

### Tiga Tipe Eksperimen dalam Riset TI

| Tipe | Deskripsi | Kapan Digunakan |
|------|----------|----------------|
| **Comparison Study** | Metode A vs B pada kondisi identik | Membandingkan pendekatan berbeda |
| **Ablation Study** | Full system → lepas komponen satu per satu | Mengukur kontribusi tiap komponen |
| **Parameter Study** | Variasikan satu parameter, amati dampak | Uji sensitifitas/robustness |

### Fairness dalam Perbandingan

Perbandingan yang adil = **kondisi identik** untuk semua metode: dataset sama, preprocessing sama, tuning effort sebanding, environment sama, metrik sama.

Contoh tidak adil: Transformer (30 fitur tambahan + Bayesian optimization) vs RF (default params) → hasilnya misleading.

### Threats to Validity = Diidentifikasi Sebelum Eksperimen

Ancaman validitas harus diidentifikasi **sebelum** eksperimen dan mitigasinya dirancang sebagai bagian dari desain — bukan ditulis sebagai boilerplate setelah selesai.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan testing | Memastikan sistem memenuhi requirement | Membuktikan hubungan kausal antar variabel |
| Baseline | Versi sebelumnya (last release) | Metode tervalidasi dari literatur |
| Kegagalan | Bug → fix → release | H₀ tidak ditolak → tetap kontribusi ilmiah |
| Sukses | 100% test pass | Evidence valid — mendukung atau menolak hipotesis |

### Istilah Penting

- **Causality** — Hubungan sebab-akibat (covariance + temporal + elimination)
- **Controlled Experiment** — Ubah satu variabel, kontrol sisanya, amati efek
- **Fairness** — Semua metode diuji pada kondisi yang benar-benar identik
- **Threats to Validity** — Faktor yang bisa melemahkan kesimpulan jika tidak dimitigasi
- **Conclusion Validity** — Validitas statistik: power, sample size, uji yang tepat

---

## Template A.7 — Desain Eksperimen Lengkap

```
EXPERIMENT DESIGN

Research Question : Apakah algoritma XGBoost dengan seleksi fitur berbasis PSO menghasilkan akurasi, F1-Score per kelas, dan waktu inferensi yang lebih baik secara statistik dibandingkan Logistic Regression+RFE dan Random Forest tanpa seleksi fitur dalam mendeteksi serangan DoS pada dataset trafik MQTT hasil simulasi jaringan IoT lokal?____________________
Hypothesis        : H₀: Tidak ada perbedaan signifikan (p > 0,05) antara F1-Score dan waktu inferensi XGBoost+PSO dibandingkan Logistic Regression+RFE dan Random Forest dalam deteksi DoS pada dataset MQTT lokal.

H₁: XGBoost+PSO menghasilkan F1-Score lebih tinggi dan waktu inferensi lebih rendah secara signifikan (p < 0,05) dibandingkan kedua baseline pada dataset MQTT lokal yang sama.____________________
Tipe Eksperimen   : [ x ] Comparison  [ x ] Ablation  [ ] Parameter

Kondisi Eksperimen:
| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control |Logistic Regression dengan seleksi fitur RFE — mewakili pendekatan paling ringan yang umum dipakai di literatur deteksi intrusi IoT (Primadya et al., 2024)           |LR + RFE          |Dataset MQTT lokal 1.054.817 rekaman; Stratified K-Fold k=10, random_state=42; MinMaxScaler + Label Encoding; mesin yang sama             |
| Control |Random Forest tanpa seleksi fitur — mewakili state-of-the-practice yang paling banyak direplikasi di literatur deteksi intrusi IoT berbasis MQTT (Akbar, 2023)           |Random Forest          |Dataset MQTT lokal 1.054.817 rekaman; Stratified K-Fold k=10, random_state=42; MinMaxScaler + Label Encoding; mesin yang sama             |
| Treatment |XGBoost dengan seleksi fitur PSO — metode yang diusulkan untuk menjawab gap Performance Gap dan Data Gap yang ditemukan di WS-03. PSO memilih 11 fitur paling relevan sebelum XGBoost dilatih         |XGBoost + PSO          |Dataset MQTT lokal 1.054.817 rekaman; Stratified K-Fold k=10, random_state=42; MinMaxScaler + Label Encoding; mesin yang sama             |

Fairness Checklist:
  [ ✓ ] Dataset identik untuk semua kondisi
  [ ✓ ] Preprocessing setara
  [ ✓ ] Tuning effort setara
  [ ✓ ] Environment identik
  [ ✓ ] Metrik evaluasi sama

Threat Analysis:
| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal    |Data leakage antara data latih dan data uji — PSO yang dijalankan sebelum pembagian fold bisa "melihat" informasi dari data uji sehingga hasil akurasi menjadi over-optimistic dan tidak mencerminkan performa nyata                 |PSO dijalankan hanya di dalam setiap fold, khusus pada data latih fold tersebut. Data uji tidak pernah menyentuh proses seleksi fitur. Ini diverifikasi dengan memeriksa tidak ada overlap indeks antara data latih dan data uji setiap fold          |
| External    |Dataset hanya mencakup 2 jenis serangan DoS (SYN Flood dan MQTT Attack) dari satu lingkungan simulasi lokal — hasil tidak bisa langsung digeneralisasi ke seluruh variasi serangan DoS yang mungkin terjadi di jaringan IoT nyata yang lebih beragam                 |Akui secara eksplisit sebagai limitasi di bagian Discussion. Hindari klaim generalisasi yang terlalu luas. Sarankan pengujian pada dataset yang lebih beragam sebagai future work. Ini sudah diidentifikasi sejak WS-03 sebagai Data Gap          |
| Construct   |Accuracy sebagai metrik utama bisa menyesatkan pada dataset yang sangat tidak seimbang — kelas MQTT Attack berjumlah 1.040.417 sementara kelas Normal hanya 7.200. Model yang selalu memprediksi MQTT Attack pun bisa mendapat accuracy tinggi                 |F1-Score per kelas dijadikan primary metric, bukan accuracy. Accuracy hanya dilaporkan sebagai secondary metric. Confusion matrix dilaporkan lengkap per fold agar tidak ada informasi yang tersembunyi di balik angka agregat          |
| Conclusion  |Jumlah fold yang terbatas (k=10) menghasilkan hanya 10 nilai per metrik per algoritma — bisa jadi tidak cukup untuk memenuhi asumsi normalitas sehingga uji parametrik seperti t-test tidak valid digunakan                 |Gunakan uji Wilcoxon Signed-Rank yang non-parametrik dan tidak mengasumsikan distribusi normal. Tambahkan Cohen's d sebagai ukuran effect size agar signifikansi statistik tidak satu-satunya penentu kesimpulan          |

Statistical Plan:
  Uji statistik   : Wilcoxon Signed-Rank Test (non-parametrik, berpasangan) untuk membandingkan distribusi F1-Score dan waktu inferensi antar kondisi____________________
  Justifikasi      :  Data hasil K-Fold (10 nilai per metrik) tidak dapat diasumsikan berdistribusi normal karena jumlah sampelnya terlalu kecil untuk diverifikasi normalitasnya. Wilcoxon dipilih karena robust terhadap distribusi non-normal dan cocok untuk perbandingan berpasangan (fold yang sama digunakan semua algoritma)
____________________
  Alpha            : α = 0,05 — threshold standar yang paling umum diterima dalam penelitian informatika____________________
  Effect size min  : Cohen's d > 0,5 (medium effect) — memastikan perbedaan yang ditemukan tidak hanya signifikan secara statistik tapi juga bermakna secara praktis untuk implementasi nyata di perangkat IoT____________________
```

---

## Latihan 1 — Desain Eksperimen

Susun desain eksperimen berdasarkan RQ, variabel, dan sistem dari WS-04 sampai WS-06.

**RQ:** Apakah XGBoost+PSO menghasilkan akurasi, F1-Score per kelas, dan waktu inferensi yang lebih baik dibanding Logistic Regression+RFE dan Random Forest dalam mendeteksi DoS pada dataset MQTT lokal?__________________________________________________
**Tipe eksperimen:** [ x ] Comparison / [ ] Ablation / [ ] Parameter

| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control | Logistic Regression + RFE sebagai baseline ringan yang mewakili lower bound perbandingan |LR + RFE | Dataset MQTT lokal; Stratified K-Fold k=10, random_state=42; MinMaxScaler; mesin sama |
| Control | Random Forest tanpa seleksi fitur sebagai baseline standar yang paling banyak dipakai di literatur deteksi intrusi IoT | Random Forest | Dataset MQTT lokal; Stratified K-Fold k=10, random_state=42; MinMaxScaler; mesin sama |
| Treatment |XGBoost + PSO sebagai metode utama yang diusulkan untuk menjawab gap yang ditemukan di WS-03 |XGBoost + PSO |Dataset MQTT lokal; Stratified K-Fold k=10, random_state=42; MinMaxScaler; mesin sama; 11 fitur dipilih PSO |

---

## Latihan 2 — Fairness Checklist

Evaluasi apakah desain eksperimen di Latihan 1 sudah fair.

| Kriteria | Status | Detail |
|----------|--------|--------|
| Dataset identik | ✅  |Semua algoritma menggunakan dataset MQTT lokal yang sama persis — 1.054.817 rekaman dengan distribusi kelas yang identik (Normal: 7.200, SYN Flood: 7.200, MQTT Attack: 1.040.417). Path dataset dikunci di config.yaml |
| Preprocessing setara | ✅  |MinMaxScaler dan Label Encoding dijalankan sekali sebelum eksperimen, hasilnya disimpan sebagai file terpisah dan dibaca oleh semua algoritma. Tidak ada algoritma yang menerima data raw sementara yang lain menerima data yang sudah dinormalisasi |
| Tuning effort setara | ✅  |Semua algoritma menggunakan parameter default sklearn. PSO untuk XGBoost bukan hyperparameter tuning melainkan bagian dari metode yang memang sedang diuji. LR dan RF tidak mendapat grid search tambahan yang tidak diberikan ke XGBoost+PSO |
| Environment identik | ✅  |Semua eksperimen dijalankan di mesin yang sama secara berurutan — tidak ada eksperimen yang dijalankan di cloud sementara yang lain di lokal. Spesifikasi hardware dicatat otomatis di header results.csv |
| Metrik evaluasi sama | ✅  |Accuracy, F1-Score per kelas, dan waktu inferensi diukur dengan cara identik untuk semua algoritma menggunakan sklearn classification_report() dan time.perf_counter(). Tidak ada metrik yang hanya diukur untuk satu algoritma saja |

**Ada yang tidak fair?** [ ] Ya / [ ✓ ] Tidak
> Jika ya, bagaimana cara memperbaikinya? Kelima kriteria fairness terpenuhi. Desain eksperimen ini memastikan bahwa perbedaan hasil yang ditemukan nanti murni karena perbedaan algoritma, bukan karena perbedaan kondisi eksperimen. Ini yang memungkinkan klaim kausalitas — bahwa XGBoost+PSO lebih baik karena algoritmanya, bukan karena kondisi yang menguntungkannya.________________

---

## Latihan 3 — Threat Analysis

Identifikasi ancaman validitas untuk desain eksperimen ini.

| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal | Data leakage PSO — jika PSO dijalankan pada seluruh dataset sebelum pembagian fold, informasi dari data uji ikut mempengaruhi seleksi fitur sehingga hasil akurasi over-optimistic dan tidak mencerminkan performa pada data yang benar-benar belum pernah dilihat model | PSO dijalankan hanya di dalam setiap fold pada data latih fold tersebut saja. Setiap fold menghasilkan subset fitur yang mungkin berbeda. Data uji tidak pernah menyentuh proses seleksi fitur. Diverifikasi dengan memeriksa tidak ada overlap indeks antar set |
| External | Generalisasi terbatas — dataset hanya mencakup 2 jenis serangan DoS dari satu lingkungan simulasi lokal. Model yang baik di sini belum tentu baik untuk jenis serangan lain (Slowloris, UDP Flood, HTTP Flood) atau pada topologi jaringan IoT yang berbeda | Batasi klaim kesimpulan hanya pada konteks dataset ini. Nyatakan secara eksplisit di bagian Discussion bahwa hasil berlaku untuk kondisi serangan SYN Flood dan MQTT Attack pada jaringan MQTT lokal. Sarankan replikasi pada dataset yang lebih beragam sebagai future work |
| Construct | Accuracy menyesatkan pada data imbalanced — kelas MQTT Attack mendominasi dataset (1.040.417 dari 1.054.817 rekaman). Model yang selalu memprediksi MQTT Attack bisa mendapat accuracy 98% tanpa benar-benar bisa membedakan kelas Normal dan SYN Flood | F1-Score per kelas dijadikan primary metric. Accuracy hanya dilaporkan sebagai pelengkap. Confusion matrix lengkap dilaporkan per kondisi agar tidak ada informasi yang tersembunyi. Stratified K-Fold memastikan distribusi kelas terjaga di setiap fold |
| Conclusion | Sample size terbatas — K-Fold k=10 hanya menghasilkan 10 nilai per metrik per algoritma. Jumlah ini terlalu kecil untuk memverifikasi normalitas distribusi sehingga uji parametrik seperti t-test tidak valid. Risiko Type I error (menyimpulkan ada perbedaan padahal tidak ada) juga meningkat jika alpha tidak dikontrol dengan ketat | Gunakan uji Wilcoxon Signed-Rank yang non-parametrik dan tidak mengasumsikan normalitas. Tetapkan alpha = 0,05 sebelum eksperimen dimulai. Tambahkan Cohen's d untuk memastikan perbedaan bermakna secara praktis, bukan hanya statistik. Laporkan interval kepercayaan 95% |

**Ancaman mana yang paling sulit dimitigasi?** External Validity_____________
**Mengapa?**
> External validity paling sulit dimitigasi karena mitigasinya membutuhkan sumber daya yang melampaui scope penelitian ini. Untuk benar-benar memperkuat external validity, dibutuhkan pengujian pada dataset yang lebih beragam (lebih banyak jenis serangan, lebih banyak topologi jaringan), atau bahkan pengujian langsung pada jaringan IoT produksi nyata. Keduanya membutuhkan waktu, perangkat, dan akses jaringan yang tidak selalu tersedia dalam konteks penelitian mahasiswa. Berbeda dengan internal validity yang bisa diatasi dengan desain pipeline yang tepat, atau conclusion validity yang bisa diatasi dengan pemilihan uji statistik yang benar — external validity hanya bisa diperkuat dengan memperluas scope eksperimen yang secara praktis terbatas.___________________________________________________

---

## Refleksi

> Sebuah paper melaporkan "metode kami mengalahkan semua baseline." Apa 3 pertanyaan pertama yang harus diajukan untuk mengevaluasi klaim ini?

**Jawaban:**
1. Apakah perbandingannya adil?___________________________________________________
2. Apakah baseline yang dipilih representatif dan bukan straw man?___________________________________________________
3. Apakah signifikansinya statistik dan bermakna secara praktis? ___________________________________________________
