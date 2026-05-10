# WS-06: System-Experiment Mapping

> **Bab 6 — System Design sebagai Experimental Artifact**

---

## Ringkasan Materi

### Sistem = Instrumen Pengujian, Bukan Produk

Seorang engineer bertanya "apakah sistem bekerja?" — seorang peneliti bertanya "apa yang bisa dibuktikan sistem ini?" Sistem dalam riset adalah **artifact** — objek yang sengaja dibuat untuk menguji klaim spesifik.

### System as Experiment Model

```
RQ → Variable → System Component → Experimental Setup → Output
```

Setiap komponen sistem harus bisa ditelusuri ke variabel riset (top-down), dan setiap pengukuran harus menjawab RQ (bottom-up).

### Mapping Variabel ke Komponen

| Tipe Variabel | Peran di Sistem | Contoh |
|---------------|----------------|--------|
| **IV** (Independent) | Modul yang bisa di-toggle/swap | Algoritma A vs B |
| **DV** (Dependent) | Modul pengukuran | Logger, metrics collector |
| **CV** (Control) | Config yang dikunci | Dataset, parameter tetap |

Jika variabel tidak bisa di-map ke komponen apapun → arsitektur perlu didesain ulang.

### 4 Prinsip Desain Eksperimental

| Prinsip | Pertanyaan Kunci |
|---------|-----------------|
| **Traceability** | Komponen ini melayani variabel yang mana? |
| **Modularity** | Bisakah IV diubah tanpa memengaruhi yang lain? |
| **Controllability** | Apakah CV dieksternalisasi ke config file? |
| **Measurability** | Apakah sistem otomatis menghasilkan data yang dibutuhkan? |

### Variable Isolation melalui Arsitektur

- **Modular architecture** — Pisahkan berdasarkan variabel
- **Configuration-driven** — Ubah config (YAML/JSON), bukan code
- **Feature toggles** — On/off flag untuk ablation study

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan sistem | Memenuhi kebutuhan user | Menguji hipotesis, menghasilkan bukti |
| Arsitektur | Optimasi performa & skalabilitas | Optimasi isolasi variabel & reprodusibilitas |
| Konfigurasi | Sering hardcoded | Dieksternalisasi ke config file |
| Fitur tambahan | Menambah nilai user | Menambah noise jika tidak terkait RQ |

### Istilah Penting

- **Artifact** — Objek yang sengaja dibuat untuk memecahkan masalah atau menguji proposisi
- **Traceability** — Kemampuan menelusuri hubungan RQ → variabel → komponen → output
- **Variable Isolation** — Mengubah hanya satu variabel sambil menahan yang lain konstan
- **Ablation Study** — Menguji kontribusi tiap komponen dengan melepasnya satu per satu
- **Configuration-driven Execution** — Semua parameter di config file, bukan hardcoded

---

## Template A.6 — Mapping RQ ke Arsitektur Sistem

```
SYSTEM-EXPERIMENT MAPPING

Research Question: Apakah XGBoost+PSO menghasilkan akurasi, F1-Score per kelas, dan waktu inferensi yang lebih baik dibanding Logistic Regression+RFE dan Random Forest dalam mendeteksi DoS pada dataset MQTT lokal?____________________

Variable → Component Mapping:
| Variabel | Tipe | Komponen Sistem | Cara Manipulasi/Pengukuran |
|----------|------|-----------------|---------------------------|
|Jenis algoritma klasifikasi          | IV   |Modul Classifier — bisa di-swap antar kondisi eksperimen                 |      Ganti satu baris di config.yaml: model_type: xgboost_pso / lr_rfe / random_forest                     |
|Akurasi dan F1-Score per kelas          | DV   |Modul Evaluasi — menghasilkan confusion matrix dan classification report otomatis setiap fold                 |sklearn classification_report() dipanggil otomatis setelah predict() setiap fold; hasilnya disimpan ke results.csv                           |
|Dataset MQTT lokal          | CV   |Modul Data Loader — membaca dataset yang sama untuk semua kondisi                 |Path dataset dikunci di config.yaml; tidak bisa diubah tanpa mengubah file config secara eksplisit                           |

4 Prinsip Desain:
  [ x ] Traceability — Setiap komponen bisa ditelusuri ke variabel
  [ x ] Variable Isolation — IV bisa diubah tanpa mengubah CV
  [ x ] Measurement Integration — Pengukuran DV built-in
  [ x ] Reproducibility — Setup bisa direkonstruksi

Experimental Setup:
  Input data     : Dataset MQTT lokal 1.054.817 rekaman dalam format CSV — sudah melalui preprocessing (MinMaxScaler + Label Encoding)
____________________
  Parameter      : Dikunci di config.yaml — model_type, k_fold=10, random_state=42, dataset_path, scaler_type, hasil PSO (11 fitur terpilih untuk kondisi XGBoost+PSO)____________________
  Output format  : results.csv berisi per-fold metrics (accuracy, F1 per kelas, inference_time) + summary statistics (mean, std) + metadata hardware____________________
```

---

## Latihan 1 — Variable-to-Component Mapping

Gunakan RQ dan variabel dari WS-05. Petakan ke komponen sistem.

**RQ:** Apakah XGBoost+PSO menghasilkan akurasi, F1-Score per kelas, dan waktu inferensi yang lebih baik dibanding Logistic Regression+RFE dan Random Forest dalam mendeteksi DoS pada dataset MQTT lokal?__________________________________________________

