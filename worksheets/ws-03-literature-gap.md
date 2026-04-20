# WS-03: Literature Mapping & Gap

> **Bab 3 — Literature Review, Research Gap & Baseline**

---

## Ringkasan Materi

### Literature Review = Positioning, Bukan Ringkasan

Literature review bukan merangkum paper satu per satu. Pendekatan yang benar adalah **concept-centric** — organisasi berdasarkan tema, metode, atau variabel. Tujuan: menemukan **pola, kontradiksi, dan gap**.

### Empat Jenis Research Gap

| Jenis Gap | Deskripsi | Contoh |
|-----------|----------|--------|
| **Performance Gap** | Performa belum memadai | Akurasi deteksi hanya 78% pada kasus tertentu |
| **Method Gap** | Pendekatan belum diterapkan | Belum ada yang pakai transformer untuk task ini |
| **Data Gap** | Dataset terbatas/tidak representatif | Semua studi pakai dataset sintetis |
| **Context Gap** | Belum diuji pada konteks berbeda | Belum ada evaluasi di negara berkembang |

Gap terkuat = kombinasi 2+ jenis.

### Systematic Search Strategy

1. **Database**: IEEE Xplore, ACM DL, Scopus, Google Scholar
2. **Boolean query** yang terdokumentasi eksplisit
3. **Snowballing**: backward (telusuri referensi) + forward (cari yang mengutip)
4. Klaim "belum ada penelitian" harus didukung **bukti pencarian**

### Baseline Selection — 3 Kriteria

| Kriteria | Pertanyaan |
|----------|-----------|
| **Relevan** | Apakah menyelesaikan masalah yang sama? |
| **Representatif** | Apakah mewakili common practice? |
| **State-of-the-Art** | Apakah terbaru/terbaik? |

Membandingkan deep learning 2024 dengan decision tree sederhana tanpa justifikasi = **straw man comparison** (perbandingan tidak jujur).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan baca literatur | Mencari solusi yang sudah ada | Memahami apa yang belum terjawab |
| Cara membaca paper | Tutorial, how-to | Metode, limitasi, gap |
| Baseline | Framework terpopuler | State-of-the-art yang rigorous |
| Dokumentasi pencarian | Tidak diperlukan | Wajib (reproducible) |

### Istilah Penting

- **Concept-centric** — Organisasi literatur berdasarkan konsep/metode, bukan per penulis
- **Snowballing** — Backward (telusuri referensi) + Forward (cari yang mengutip paper kunci)
- **Research Position** — Pernyataan eksplisit posisi riset terhadap studi sebelumnya
- **Straw man comparison** — Memilih baseline lemah agar metode sendiri terlihat lebih baik

---

## Template A.3 — Literature Mapping & Gap Identification

