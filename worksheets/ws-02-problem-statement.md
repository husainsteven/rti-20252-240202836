# WS-02: Problem Statement

> **Bab 2 — Problem Formulation & System Context**

---

## Ringkasan Materi

### Problem Formation Model

Masalah riset melewati 5 tahap transformasi. Melompat langsung dari Reality ke Variable adalah kesalahan paling umum.

```
Reality → Observed Issue (Symptom) → Diagnosed Problem (Root Cause)
→ Researchable Problem (Scoped) → Measurable Variable (Operationalized)
```

### Topic ≠ Problem ≠ Research Problem

| Level | Contoh | Status |
|-------|--------|--------|
| **Topik** | Keamanan IoT | Terlalu luas, tidak bisa diuji |
| **Problem** | MQTT tidak terenkripsi | Spesifik tapi belum riset |
| **Research Problem** | Belum ada studi membandingkan overhead TLS 1.3 vs DTLS pada MQTT di IoT RAM < 64KB | Bisa dirancang eksperimennya |

### Symptom vs Root Cause

Apa yang diamati (gejala) ≠ mengapa terjadi (akar masalah). Gunakan **5 Whys** atau **Fishbone Diagram** untuk menggali.

Contoh: "User meninggalkan checkout" (symptom) → "Waktu loading > 8 detik karena API call sequential" (root cause).

### System Thinking

Setiap masalah riset TI harus terikat pada komponen sistem: **Input → Process → Output → Outcome → Constraints → Stakeholders**.

### Problem Quality Check

Masalah riset yang layak harus memenuhi 5 kriteria:
- **Clarity** — Satu orang membaca akan paham
- **Measurability** — Ada metrik kuantitatif
- **Relevance** — Penting untuk domain
- **Testability** — Bisa gagal (falsifiable)
- **Impact** — Ada kontribusi jika terjawab

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Menyelesaikan masalah (*solve*) | Memahami dan membuktikan (*understand & prove*) |
| Masalah | Bug, error, fitur belum ada | Gap dalam pengetahuan |
| Scope | Selesaikan semua yang perlu | Batasi agar bisa dibuktikan |
| Output | Working system | Evidence, paper, replicable findings |

### Istilah Penting

- **Problem Statement** — Formulasi tertulis: konteks sistem + gap + dampak + justifikasi
- **System Context** — Deskripsi lengkap: input, proses, output, outcome, constraints, stakeholders
- **Problem Drift** — Masalah "bermutasi" dari pendahuluan ke metodologi karena statement awal tidak presisi
- **Solution-First Thinking** — Memulai dari solusi tanpa masalah yang jelas — berbahaya dalam riset
- **Operational Definition** — Definisi variabel yang cukup jelas agar peneliti lain bisa mengukur hal yang sama

---

## Template A.2 — Problem Statement Builder

```
PROBLEM STATEMENT BUILDER

Domain & Konteks
  Domain   : Keamanan Jaringan Internet of Things (IoT)____________________
  Konteks  : Perangkat IoT yang berkomunikasi menggunakan protokol
             MQTT rentan terhadap serangan Denial of Service (DoS).
             Protokol MQTT dipilih karena ringan dan efisien, namun
             tidak memiliki mekanisme keamanan bawaan yang kuat.____________________

System Context
  Input       : Trafik jaringan MQTT dari perangkat IoT (data paket
                normal + paket serangan DoS: SYN Flood & MQTT Attack)____________________
  Process     : Klasifikasi trafik menggunakan algoritma machine
                learning (XGBoost + seleksi fitur PSO)
  Output      : Label klasifikasi: Normal / SYN Flood / MQTT Attack____________________
  Outcome     : Sistem mampu mendeteksi serangan DoS secara akurat
                dan efisien di jaringan IoT berbasis MQTT____________________
  Constraints : Perangkat IoT memiliki keterbatasan RAM dan daya
                komputasi; model harus ringan dan cepat____________________
  Stakeholders: Pengembang sistem IoT, administrator jaringan,
                pengguna smart home/industri, BSSN (keamanan siber
                nasional)____________________

Fenomena → Problem
  Fenomena yang diamati             : Meningkatnya serangan siber terhadap
                                perangkat IoT yang menggunakan protokol
                                MQTT di lingkungan smart home dan industri____________________
  Gejala (symptom) yang terukur     : Broker MQTT mengalami overload dan
                                perangkat IoT tidak dapat berkomunikasi
                                saat terjadi serangan DoS; latensi
                                meningkat drastis dan koneksi terputus____________________
  Masalah yang didiagnosis          : Protokol MQTT tidak memiliki mekanisme
                                deteksi serangan bawaan; metode deteksi
                                berbasis rule konvensional tidak mampu
                                mengenali pola serangan baru yang dinamis____________________
  Masalah riset (researchable)      : Belum ada studi yang membandingkan
                                efektivitas XGBoost + PSO vs metode lain
                                untuk deteksi DoS pada trafik MQTT nyata
                                dari simulasi lingkungan IoT lokal____________________
  Variabel yang terukur             : Accuracy, Precision, Recall, F1-Score,
                                waktu pelatihan model (detik per fold),
                                jumlah fitur yang dipilih PSO____________________

Problem Quality Check
  [ X ] Clarity — Apakah satu orang membaca akan paham?
  [ X ] Measurability — Apakah ada metrik kuantitatif?
  [ X ] Relevance — Apakah penting untuk domain?
  [ X ] Testability — Apakah bisa gagal?
  [ X ] Impact — Apakah ada kontribusi jika terjawab?

Problem Statement (1 paragraf):
  Perangkat IoT yang menggunakan protokol MQTT rentan terhadap
  serangan Denial of Service (DoS) karena protokol ini tidak
  dilengkapi mekanisme deteksi ancaman bawaan. Metode deteksi
  berbasis aturan (rule-based) yang ada saat ini tidak mampu
  mengenali pola serangan baru yang terus berkembang. Diperlukan
  pendekatan machine learning yang tidak hanya akurat, tetapi juga
  efisien secara komputasi agar dapat diterapkan pada perangkat IoT
  yang memiliki keterbatasan sumber daya. Penelitian ini menguji
  apakah kombinasi XGBoost dengan seleksi fitur berbasis PSO dapat
  menghasilkan sistem deteksi serangan DoS yang akurat (≥99%) dan
  efisien (waktu pelatihan <1 detik per fold) pada dataset trafik
  MQTT yang dihasilkan dari simulasi nyata.____________________
```

