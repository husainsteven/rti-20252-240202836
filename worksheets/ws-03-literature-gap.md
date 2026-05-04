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

Topik      : Deteksi Serangan DoS pada Jaringan IoT Berbasis MQTT Menggunakan Machine Learning____________________
Database   : Google Scholar, ResearchGate, jurnal.itg.ac.id, eksplora.stikom-bali.ac.id, jurnal.ugm.ac.id, ipssj.com____________________
Query      : ("deteksi serangan" OR "attack detection") AND ("IoT" OR "MQTT") AND ("machine learning" OR "XGBoost" OR "Random Forest") AND ("DoS" OR "DDoS")____________________
Tahun      : 2022–2025____________________
Hasil awal : 23____ paper → Screening → 5____ paper final

Literature Matrix (concept-centric):

| Study | Tahun | Method | Data | Result | Limitation |
|-------|-------|--------|------|--------|------------|
|1. Dwi Azahra & Pertiwi
Jurnal Algoritma, ITG	|2025|XGBoost + PSO|Simulasi MQTT lokal: 1.054.817 rekaman (Normal, SYN Flood, MQTT Attack)|Akurasi 99,89%; F1 97,81%; waktu latih 0,24 dtk/fold|Hanya 2 jenis serangan; dataset simulasi lokal belum diuji di jaringan produksi nyata|
|2.Primadya et al.
Jurnal Eksplora Informatika, STIKOM Bali|2024|Logistic Regression + RFE|CICIoT2023|Akurasi naik rata-rata 4% dengan seleksi fitur; waktu 1–4 detik|Model linear kurang kuat untuk pola non-linear; waktu 1–4 detik masih lambat untuk IoT terbatas|
|3. Samsudiat & Ramli — JNTETI, UGM|2025|Filter + Wrapper + Bayesian Optimization|CICIoT2023|Akurasi dan F1 hingga 99,74%; waktu komputasi turun 97,41%|Proses Bayesian optimization kompleks dan mahal secara komputasi untuk real-time IoT|
|4. Jumaidi et al. — IPSSJ|2025|Ensemble Learning (LightGBM, XGBoost, CatBoost) + SMOTE + SHAP|CIC-BCCC-NRC TabularIoTAttack-2024|Akurasi 99,21%; F1 99,21%|Inference time ensemble mencapai 891 detik — tidak realistis untuk IoT real-time|
|5. Akbar — Skripsi, UIN Malang|2023|Random Forest|Dataset MQTT lokal (bruteforce vs normal)|Akurasi tinggi; tidak ditemukan overfitting|Hanya 1 jenis serangan (bruteforce); tidak uji DoS; tidak ada seleksi fitur|

Pola yang ditemukan:
  Metode dominan     :Algoritma berbasis tree (Random Forest, XGBoost) — muncul di 4 dari 5 studi karena andal untuk data trafik jaringan berdimensi tinggi.____________________
  Dataset umum       : CICIoT2023 dipakai oleh 3 dari 5 studi. Hanya 2 studi yang membangun dataset simulasi lokal sendiri.____________________
  Limitasi berulang  :Efisiensi waktu inferensi hampir selalu diabaikan padahal kritis untuk deployment nyata di perangkat IoT terbatas.____________________

GAP IDENTIFICATION

Gap 1: [Jenis: performance / method / data / context]
  Deskripsi    : Dari literatur yang dibaca, terlihat jelas ada trade-off yang belum terpecahkan antara akurasi dan efisiensi komputasi. Model yang sangat akurat ternyata terlalu lambat untuk IoT, sementara model yang lebih cepat akurasinya masih kurang memadai. Ini langsung berkaitan dengan root cause yang sudah ditemukan di WS-02 — belum ada metode yang benar-benar menjawab kebutuhan nyata perangkat IoT terbatas____________________
  Bukti        : 
Jumaidi et al. (2025) mencapai akurasi 99,21% tetapi inference time-nya 891 detik — tidak mungkin dipakai real-time di IoT. Primadya et al. (2024) lebih ringan dengan waktu 1–4 detik, tapi akurasinya hanya sekitar 93,80%. Dwi Azahra & Pertiwi (2025) menunjukkan XGBoost+PSO berhasil mencapai 99,89% dengan waktu hanya 0,24 detik per fold, namun baru diuji pada 2 jenis serangan dari satu lingkungan simulasi lokal sehingga belum bisa diklaim sebagai solusi yang komprehensif.____________________
  Signifikansi : 