```
LITERATURE MAPPING

Topik      : Deteksi Serangan DoS/DDoS pada Jaringan IoT Menggunakan
           Machine Learning dengan Optimasi Seleksi Fitur____________________
Database   : Google Scholar, ResearchGate, portal jurnal nasional
           (jurnal.itg.ac.id, eksplora.stikom-bali.ac.id,
           jurnal.ugm.ac.id, ipssj.com)____________________
Query      : ("deteksi serangan" OR "intrusion detection") AND
           ("IoT" OR "Internet of Things") AND
           ("machine learning" OR "XGBoost" OR "Random Forest")
           AND ("MQTT" OR "DoS" OR "DDoS")____________________
Tahun      : 2022–2025____________________
Hasil awal : 23____ paper → Screening → 5____ paper final

Literature Matrix (concept-centric):

| Study | Tahun | Method | Data | Result | Limitation |
|-------|-------|--------|------|--------|------------|
|1. Dwi Azahra & Pertiwi — Jurnal Algoritma, ITG|2025|XGBoost + PSO (seleksi fitur)|Simulasi MQTT lokal (1.054.817 rekaman: normal, SYN Flood, MQTT Attack)|Akurasi 99,89%; F1-score 96,95–97,81%; waktu latih 0,16–0,24 detik/fold|Dataset hanya dari simulasi lab lokal, belum diuji pada jaringan IoT produksi nyata; hanya dua jenis serangan|
|2. Primadya et al. — Jurnal Eksplora Informatika, STIKOM Bali|2024|Logistic Regression + RFE (seleksi fitur)|CICIoT2023|Akurasi rata-rata meningkat 4% setelah seleksi fitur; waktu pemrosesan 1–4 detik|Logistic Regression kurang cocok untuk data non-linear; waktu komputasi 1–4 detik masih terlalu lambat untuk perangkat IoT terbatas|
|3. Samsudiat & Ramli — JNTETI, UGM|2025|Seleksi fitur gabungan (filter + wrapper) + Optimasi Bayesian|CICIoT2023|Akurasi dan F1 hingga 99,74%; waktu komputasi turun 97,41%|Kompleksitas implementasi tinggi; optimasi Bayesian butuh banyak iterasi, kurang efisien untuk deployment real-time|
|4. Jumaidi et al. — IPSSJ|2025|Ensemble Learning (LightGBM, XGBoost, CatBoost) + SMOTE + SHAP|CIC-BCCC-NRC TabularIoTAttack-2024|Ensemble: akurasi 99,21%; LightGBM tunggal: 97,58% dengan inference 292 detik|Inference time ensemble 891 detik — jauh terlalu lambat untuk lingkungan IoT real-time; kelas Recon Scan sulit terdeteksi akibat tumpang tindih fitur|
|5. Akbar — Skripsi, UIN Malang|2023|Random Forest|Dataset trafik MQTT (bruteforce vs normal)|Akurasi tinggi dalam klasifikasi bruteforce; tidak ditemukan overfitting|Hanya mencakup satu jenis serangan (bruteforce); tidak menguji DoS/SYN Flood; tidak ada seleksi fitur|

Pola yang ditemukan:
  Metode dominan     : Supervised learning berbasis tree (Random Forest, XGBoost, Decision Tree) paling banyak digunakan karena interpretabilitasnya baik dan akurasinya tinggi pada data trafik jaringan.____________________
  Dataset umum       : Mayoritas studi memakai dataset publik seperti CICIoT2023. Hanya satu studi (Dwi Azahra & Pertiwi, 2025) yang menggunakan dataset simulasi langsung dari lingkungan MQTT lokal.____________________
  Limitasi berulang  : Hampir semua studi tidak menguji efisiensi komputasi secara eksplisit untuk perangkat IoT dengan sumber daya terbatas. Waktu inferensi sering diabaikan, padahal ini kritis untuk deployment nyata.____________________

GAP IDENTIFICATION

Gap 1: [Jenis: performance / method / data / context]
  Deskripsi    : Model dengan akurasi tinggi seperti Ensemble Learning
                 (Jumaidi et al., 2025) mencapai 99,21% namun membutuhkan
                 waktu inferensi 891 detik — jauh terlalu lambat untuk
                 perangkat IoT real-time. Di sisi lain, metode yang
                 lebih ringan seperti Logistic Regression (Primadya et al.,
                 2024) hanya mencapai akurasi 93,80% setelah seleksi fitur.
                 Belum ada studi yang berhasil menyeimbangkan keduanya
                 secara optimal khusus pada trafik MQTT.____________________
  Bukti        : Jumaidi et al. (2025) melaporkan inference time 891 detik
                 untuk model ensemble pada dataset IoT. Primadya et al.
                 (2024) mencatat waktu pemrosesan 1–4 detik untuk Logistic
                 Regression, namun akurasinya masih di bawah 94%.
                 Dwi Azahra & Pertiwi (2025) menunjukkan XGBoost+PSO
                 mampu mencapai akurasi 99,89% dengan waktu latih hanya
                 0,24 detik per fold, namun baru diuji pada 2 jenis
                 serangan dari dataset simulasi lokal.____________________
  Signifikansi : Gap ini penting karena perangkat IoT memiliki keterbatasan
                 sumber daya nyata. Model yang tidak efisien tidak dapat
                 diimplementasikan pada broker MQTT skala kecil di
                 lingkungan smart home atau industri ringan.____________________

Gap 2: [Jenis: Data Gap____]
  Deskripsi    : Mayoritas studi menggunakan dataset publik seperti
                 CICIoT2023 yang dihasilkan dari lingkungan lab luar
                 negeri. Belum ada studi yang secara khusus membangun
                 dan memvalidasi dataset trafik MQTT dari simulasi
                 jaringan IoT lokal dengan variasi serangan DoS yang
                 lebih beragam (lebih dari 2 jenis serangan).____________________
  Bukti        : Dari 5 paper yang ditemukan, 3 studi (Primadya et al.,
                 2024; Samsudiat & Ramli, 2025; Jumaidi et al., 2025)
                 menggunakan dataset publik CICIoT2023 atau
                 CIC-BCCC-NRC-2024. Hanya Dwi Azahra & Pertiwi (2025)
                 dan Akbar (2023) yang menggunakan dataset dari simulasi
                 lokal, itupun masing-masing hanya mencakup 2 jenis
                 serangan dan 1 jenis serangan saja.____________________
  Signifikansi : Dataset yang tidak merepresentasikan kondisi jaringan
                 MQTT lokal berpotensi menghasilkan model yang overfit
                 terhadap pola serangan tertentu dan gagal mendeteksi
                 variasi serangan baru yang muncul di lingkungan nyata.____________________

Baseline Selection:
| Baseline | Relevansi | Representatif | Source |
|----------|-----------|---------------|--------|
|Logistic Regression + RFE          |Menyelesaikan task yang sama: deteksi DoS pada jaringan IoT dengan seleksi fitur menggunakan dataset CICIoT2023           |Digunakan sebagai pembanding di hampir semua studi klasifikasi trafik jaringan IoT; mewakili pendekatan lower bound               |Primadya et al. (2024), Jurnal Eksplora Informatika, doi: 10.30864/eksplora.v13i2.1065        |
|Random Forest (tanpa seleksi fitur)          |Digunakan langsung untuk deteksi serangan pada protokol MQTT — task yang identik dengan penelitian ini           |Muncul di 4 dari 5 paper; konsisten menjadi acuan perbandingan di literatur deteksi intrusi IoT; mewakili state-of-the-practice               |Akbar (2023), Skripsi Teknik Informatika, UIN Malang, etheses.uin-malang.ac.id/51427        |
```