---

## Latihan 1 — Dari Topik ke Masalah Riset

Pilih satu topik di bidang TI yang diminati. Transformasikan melalui 5 tahap Problem Formation Model.

**Topik awal:** Keamanan Jaringan IoT________________________________________

| Tahap | Hasil |
|-------|-------|
| Reality | Jutaan perangkat IoT (smart home, sensor industri) saling terhubung menggunakan protokol MQTT setiap hari. Teknologi IoT telah banyak diterapkan di berbagai bidang seperti rumah pintar dan sistem keamanan, namun seiring meningkatnya jumlah perangkat yang terhubung, risiko terhadap serangan siber juga semakin besar Itg |
| Observed Issue (Symptom) | Salah satu jenis serangan yang umum terjadi adalah Denial of Service (DoS), yaitu upaya membanjiri sistem dengan lalu lintas data berlebih sehingga mengganggu komunikasi antarperangkat Itg. Broker MQTT mengalami overload, perangkat tidak responsif, koneksi terputus |
| Diagnosed Problem (Root Cause) | Serangan ini kerap memanfaatkan protokol MQTT, yang populer di lingkungan IoT karena sifatnya yang ringan dan efisien Itg — namun justru karena ringannya, protokol ini tidak memiliki mekanisme autentikasi dan deteksi serangan yang kuat. Sistem deteksi berbasis aturan statis tidak mampu mengenali variasi serangan baru |
| Researchable Problem | Metode deep learning seperti LSTM dan GRU membutuhkan sumber daya komputasi besar sehingga kurang efisien untuk implementasi nyata Itg pada perangkat IoT terbatas. Belum jelas apakah XGBoost + PSO dapat menjadi alternatif yang lebih efisien tanpa mengorbankan akurasi deteksi |
| Measurable Variable | Variabel independen: algoritma (XGBoost+PSO vs baseline). Variabel dependen: accuracy, precision, recall, F1-score, dan waktu pelatihan per fold (detik) |

**Apakah terjebak solution-first thinking?** [ ] Ya / [ ] Tidak
> Jika ya, kembali ke tahap mana? Tidak terjebak — masalah diidentifikasi terlebih dahulu (protokol MQTT rentan, metode lama tidak efisien), baru kemudian XGBoost+PSO diusulkan sebagai instrumen pengujian, bukan sebagai solusi yang langsung diasumsikan benar.________________________

---

## Latihan 2 — System Context Decomposition

Gambarkan konteks sistem dari masalah riset di Latihan 1.

| Komponen | Deskripsi |
|----------|----------|
| Input | Paket trafik jaringan MQTT yang ditangkap secara real-time menggunakan Wireshark dari simulasi lingkungan IoT lokal. Dataset realistis berjumlah 1.054.817 rekaman, terdiri dari 7.200 data normal, 7.200 SYN Flood, dan 1.040.417 MQTT Attack, diperoleh melalui perekaman lalu lintas dengan Wireshark ResearchGate |
| Process | Preprocessing data (pembersihan, encoding, normalisasi) → seleksi fitur menggunakan PSO → klasifikasi menggunakan XGBoost → validasi menggunakan Stratified K-Fold Cross Validation |
| Output | Label klasifikasi untuk setiap paket: Normal, SYN Flood, atau MQTT Attack. Model XGBoost dengan 11 fitur hasil seleksi PSO menunjukkan akurasi rata-rata 99,89%, precision 94,44%–96,80%, recall 99,63%–99,97%, dan F1-score 96,95%–97,81% Itg |
| Outcome | Sistem deteksi serangan DoS yang akurat dan ringan, sehingga dapat diimplementasikan pada perangkat IoT dengan sumber daya terbatas untuk melindungi broker MQTT dari serangan siber |
| Constraints | Perangkat IoT memiliki keterbatasan RAM, daya komputasi, dan bandwidth. Model harus memiliki waktu inferensi rendah. Waktu pelatihan per fold hanya 0,16–0,24 detik ResearchGate, yang sangat sesuai untuk lingkungan IoT |
| Stakeholders | Pengembang sistem IoT; administrator jaringan; pengguna smart home dan industri; BSSN (Badan Siber dan Sandi Negara) sebagai regulator keamanan siber nasional |