| Variabel | Tipe | Komponen Sistem | Cara Manipulasi / Pengukuran |
|----------|------|-----------------|---------------------------|
| Jenis algoritma | IV | Modul Classifier (swappable) | Ganti config: model_type: xgboost_pso / lr_rfe / random_forest |
| Akurasi & F1-Score | DV | Modul Evaluasi (built-in) | sklearn classification_report() otomatis per fold → disimpan ke results.csv |
| Dataset MQTT lokal | CV | Modul Data Loader (locked) | Path dikunci di config — tidak bisa diubah tanpa mengubah config secara eksplisit |

**Apakah semua variabel bisa di-map?** [ ✓ ] Ya / [ ] Tidak
> Jika tidak, komponen apa yang perlu ditambahkan? Semua variabel (IV, DV, CV) memiliki komponen sistem yang bersesuaian. Tidak ada variabel yang "mengambang" tanpa representasi di arsitektur._________

---

## Latihan 2 — 4 Prinsip Desain

Evaluasi desain sistem terhadap 4 prinsip.

| Prinsip | Status | Bukti / Penjelasan |
|---------|--------|-------------------|
| Traceability | Terpenuhi ✅ | Setiap modul diberi label variabel: Classifier=IV, Evaluasi+Timer=DV, DataLoader+Validator+Preprocessor=CV. Struktur folder kode mencerminkan pembagian ini sehingga mudah ditelusuri |
| Modularity | Terpenuhi ✅ | Modul Classifier sepenuhnya terpisah dari modul lainnya. Mengganti dari XGBoost+PSO ke Random Forest hanya membutuhkan perubahan satu baris di config.yaml — tidak ada perubahan kode di modul lain |
| Controllability | Terpenuhi ✅ | Semua CV dieksternalisasi ke config.yaml: dataset_path, k_fold, random_state, scaler_type, selected_features. Tidak ada parameter yang di-hardcode di dalam kode |
| Measurability | Terpenuhi ✅ | Modul Evaluasi dan Timer berjalan otomatis tanpa intervensi manual. Hasilnya langsung tersimpan ke results.csv dengan format yang konsisten setiap run |

**Prinsip mana yang paling sulit dipenuhi?** Modularity — khususnya pada integrasi PSO dengan XGBoost. PSO perlu menjalankan evaluasi model sebagai bagian dari proses seleksi fitur, sehingga ada ketergantungan antara Modul Feature Selection dan Modul Classifier yang harus dikelola dengan hati-hati agar tidak menciptakan data leakage._______________
**Strategi untuk mengatasinya:**
> PSO dijalankan hanya pada data latih di dalam setiap fold — tidak pernah menyentuh data uji. Fitur yang dipilih PSO pada fold tertentu hanya digunakan untuk fold tersebut, bukan untuk fold lain. Ini memastikan isolasi variabel tetap terjaga dan tidak ada informasi dari data uji yang "bocor" ke proses pelatihan.___________________________________________________

---

## Latihan 3 — Ablation Study Planning

Jika sistem memiliki 3 komponen utama, rencanakan ablation study.

| Kondisi | Komponen A | Komponen B | Komponen C | Hasil yang Diharapkan |
|---------|-----------|-----------|-----------|----------------------|
| Full |✅ |✅ | ✅ | Akurasi dan F1 tertinggi, waktu inferensi terendah — kondisi terbaik yang diklaim |
| – A | ❌ | ✅ | ✅ | Akurasi dan F1 diprediksi turun — membuktikan kontribusi nyata XGBoost dibanding RF biasa |
| – B | ✅ | ❌ | ✅ | Waktu inferensi meningkat dan F1 kelas minoritas turun — membuktikan kontribusi PSO dalam mereduksi dimensi |
| – C | ✅ | ✅ | ❌ | Variance estimasi performa lebih tinggi dan tidak stabil — membuktikan kontribusi Stratified K-Fold |

**Komponen mana yang diprediksi paling berkontribusi?** Komponen B — Seleksi Fitur PSO_____
**Mengapa?**
> Dari literatur di WS-03, Dwi Azahra & Pertiwi (2025) menunjukkan bahwa PSO berhasil mereduksi fitur dari jumlah penuh menjadi hanya 11 fitur tanpa kehilangan akurasi yang berarti, sekaligus menurunkan waktu pelatihan secara signifikan. Ini mengindikasikan bahwa sebagian besar fitur dalam dataset MQTT bersifat redundan. Dengan menghapus fitur redundan, PSO tidak hanya mempercepat model tapi juga mengurangi risiko overfitting — dua manfaat sekaligus yang langsung menjawab root cause dari WS-02.___________________________________________________

---

## Refleksi

> Apa risiko jika sistem dibangun seperti produk (monolitik, fitur lengkap) lalu baru dilakukan eksperimen? Mengapa arsitektur modular penting untuk riset?

**Jawaban:**
> Kalau sistem dibangun seperti produk — monolitik, fitur lengkap, semua komponen saling bergantung — ada dua risiko besar dalam konteks penelitian. Pertama, variable isolation menjadi hampir mustahil. Ketika semua komponen saling berkaitan erat, mengubah satu hal (misalnya algoritma) bisa diam-diam mempengaruhi hal lain (misalnya cara data diproses), sehingga perbedaan hasil antar kondisi tidak bisa diklaim murni karena algoritma yang berbeda.___________________________________________________
> Kedua, reproduksi eksperimen menjadi sangat sulit. Peneliti lain tidak bisa mereproduksi hasil jika konfigurasi tersebar di berbagai tempat dalam kode, atau jika parameter di-hardcode tanpa dokumentasi. Arsitektur modular menyelesaikan kedua masalah ini: setiap komponen punya satu tanggung jawab yang jelas, perubahan pada IV tidak mempengaruhi CV, dan seluruh konfigurasi tersimpan di satu file yang bisa dibagikan. Dalam riset, arsitektur bukan soal estetika kode — tapi soal validitas eksperimen.___________________________________________________
