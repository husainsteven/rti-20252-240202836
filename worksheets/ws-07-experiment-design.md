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

Research Question : Apakah XGBoost+PSO menghasilkan akurasi, F1-Score per kelas, dan waktu inferensi yang lebih baik secara statistik dibandingkan Logistic Regression+RFE dan Random Forest dalam mendeteksi serangan DoS pada dataset trafik MQTT lokal?____________________
Hypothesis        : H₀: Tidak ada perbedaan signifikan (p > 0,05) antara F1-Score dan waktu inferensi XGBoost+PSO dibandingkan kedua baseline pada dataset MQTT lokal.

H₁: XGBoost+PSO menghasilkan F1-Score lebih tinggi dan waktu inferensi lebih rendah secara signifikan (p < 0,05) dibandingkan kedua baseline pada dataset MQTT lokal yang sama.____________________
Tipe Eksperimen   : [ x ] Comparison  [ ] Ablation  [ ] Parameter

Kondisi Eksperimen:
| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control | Logistic Regression + RFE sebagai baseline ringan (Primadya et al., 2024)           | LR + RFE          | Dataset MQTT lokal; K-Fold k=10, seed=42; MinMaxScaler; mesin sama            |
| Control | Random Forest tanpa seleksi fitur sebagai baseline standar (Akbar, 2023)          | Random Forest          | Dataset MQTT lokal; K-Fold k=10, seed=42; MinMaxScaler; mesin sama            |
| Treatment | XGBoost + PSO sebagai metode utama untuk menjawab gap WS-03. PSO memilih 11 fitur paling relevan         | XGBoost + PSO         | Dataset MQTT lokal; K-Fold k=10, seed=42; MinMaxScaler; mesin sama; 11 fitur PSO             |

Fairness Checklist:
  [ x ] Dataset identik untuk semua kondisi
  [ x ] Preprocessing setara
  [ x ] Tuning effort setara
  [ x ] Environment identik
  [ x ] Metrik evaluasi sama

Threat Analysis:
| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal    |Data leakage — PSO yang dijalankan sebelum pembagian fold bisa menyebabkan informasi data uji ikut mempengaruhi seleksi fitur sehingga akurasi over-optimistic                 |PSO dijalankan hanya pada data latih di dalam setiap fold. Data uji tidak pernah menyentuh proses seleksi fitur. Diverifikasi dengan cek tidak ada overlap indeks antar set          |
| External    |Dataset hanya mencakup 2 jenis serangan DoS dari satu simulasi lokal — hasil tidak bisa langsung digeneralisasi ke variasi serangan atau topologi jaringan IoT yang berbeda                 |Batasi klaim kesimpulan pada konteks dataset ini saja. Nyatakan eksplisit di Discussion dan sarankan replikasi pada dataset lebih beragam sebagai future work          |
| Construct   |Accuracy menyesatkan pada data imbalanced — kelas MQTT Attack mendominasi (1.040.417 rekaman). Model yang selalu prediksi kelas mayoritas pun bisa dapat accuracy tinggi                 |F1-Score per kelas dijadikan primary metric. Accuracy hanya sebagai secondary. Confusion matrix lengkap dilaporkan per kondisi agar tidak ada informasi yang tersembunyi          |
| Conclusion  |K-Fold k=10 hanya menghasilkan 10 nilai per metrik — terlalu kecil untuk memverifikasi normalitas sehingga uji parametrik seperti t-test tidak valid digunakan                 |Gunakan uji Wilcoxon Signed-Rank non-parametrik. Tambahkan Cohen's d sebagai effect size. Tetapkan alpha = 0,05 sebelum eksperimen dimulai          |

Statistical Plan:
  Uji statistik   : Wilcoxon Signed-Rank Test (non-parametrik, berpasangan)____________________
  Justifikasi      : Data K-Fold (10 nilai) tidak bisa diasumsikan normal; Wilcoxon robust untuk distribusi non-normal dan cocok untuk perbandingan berpasangan____________________
  Alpha            : α = 0,05____________________
  Effect size min  :  Cohen's d > 0,5 (medium effect) — memastikan perbedaan bermakna secara praktis untuk implementasi nyata di IoT____________________
