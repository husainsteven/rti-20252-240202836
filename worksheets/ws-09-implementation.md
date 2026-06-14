# WS-09: Implementation & Environment

> **Bab 9 — Implementasi Riset & Kontrol Lingkungan**

---

## Ringkasan Materi

### Implementasi Riset ≠ Coding Biasa

Tujuan implementasi riset bukan membuat software yang berfungsi, melainkan membangun **instrumen pengukuran yang konsisten**. Setiap modul harus di-mapping ke variabel (dari Bab 6), parameter harus config-driven, dan logging aktif dari hari pertama.

### Reproducible Implementation Model

```
Design → Implementation → Environment Setup → Execution Consistency → Reproducibility → Trustworthy Result
```

Setiap transisi memiliki syarat:
- Design → Implementation: kode sesuai mapping variabel-ke-komponen
- Implementation → Environment: versi, dependency, seed, path, OS eksplisit
- Environment → Consistency: seed terkunci, urutan deterministik
- Consistency → Reproducibility: dokumentasi lengkap
- Reproducibility → Trust: siapa pun ikuti dokumentasi → hasil sama/serupa

### Repeatability vs Reproducibility

| Level | Peneliti | Environment | Hasil |
|-------|---------|-------------|-------|
| **Repeatability** | Sama | Sama | Sama persis |
| **Reproducibility** | Berbeda | Berbeda (ikuti docs) | Sama/serupa |

Capai **repeatability** dulu, baru **reproducibility**.

### Engineering vs Research Perspective

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan | Sistem berfungsi untuk user | Instrumen pengukuran konsisten |
| Dependency | Update ke terbaru | Lock di versi spesifik |
| Testing | Unit, integration, E2E | Repeatability test (run ulang → sama?) |
| Dokumentasi | User guide, API docs | Environment spec, execution steps, expected output |
| Config | Default masuk akal | Setiap parameter eksplisit & adjustable |

### Jebakan Kognitif

1. Menunda environment setup → bug sulit dilacak
2. Tidak pakai version control → hasil tidak bisa direkonstruksi
3. Menolak Docker/container → "di laptop saya bisa" saat review
4. 3× hasil sama ≠ repeatable (bisa cache/state tersimpan)

### Istilah Penting

- **Environment Specification** — Deskripsi lengkap: hardware, OS, runtime, library + versi, config, seed
- **Dependency** — Komponen eksternal yang harus di-lock versinya
- **Config-driven** — Parameter dieksternalisasi ke file konfigurasi, bukan hardcode

---

## Template A.9 — Dokumentasi Setup Eksperimen

```
EXPERIMENT SETUP DOCUMENTATION

Hardware:
  CPU     : Intel Core i5-1235U, 10 Core (2P+8E), 1.3GHz base / 4.4GHz boost____________________
  RAM     : 8 GB DDR4____________________
  GPU     : CPU-only (tidak menggunakan GPU — XGBoost dan sklearn berjalan di CPU)____________________
  Storage : 512 GB SSD NVMe____________________

Software:
  OS        : Windows 11 Home 64-bit (atau Google Colab: Ubuntu 22.04 LTS jika dijalankan di cloud)____________________
  Runtime   : Python 3.10.12____________________
  Framework : scikit-learn 1.3.2 sebagai framework utama klasifikasi dan evaluasi____________________

Dependencies:
| Library | Version | Sumber | Hash/Checksum |
|---------|---------|--------|---------------|
| scikit-learn        | 1.3.2        | PyPI       | sha256: dikunci via pip freeze              |
| xgboost        | 2.0.3        | PyPI       | sha256: dikunci via pip freeze              |
| pyswarms        | 1.3.0        | PyPI       | sha256: dikunci via pip freeze              |
| pandas        | 2.1.4        | PyPI       | sha256: dikunci via pip freeze             |
| numpy        | 1.26.2        | PyPI       | sha256: dikunci via pip freeze              |
| matplotlib        | 3.8.2        | PyPI       | sha256: dikunci via pip freeze              |
| scipy        | 1.11.4        | PyPI       | sha256: dikunci via pip freeze              |

Konfigurasi:
  Config file     : config.yaml — dikelola via version control (Git), tidak boleh di-hardcode di dalam kode____________________
  Random seed     : 42 — ditetapkan di semua level: Python random.seed(42), NumPy np.random.seed(42), XGBoost random_state=42, Stratified K-Fold random_state=42____________________
  Hyperparameters : Semua menggunakan default sklearn/xgboost kecuali yang menjadi bagian dari metode — n_splits=10 untuk K-Fold, n_particles=30 dan n_iterations=100 untuk PSO____________________

Reproducibility Check:
  [ x ] Dependency terdokumentasi (requirements.txt / lock file)
  [ x ] Seed ditetapkan di semua level (Python, NumPy, framework)
  [ x ] Config di version control
  [ x ] README instruksi reproduksi lengkap
```