Gap ini bukan hanya soal angka akademik. Perangkat IoT seperti sensor rumah pintar dan aktuator industri punya keterbatasan RAM dan CPU yang nyata di lapangan. Selama trade-off ini belum terpecahkan secara komprehensif, tidak akan ada sistem IDS yang benar-benar bisa diimplementasikan di perangkat IoT skala kecil hingga menengah.____________________

Gap 2: [Jenis: Data Gap____]
  Deskripsi    : Mayoritas studi menggunakan dataset publik buatan luar negeri yang tidak mencerminkan kondisi trafik MQTT dari jaringan IoT lokal. Sementara itu, dataset simulasi lokal yang ada hanya mencakup 1 hingga 2 jenis serangan saja — jauh dari variasi serangan nyata yang mungkin terjadi di lapangan.____________________
  Bukti        : 
Dari 5 paper, sebanyak 3 studi yaitu Primadya et al. (2024), Samsudiat & Ramli (2025), dan Jumaidi et al. (2025) menggunakan dataset publik CICIoT2023 atau CIC-BCCC-NRC-2024 yang dihasilkan dari lingkungan lab luar negeri. Hanya Dwi Azahra & Pertiwi (2025) dan Akbar (2023) yang menggunakan simulasi lokal, itupun masing-masing hanya mencakup 2 dan 1 jenis serangan saja.____________________
  Signifikansi :Model yang dilatih pada dataset yang tidak representatif berisiko overfit terhadap pola serangan tertentu dan bisa gagal total ketika dihadapkan pada variasi serangan baru di jaringan IoT nyata. Ini memperkuat gap pertama — akurasi tinggi di atas kertas belum tentu berarti andal di lapangan.____________________

Baseline Selection:
| Baseline | Relevansi | Representatif | Source |
|----------|-----------|---------------|--------|
|Logistic Regression + RFE          |Menyelesaikan task yang sama: deteksi DoS pada IoT dengan seleksi fitur menggunakan dataset CICIoT2023 yang mencakup serangan DoS          |Digunakan sebagai pembanding di hampir semua studi klasifikasi trafik jaringan IoT; mewakili pendekatan lower bound               |Primadya et al. (2024), Jurnal Eksplora Informatika, doi: 10.30864/eksplora.v13i2.1065        |
|Random Forest (tanpa seleksi fitur)          |Digunakan langsung untuk mendeteksi serangan pada protokol MQTT — task yang identik dengan penelitian ini           |Muncul di 4 dari 5 paper; konsisten menjadi acuan perbandingan di literatur deteksi intrusi IoT; mewakili state-of-the-practice               |Akbar (2023), Skripsi Teknik Informatika, UIN Malang, etheses.uin-malang.ac.id/51427        |
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
| 2 |Primadya et al., Jurnal Eksplora Informatika	 |2024 |Logistic Regression + RFE |CICIoT2023 |Akurasi naik 4% dengan seleksi fitur; waktu 1–4 detik |Model linear; lambat untuk IoT terbatas |
| 3 |Samsudiat & Ramli, JNTETI (UGM) |2025 |Filter+Wrapper+Bayesian Optimization |CICIoT2023 |Akurasi 99,74%; komputasi turun 97,41% |Proses Bayesian kompleks; kurang efisien untuk real-time |
| 4 |Jumaidi et al., IPSSJ |2025 |Ensemble (LightGBM+XGBoost+CatBoost)+SHAP |CIC-BCCC-NRC 2024 |Akurasi 99,21%; F1 99,21% |Inference 891 detik — tidak realistis untuk IoT real-time |
| 5 |Akbar, Skripsi UIN Malang |2023 |Random Forest |Dataset MQTT bruteforce lokal |Akurasi tinggi, anti-overfitting |Hanya 1 jenis serangan; tidak uji DoS; tidak ada seleksi fitur |