---

## Latihan 1 — Concept-Centric Literature Table

Gunakan topik riset dari WS-02. Cari minimal 5 paper relevan menggunakan Google Scholar atau database lain.

**Topik riset:**  Deteksi Serangan DoS pada Jaringan IoT Berbasis Protokol MQTT Menggunakan Machine Learning dengan Seleksi Fitur________________________________________
**Query pencarian:** ("deteksi serangan" OR "attack detection") AND 
("IoT" OR "MQTT" OR "Internet of Things") AND 
("machine learning" OR "XGBoost" OR "Random Forest" OR "SVM") AND 
("DoS" OR "DDoS" OR "denial of service")____________________________________
**Database:** Google Scholar, ResearchGate, jurnal.itg.ac.id, eksplora.stikom-bali.ac.id, jurnal.ugm.ac.id___________________________________________

| # | Study | Tahun | Method | Dataset | Result | Limitasi |
|---|-------|-------|--------|---------|--------|----------|
| 1 | Dwi Azahra & Pertiwi, Jurnal Algoritma (ITG) | 2025 | XGBoost + PSO | Simulasi MQTT lokal (Wireshark) | Akurasi 99,89%; F1 97,81%; latih 0,24 detik | Hanya 2 jenis serangan; dataset lab lokal, belum divalidasi cross-environment |
| 2 |Primadya et al., Jurnal Eksplora Informatika (STIKOM Bali) |2024 |Logistic Regression + RFE |CICIoT2023 |Akurasi naik 4% dengan seleksi fitur; waktu 1–4 detik |Model linear, kurang kuat untuk pola serangan non-linear; lambat untuk IoT |
| 3 |Samsudiat & Ramli, JNTETI (UGM) |2025 |Filter+Wrapper+Bayesian Optimization |CICIoT2023 |Akurasi 99,74%; komputasi turun 97,41% |Proses optimasi Bayesian kompleks dan mahal secara komputasi |
| 4 |Jumaidi et al., IPSSJ |2025 |Ensemble (LightGBM+XGBoost+CatBoost)+SHAP |CIC-BCCC-NRC 2024 |Akurasi 99,21%; F1 99,21% |Inference 891 detik — tidak realistis untuk IoT real-time |
| 5 |Akbar, Skripsi UIN Malang |2023 |Random Forest |Dataset MQTT bruteforce lokal |Akurasi tinggi, anti-overfitting |Hanya deteksi bruteforce; tidak ada seleksi fitur; tidak uji DoS |