---

## Latihan 1 — Environment Specification

Dokumentasikan environment untuk eksperimen Anda (boleh environment saat ini atau yang direncanakan).

| Komponen | Spesifikasi |
|----------|------------|
| CPU | Intel Core i5-1235U, 10 Core, 1.3GHz base / 4.4GHz boost |
| RAM | 8 GB DDR4 |
| GPU | CPU-only — XGBoost, sklearn, dan pyswarms tidak memerlukan GPU |
| OS | Windows 11 Home 64-bit / Google Colab Ubuntu 22.04 LTS |
| Runtime | Python 3.10.12 |
| Framework | scikit-learn 1.3.2 |
| Random Seed | 42 — ditetapkan di semua level sebelum eksperimen dimulai |

**Dependencies (minimal 5):**

| Library | Version | Alasan Dibutuhkan |
|---------|---------|-------------------|
| scikit-learn | 1.3.2 | Logistic Regression, Random Forest, Stratified K-Fold, classification_report, MinMaxScaler, LabelEncoder, RFE |
| xgboost | 2.0.3 | Algoritma XGBoost sebagai metode utama yang diuji |
| pyswarms | 1.3.0 | Implementasi PSO untuk seleksi fitur pada kondisi XGBoost+PSO |
| pandas | 2.1.4 | Loading, manipulasi, dan penyimpanan dataset MQTT lokal dalam format DataFrame |
| numpy | 1.26.2 | Operasi numerik, array manipulation, dan kontrol random seed di level NumPy |
| matplotlib | 3.8.2 | Visualisasi hasil: grafik perbandingan metrik antar kondisi untuk WS-12 |
| scipy | 1.11.4 | Uji Wilcoxon Signed-Rank dan perhitungan Cohen's d untuk analisis statistik WS-14 |

---

## Latihan 2 — Repeatability Test Plan

Rancang tes repeatability sederhana: jalankan kode yang sama 3× di environment yang sama.

| Run | Seed | Metrik Utama | Hasil Sama? |
|-----|------|-------------|-------------|
| 1 | 42 | F1-Score per kelas (Normal, SYN Flood, MQTT Attack) dan waktu inferensi per fold untuk semua kondisi | — |
| 2 | 42 | F1-Score per kelas dan waktu inferensi per fold | [ x ] Ya / [ ] Tidak |
| 3 | 42 | F1-Score per kelas dan waktu inferensi per fold | [ x ] Ya / [ ] Tidak |

**Jika hasil berbeda, kemungkinan penyebab:**
> Penyebab paling umum di penelitian ini:

1. Random state tidak dikontrol di semua level — PSO menggunakan numpy random di balik layar. Jika hanya Python seed yang di-set tapi NumPy seed tidak, hasil PSO bisa berbeda antar run meskipun Python seed sama.

2. Thermal throttling — CPU yang overheat pada run ke-2 dan ke-3 akan menurunkan clock speed sehingga waktu inferensi bisa berbeda antar run meskipun F1-Score tetap sama. Ini bukan bug pada kode, tapi perlu dicatat sebagai variasi lingkungan.