**Pola yang terlihat — Metode dominan:** Algoritma berbasis tree (Random Forest, XGBoost) mendominasi — muncul di 4 dari 5 studi — karena mampu menangani data trafik jaringan berdimensi tinggi tanpa preprocessing yang terlalu berat dan hasilnya konsisten di berbagai kondisi dataset.___________________
**Limitasi yang berulang:** Hampir semua studi tidak menjadikan efisiensi waktu inferensi sebagai metrik utama evaluasi. Padahal untuk perangkat IoT dengan RAM terbatas, waktu inferensi adalah faktor penentu apakah model benar-benar bisa digunakan di lapangan, bukan hanya di atas kertas.______________________________

---

## Latihan 2 — Gap Identification

Berdasarkan tabel di Latihan 1, identifikasi gap.

| Jenis Gap | Ditemukan? | Gap Statement |
|-----------|-----------|---------------|
| Performance Gap | [✓ ] Ya / [ ] Tidak | Model akurasi tinggi seperti Ensemble dari Jumaidi et al. (2025) memiliki inference time 891 detik — tidak layak untuk IoT real-time. Model lebih ringan seperti Logistic Regression dari Primadya et al. (2024) hanya mencapai sekitar 93,80%. Belum ada yang berhasil menyeimbangkan keduanya khusus untuk trafik MQTT lokal. |
| Method Gap | [✓ ] Ya / [ ] Tidak |Kombinasi XGBoost + PSO baru diuji pada 2 jenis serangan oleh satu studi saja (Dwi Azahra & Pertiwi, 2025). Belum ada perbandingan sistematis metode ini terhadap baseline lain pada variasi serangan yang lebih beragam dan dataset yang lebih representatif. |
| Data Gap | [✓ ] Ya / [ ] Tidak |3 dari 5 studi memakai dataset publik luar negeri (CICIoT2023). Dataset simulasi MQTT lokal yang mencakup lebih dari 2 jenis serangan belum tersedia dan belum divalidasi lintas lingkungan. Ini memperlemah klaim generalisasi dari setiap studi yang ada. |
| Context Gap | [ ✓] Ya / [ ] Tidak |Belum ada studi yang secara eksplisit menguji performa deteksi DoS pada MQTT di perangkat IoT dengan sumber daya sangat terbatas (RAM rendah) sebagai konteks utama pengujian, bukan hanya sebagai asumsi di bagian diskusi. |

**Gap utama yang dipilih:**Performance Gap + Data Gap — kombinasi 2 jenis gap sesuai prinsip WS-03 bahwa gap terkuat adalah yang didukung oleh lebih dari satu jenis bukti._____________________________
**Mengapa gap ini penting (bukan sekadar "belum ada yang meneliti")?**
> Gap ini penting karena masalahnya terukur dan berdampak nyata. Ada studi yang akurat tapi terbukti lambat dengan angka 891 detik, ada yang cepat tapi akurasinya tidak cukup, dan dataset yang tersedia tidak mencerminkan kondisi lokal. Selama dua masalah ini belum terjawab bersamaan dan dibuktikan dengan eksperimen yang komprehensif, sistem deteksi DoS yang benar-benar bisa diterapkan di perangkat IoT nyata di lapangan belum akan terwujud. Gap ini juga langsung terhubung dengan root cause yang sudah diidentifikasi di WS-02 — yaitu kesenjangan antara kebutuhan keamanan real-time dan keterbatasan sumber daya perangkat IoT.___________________________________________________

---

## Latihan 3 — Baseline Selection

Pilih 2 baseline dari literatur yang sudah dibaca.