**Pola yang terlihat — Metode dominan:** Algoritma berbasis tree (Random Forest, XGBoost) mendominasi — muncul di 4 dari 5 studi — karena mampu menangani data trafik jaringan yang berdimensi tinggi tanpa preprocessing ekstensif.___________________
**Limitasi yang berulang:** Hampir semua studi mengabaikan efisiensi waktu komputasi sebagai metrik utama. Padahal untuk perangkat IoT dengan RAM terbatas, waktu inferensi adalah faktor penentu apakah model benar-benar bisa digunakan.______________________________

---

## Latihan 2 — Gap Identification

Berdasarkan tabel di Latihan 1, identifikasi gap.

| Jenis Gap | Ditemukan? | Gap Statement |
|-----------|-----------|---------------|
| Performance Gap | [✓ ] Ya / [ ] Tidak | Model dengan akurasi tinggi (>99%) seperti ensemble pada studi Jumaidi et al. memiliki inference time 891 detik — tidak dapat digunakan secara real-time di perangkat IoT. Ada trade-off antara akurasi dan efisiensi yang belum terpecahkan |
| Method Gap | [✓ ] Ya / [ ] Tidak |Kombinasi XGBoost dengan seleksi fitur berbasis PSO belum banyak dieksplorasi khusus untuk trafik MQTT. Mayoritas studi menggunakan RFE, PCA, atau SHAP — yang memiliki keterbatasan dimensionalitas atau ketergantungan komputasi tinggi |
| Data Gap | [✓ ] Ya / [ ] Tidak |Hampir semua studi menggunakan dataset publik (CICIoT2023, UNSW-NB15) yang tidak mencerminkan variasi trafik MQTT dari lingkungan IoT lokal Indonesia. Dataset yang dihasilkan dari simulasi jaringan nyata lokal masih sangat jarang |
| Context Gap | [ ✓] Ya / [ ] Tidak |Belum ada studi yang secara eksplisit menguji dan membandingkan performa deteksi DoS pada protokol MQTT di lingkungan jaringan IoT dengan sumber daya komputasi sangat terbatas (mikrokontroler/Raspberry Pi kelas bawah) |

**Gap utama yang dipilih:** Kombinasi Method Gap + Performance Gap — yaitu belum adanya metode seleksi fitur yang ringan secara komputasi namun tetap menghasilkan akurasi tinggi, khusus untuk deteksi DoS pada trafik MQTT IoT._____________________________
**Mengapa gap ini penting (bukan sekadar "belum ada yang meneliti")?**
> Gap ini penting secara praktis karena perangkat IoT seperti sensor rumah pintar, meter pintar, dan aktuator industri memiliki keterbatasan RAM dan CPU yang nyata. Model dengan akurasi 99% tapi butuh ratusan detik untuk inferensi tidak berguna di lapangan. Dengan demikian, gap ini bukan sekadar kekosongan akademik, tapi hambatan nyata bagi implementasi IDS pada jaringan IoT skala kecil hingga menengah di Indonesia. Jika terpecahkan, hasilnya langsung dapat diterapkan pada perangkat IoT yang sudah tersebar luas.___________________________________________________

---

## Latihan 3 — Baseline Selection

Pilih 2 baseline dari literatur yang sudah dibaca.