3. Background process aktif — Update Windows atau antivirus scan yang berjalan bersamaan dapat mempengaruhi waktu inferensi secara tidak konsisten.___________________________________________________

**Checklist kontrol yang sudah diterapkan:**
- [ x ] Random seed di-set di semua level
- [ x ] Tidak ada background process yang mengganggu
- [ x ] Cache dibersihkan antar-run
- [ x ] Config file yang sama untuk semua run

---

## Latihan 3 — README Eksperimen

Tulis README minimum untuk eksperimen Anda (6 komponen wajib).

```
# Judul Eksperimen: Deteksi Serangan DoS pada IoT Berbasis MQTT
Menggunakan XGBoost + PSO vs Baseline (LR+RFE, Random Forest)
____________________

## 1. Environment
> - OS      : Windows 11 / Google Colab Ubuntu 22.04 LTS
- Python  : 3.10.12
- CPU     : Intel Core i5-1235U, 8 GB RAM
- GPU     : CPU-only

## 2. Installation
>  Clone repository dan install dependency:
git clone https://github.com/[username]/mqtt-dos-detection
cd mqtt-dos-detection
pip install -r requirements.txt

- requirements.txt berisi versi yang sudah dikunci:
- scikit-learn==1.3.2
- xgboost==2.0.3
- pyswarms==1.3.0
- pandas==2.1.4
- numpy==1.26.2
- matplotlib==3.8.2
- scipy==1.11.4

## 3. Data
> - Sumber  : Simulasi trafik MQTT lokal, direkam menggunakan Wireshark
- Format  : CSV, 1.054.817 rekaman, 49 fitur
- Kelas   : Normal (7.200), SYN Flood (7.200), MQTT Attack (1.040.417)
- Path    : data/mqtt_dataset.csv (sesuai config.yaml)

## 4. Execution
> Jalankan eksperimen untuk semua kondisi:
python run_experiment.py --config config.yaml

Jalankan kondisi tertentu saja:
python run_experiment.py --config config.yaml --model xgboost_pso
python run_experiment.py --config config.yaml --model lr_rfe
python run_experiment.py --config config.yaml --model random_fores

## 5. Configuration
> model_type    : xgboost_pso   # ganti: lr_rfe / random_forest
k_fold        : 10
random_state  : 42
dataset_path  : data/mqtt_dataset.csv
scaler        : minmax
pso_particles : 30
pso_iterations: 100
output_path   : results/results.csv

## 6. Expected Output
> File: results/results.csv
 Format per baris: kondisi, fold, accuracy, f1_normal,
                   f1_synflood, f1_mqttattack, inference_time

 Contoh output XGBoost+PSO:
 xgboost_pso, fold_1, 0.9989, 0.9981, 0.9695, 0.9781, 0.24
 xgboost_pso, fold_2, 0.9987, 0.9979, 0.9701, 0.9775, 0.23
 ...
 Summary: mean F1 >= 0.97, mean inference_time < 1.0 detik/fold
```

---

## Refleksi

> Apakah eksperimen Anda saat ini bisa direproduksi oleh orang lain tanpa bantuan Anda? Komponen apa yang masih hilang?

**Level saat ini:** [ x ] Repeatability / [ ] Reproducibility / [ ] Belum keduanya
**Komponen yang belum terdokumentasi:**
> 1. Hash dataset — Checksum (MD5 atau SHA256) dari file dataset MQTT lokal belum dicatat. Tanpa hash ini, tidak ada cara untuk memverifikasi bahwa dataset yang digunakan reviewer identik dengan yang digunakan peneliti.

2. Verifikasi di environment berbeda — README sudah ditulis, tapi belum diuji apakah instruksinya benar-benar berfungsi di mesin lain. Idealnya ada satu sesi pengujian di Google Colab untuk membuktikan instruksi README dapat diikuti dari nol.___________________________________________________