```

---

## Latihan 1 — Desain Eksperimen

Susun desain eksperimen berdasarkan RQ, variabel, dan sistem dari WS-04 sampai WS-06.

**RQ:** Apakah XGBoost+PSO menghasilkan akurasi, F1-Score per kelas, dan waktu inferensi yang lebih baik dibanding Logistic Regression+RFE dan Random Forest dalam mendeteksi DoS pada dataset MQTT lokal?__________________________________________________
**Tipe eksperimen:** [ x ] Comparison / [ ] Ablation / [ ] Parameter

| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control | Logistic Regression + RFE sebagai baseline ringan (Primadya et al., 2024) | LR + RFE | Dataset MQTT lokal; K-Fold k=10, seed=42; MinMaxScaler; mesin sama |
| Control | Random Forest tanpa seleksi fitur sebagai baseline standar (Akbar, 2023) | Random Forest | Dataset MQTT lokal; K-Fold k=10, seed=42; MinMaxScaler; mesin sama |
| Treatment | XGBoost + PSO sebagai metode utama yang diusulkan untuk menjawab gap WS-03 | XGBoost + PSO | Dataset MQTT lokal; K-Fold k=10, seed=42; MinMaxScaler; mesin sama; 11 fitur PSO |

---

## Latihan 2 — Fairness Checklist

Evaluasi apakah desain eksperimen di Latihan 1 sudah fair.

| Kriteria | Status | Detail |
|----------|--------|--------|
| Dataset identik | ✅ | Semua algoritma pakai dataset MQTT lokal yang sama — 1.054.817 rekaman, dikunci di config.yaml |
| Preprocessing setara | ✅ | MinMaxScaler + Label Encoding dijalankan sekali sebelum eksperimen, hasilnya dibaca semua algoritma |
| Tuning effort setara | ✅ | Semua pakai parameter default sklearn. PSO adalah bagian dari metode yang diuji, bukan tuning ekstra untuk XGBoost |
| Environment identik | ✅ | Semua eksperimen dijalankan di mesin yang sama secara berurutan; spesifikasi hardware dicatat di results.csv |
| Metrik evaluasi sama | ✅ | Accuracy, F1-Score per kelas, dan waktu inferensi diukur identik untuk semua algoritma |

**Ada yang tidak fair?** [ ] Ya / [ Tidak ] Tidak
> Jika ya, bagaimana cara memperbaikinya? ________________

---

## Latihan 3 — Threat Analysis

Identifikasi ancaman validitas untuk desain eksperimen ini.

| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal | Data leakage PSO — jika PSO dijalankan pada seluruh dataset sebelum pembagian fold, informasi data uji mempengaruhi seleksi fitur sehingga akurasi over-optimistic | PSO hanya dijalankan pada data latih di dalam setiap fold. Data uji tidak pernah menyentuh proses seleksi fitur |
| External | Dataset hanya 2 jenis serangan dari satu simulasi lokal — tidak bisa digeneralisasi ke jenis serangan atau topologi jaringan IoT yang berbeda | Batasi klaim pada konteks dataset ini. Nyatakan sebagai limitasi di Discussion dan sarankan replikasi sebagai future work |
| Construct | Accuracy menyesatkan pada data imbalanced — kelas MQTT Attack mendominasi sehingga accuracy tinggi bisa dicapai tanpa deteksi kelas minoritas yang baik | F1-Score per kelas sebagai primary metric. Accuracy hanya secondary. Confusion matrix lengkap dilaporkan per kondisi |
| Conclusion | K-Fold k=10 menghasilkan hanya 10 nilai per metrik — terlalu kecil untuk asumsi normalitas sehingga t-test tidak valid | Gunakan Wilcoxon Signed-Rank non-parametrik. Tambahkan Cohen's d. Alpha = 0,05 ditetapkan sebelum eksperimen |

**Ancaman mana yang paling sulit dimitigasi?** External Validity_____________
**Mengapa?**
> karena mitigasinya membutuhkan pengujian pada lebih banyak dataset dan topologi jaringan yang berbeda, sesuatu yang melampaui scope penelitian ini. Berbeda dengan internal dan conclusion validity yang bisa diatasi dengan desain pipeline dan pemilihan uji statistik yang tepat. ___________________________________________________

---

## Refleksi

> Sebuah paper melaporkan "metode kami mengalahkan semua baseline." Apa 3 pertanyaan pertama yang harus diajukan untuk mengevaluasi klaim ini?

**Jawaban:**
1.  Apakah perbandingannya adil?___________________________________________________
2.  Apakah baseline yang dipilih representatif dan bukan straw man?___________________________________________________
3.  Apakah perbedaannya signifikan secara statistik dan bermakna secara praktis?___________________________________________________