| # | Baseline | Mengapa Relevan | Mengapa Representatif | Apakah SOTA? | Sumber |
|---|----------|----------------|----------------------|-------------|--------|
| 1 | Logistic Regression + RFE | Menyelesaikan masalah yang sama: deteksi DoS pada IoT dengan seleksi fitur. Menggunakan dataset CICIoT2023 yang merupakan benchmark standar | Logistic Regression adalah algoritma paling umum dipakai sebagai baseline dalam studi klasifikasi trafik jaringan; ada di hampir setiap studi perbandingan | Bukan SOTA, tapi merupakan common practice yang diakui komunitas — cocok sebagai batas bawah perbandingan | Primadya et al. (2024), Jurnal Eksplora Informatika, doi: 10.30864/eksplora.v13i2.1065 |
| 2 |Random Forest (tanpa seleksi fitur) |Digunakan untuk deteksi serangan pada protokol MQTT secara langsung — task yang identik |Random Forest muncul di 4 dari 5 paper yang ditemukan dan konsisten menjadi acuan perbandingan di literatur deteksi intrusi IoT |Bukan SOTA terbaru, tapi merupakan state-of-the-practice yang paling banyak direplikasi dan diakui keandalannya |Akbar (2023), Skripsi UIN Malang; juga dikonfirmasi oleh Jumaidi et al. (2025) sebagai baseline |

**Apakah pemilihan baseline ini bisa dianggap straw man?** [✓ ] Ya / [ ] Tidak
> Justifikasi: Kedua baseline dipilih bukan karena lemah, melainkan karena mewakili dua level praktik yang berbeda — Logistic Regression mewakili pendekatan paling ringan (lower bound), sementara Random Forest mewakili praktik standar yang banyak digunakan. Membandingkan XGBoost+PSO terhadap keduanya memberikan gambaran yang jujur: apakah penambahan PSO benar-benar memberikan keunggulan yang bermakna dibanding metode yang lebih sederhana? Jika XGBoost+PSO hanya sedikit lebih baik dari Random Forest biasa, maka kompleksitas tambahannya tidak justified. Ini adalah perbandingan yang fair, bukan perbandingan palsu.________________________________________

---

## Refleksi

> Apa perbedaan antara "belum ada yang meneliti ini" (klaim tanpa bukti) dengan research gap yang valid? Bagaimana cara membuktikan bahwa sebuah gap benar-benar ada?

**Jawaban:**
> Klaim "belum ada yang meneliti ini" itu sebenarnya agak berbahaya kalau tidak didukung bukti pencarian yang sistematis. Bisa saja penelitinya yang tidak tahu cara cari, bukan berarti memang kosong. Research gap yang valid berbeda: bukan sekadar topik yang belum pernah disentuh, tapi ada masalah yang teridentifikasi secara konkret dari literatur yang sudah ada — misalnya performa yang belum memadai, metode yang belum pernah dicoba untuk konteks tertentu, atau dataset yang tidak representatif.
Cara membuktikannya? Pertama, dokumentasikan query pencarian secara eksplisit dan di database mana. Kedua, tunjukkan paper-paper yang paling dekat dan jelaskan di mana tepatnya paper itu berhenti — itulah titik gap-nya. Ketiga, kalau memungkinkan, lakukan snowballing ke depan (cek siapa yang mengutip paper kunci) untuk memastikan memang belum ada yang mengisi gap itu. Gap yang valid punya "bukti negatif yang terdokumentasi," bukan sekadar pernyataan tanpa jejak pencarian.___________________________________________________

> 1. Dwi Azahra, A. & Pertiwi, K.M.D. (2025). Jurnal Algoritma, 22(2), 2272–2282. https://jurnal.itg.ac.id/index.php/algoritma/article/view/2623
2. Primadya, N.D. et al. (2024). Jurnal Eksplora Informatika, 13(2), 245–252. https://doi.org/10.30864/eksplora.v13i2.1065
3. Samsudiat & Ramli (2025). JNTETI, 14(3), 216–225. https://doi.org/10.22146/jnteti.v14i3.19764
4. Jumaidi et al. (2025). IPSSJ, 2(2). https://ipssj.com/index.php/ojs/article/view/323
5. Akbar, G.M.I. (2023). Skripsi Teknik Informatika, UIN Maulana Malik Ibrahim Malang. http://etheses.uin-malang.ac.id/51427/___________________________________________________