| # | Baseline | Mengapa Relevan | Mengapa Representatif | Apakah SOTA? | Sumber |
|---|----------|----------------|----------------------|-------------|--------|
| 1 | Logistic Regression + RFE | Menyelesaikan task yang sama: deteksi DoS pada IoT dengan seleksi fitur RFE menggunakan dataset CICIoT2023 yang mencakup serangan DoS | Digunakan sebagai pembanding di hampir semua studi klasifikasi trafik jaringan IoT; mewakili lower bound yang diakui komunitas peneliti |Bukan SOTA, tapi merupakan common practice — cocok sebagai batas bawah untuk mengukur seberapa besar keunggulan XGBoost+PSO | Primadya et al. (2024), Jurnal Eksplora Informatika, doi: 10.30864/eksplora.v13i2.1065 |
| 2 |Random Forest (tanpa seleksi fitur) |Digunakan langsung untuk mendeteksi serangan pada protokol MQTT — task yang identik dengan penelitian ini|Muncul di 4 dari 5 paper; konsisten menjadi acuan perbandingan di literatur deteksi intrusi IoT; mewakili state-of-the-practice yang paling banyak direplikasi |Bukan SOTA terbaru, tapi paling banyak diakui keandalannya — pemilihan ini adil karena mewakili praktik standar yang digunakan mayoritas peneliti |Akbar (2023), Skripsi Teknik Informatika, UIN Malang, etheses.uin-malang.ac.id/51427 |

**Apakah pemilihan baseline ini bisa dianggap straw man?** [ ] Ya / [ ✓ ] Tidak
> Justifikasi: Kedua baseline dipilih bukan karena lemah, melainkan karena mewakili dua level praktik yang berbeda dan keduanya muncul langsung dari literatur yang sudah dibaca. Logistic Regression mewakili pendekatan paling ringan yang masih relevan sebagai lower bound, sementara Random Forest mewakili praktik standar yang dipakai mayoritas peneliti. Membandingkan XGBoost+PSO terhadap keduanya memberikan gambaran yang jujur: apakah kompleksitas tambahan dari PSO benar-benar memberikan manfaat nyata? Jika XGBoost+PSO ternyata tidak lebih baik, itu tetap temuan yang valid dan harus dilaporkan — sesuai prinsip research mindset dari WS-01.________________________________________

---

## Refleksi

> Apa perbedaan antara "belum ada yang meneliti ini" (klaim tanpa bukti) dengan research gap yang valid? Bagaimana cara membuktikan bahwa sebuah gap benar-benar ada?

**Jawaban:**
> Klaim "Klaim "belum ada yang meneliti ini" itu sebenarnya berbahaya kalau tidak disertai bukti pencarian yang sistematis. Bisa saja penelitinya yang tidak tahu cara mencari, bukan berarti topiknya memang kosong. Research gap yang valid berbeda karena bukan sekadar topik yang belum pernah disentuh, melainkan ada masalah konkret yang teridentifikasi langsung dari literatur yang sudah ada. Contohnya seperti yang ditemukan di atas: ada studi yang akurasinya bagus tapi terbukti terlalu lambat dengan angka nyata 891 detik, ada yang cepat tapi akurasinya tidak cukup di angka 93,80%, dan dataset yang dipakai mayoritas peneliti tidak merepresentasikan kondisi jaringan MQTT lokal.
Cara membuktikan gap itu benar-benar ada ada tiga langkah. Pertama, dokumentasikan query pencarian secara eksplisit beserta database yang dipakai agar siapapun bisa memverifikasi sendiri. Kedua, tunjukkan paper-paper yang paling dekat dengan topik dan jelaskan di mana tepatnya paper itu berhenti — di situlah titik gap-nya. Ketiga, hubungkan gap yang ditemukan kembali ke root cause yang sudah diidentifikasi sebelumnya, sehingga gap bukan berdiri sendiri melainkan merupakan bukti bahwa masalah yang sudah didiagnosis di WS-02 memang belum terpecahkan oleh penelitian yang ada.__________________________________________________

>
1. Dwi Azahra, A. & Pertiwi, K.M.D. (2025). Jurnal Algoritma, 22(2), 2272–2282. https://jurnal.itg.ac.id/index.php/algoritma/article/view/2623
2. Primadya, N.D. et al. (2024). Jurnal Eksplora Informatika, 13(2), 245–252. https://doi.org/10.30864/eksplora.v13i2.1065
3. Samsudiat & Ramli (2025). JNTETI, 14(3), 216–225. https://doi.org/10.22146/jnteti.v14i3.19764
4. Jumaidi et al. (2025). IPSSJ, 2(2). https://ipssj.com/index.php/ojs/article/view/323
5. Akbar, G.M.I. (2023). Skripsi Teknik Informatika, UIN Maulana Malik Ibrahim Malang. http://etheses.uin-malang.ac.id/51427/___________________________________________________