**Komponen mana yang paling relevan dengan masalah riset?**  karena inti riset adalah menguji apakah kombinasi seleksi fitur PSO + XGBoost menghasilkan deteksi yang lebih akurat dan efisien. Perubahan pada proses inilah yang diukur dampaknya terhadap Output (akurasi & kecepatan)._______________

---

## Latihan 3 — Problem Quality Check

Evaluasi problem statement yang sudah dibuat menggunakan 5 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Clarity | 5 | Masalah sangat spesifik: serangan DoS pada protokol MQTT di jaringan IoT, bukan sekadar "keamanan IoT secara umum". Peneliti lain dapat memahami dan mereplikasi tanpa ambiguitas |
| Measurability | 5 | Ada metrik kuantitatif yang jelas: accuracy, precision, recall, F1-score, dan waktu pelatihan per fold dalam satuan detik |
| Relevance | 5 | Sangat relevan — serangan DoS pada IoT merupakan ancaman nyata dan terus meningkat. MQTT adalah protokol dominan di smart home dan industri Indonesia |
| Testability | 4 | Bisa gagal: jika XGBoost+PSO tidak lebih unggul dari LSTM/GRU atau SVM dalam hal akurasi, hipotesis terbantahkan. Skor 4 karena pengujian hanya pada dataset simulasi lokal, belum pada jaringan produksi nyata |
| Impact | 4 | Kontribusi nyata jika terjawab: tersedia metode deteksi DoS yang ringan untuk IoT terbatas. Skor 4 karena generalisasi ke jenis serangan lain (misal: ransomware IoT) belum dieksplorasi |

**Skor total:** 23_____ / 25

**Problem statement versi final (1 paragraf):**
> Protokol MQTT yang banyak digunakan pada perangkat IoT di lingkungan smart home dan industri tidak memiliki mekanisme perlindungan bawaan terhadap serangan Denial of Service (DoS). Metode deteksi konvensional berbasis aturan terbukti tidak efektif menghadapi variasi serangan baru, sementara metode deep learning seperti LSTM membutuhkan komputasi besar yang tidak sesuai dengan keterbatasan perangkat IoT. Penelitian ini merumuskan masalah: apakah algoritma XGBoost yang dikombinasikan dengan seleksi fitur PSO dapat menghasilkan sistem deteksi serangan DoS pada jaringan MQTT dengan akurasi tinggi (≥99%) sekaligus efisiensi komputasi rendah (waktu pelatihan <1 detik per fold)? Jawaban atas pertanyaan ini berpotensi menghasilkan model IDS yang praktis dan dapat diimplementasikan langsung pada perangkat IoT terbatas sumber daya.___________________________________________________
> ___________________________________________________

---

## Refleksi

> Bandingkan "masalah" yang biasa ditemui saat coding (bug, error) dengan masalah riset. Apa perbedaan fundamental dalam cara mendefinisikan dan mendekati keduanya?

**Jawaban:**
> Masalah coding (bug/error): Masalahnya jelas terlihat — program crash, output salah, error message muncul. Solusinya pun ada: debug, cek log, fix kode. Tujuannya adalah membuat sistem berjalan (engineering mindset: "bagaimana membuatnya jalan?"). Tidak perlu membuktikan mengapa sesuatu gagal secara ilmiah — cukup berhasil diperbaiki.
Masalah riset: Masalahnya tidak selalu terlihat langsung — harus didiagnosis melalui pengamatan fenomena, literatur, dan analisis sistem. Solusinya belum tentu ada, dan bahkan kegagalan (hasil negatif) pun merupakan temuan yang valid. Tujuannya bukan hanya membuat sistem bekerja, melainkan membuktikan secara terukur mengapa suatu pendekatan lebih baik dari yang lain (research mindset: "apakah klaim ini benar?"). Dalam kasus di atas: bukan sekadar "buat sistem deteksi DoS yang jalan", tapi "buktikan dengan data bahwa XGBoost+PSO lebih akurat dan efisien dibanding metode lain pada kondisi jaringan IoT nyata."

___________________________________________________
> Dwi Azahra, A. & Pertiwi, K. M. D. (2025). Deteksi Serangan DoS pada IoT Berbasis MQTT Menggunakan XGB dan PSO. Jurnal Algoritma, 22(2), 2272–2282. Institut Teknologi Garut (ITG).
Tersedia di: https://jurnal.itg.ac.id/index.php/algoritma/article/view/2623
___________________________________________________
