# WS-15: Scientific Writing

> **Bab 15 — Penulisan Ilmiah**

---

## Ringkasan Materi

### Scientific Argument Flow

```
Problem → Gap → RQ → Method → Result → Analysis → Conclusion → Contribution
```

Paper ilmiah adalah **satu argumen utuh** dari masalah ke kontribusi. Setiap node harus terhubung logis ke node sebelum dan sesudahnya.

### Struktur IMRAD

| Section | Peran | Pertanyaan Kunci |
|---------|-------|-----------------|
| **Introduction** | Motivasi + frame | Why is this needed? |
| **Method** | Deskripsi (reproducible) | How was it done? |
| **Results** | Laporan objektif | What was found? |
| **Discussion** | Interpretasi + refleksi | What does it mean? |
| **Conclusion** | Ringkasan + kontribusi | So what? |

### Logical Flow — "Red Thread"

Setiap paragraf menjawab satu pertanyaan dan memicu pertanyaan berikutnya. Alur logis ini harus terasa di tiga level:
1. **Antar-kalimat** dalam paragraf
2. **Antar-paragraf** dalam section
3. **Antar-section** dalam paper

### Internal Consistency

Setiap elemen yang dijanjikan di Introduction harus hadir di Discussion/Conclusion.

**Consistency Matrix:**
```
           Intro  Method  Result  Discuss  Conclude
RQ1          ✓      ✓       ✓       ✓        ✓
RQ2          ✓      ✓       ✓       ✗ ←      ✓
Metrik-X     ✗      ✗       ✓ ←     ✗        ✗
```
**Masalah:** RQ2 dibahas di semua bagian kecuali Discussion. Metrik-X muncul di Result tapi tidak diperkenalkan di Method.

### Writing Quality Triad

| Kualitas | Deskripsi | Contoh Buruk → Baik |
|----------|----------|---------------------|
| **Clarity** | Dipahami sekali baca | "Performa meningkat" → "Accuracy meningkat dari 85.3% ke 89.7%" |
| **Precision** | Istilah eksak, tanpa ambiguitas | "signifikan" → "signifikan secara statistik (p=0.003, d=1.2)" |
| **Conciseness** | Setiap kata menambah informasi | Hapus kalimat redundan, filler words |

### Urutan Penulisan yang Disarankan

1. **Method & Results** — paling stabil, tulis pertama
2. **Discussion** — interpretasi berdasarkan hasil
3. **Introduction** — frame sesuai temuan aktual
4. **Abstract & Conclusion** — terakhir

### Target Jumlah Kata

| Section | Target |
|---------|--------|
| Introduction | 500–700 |
| Related Work | 700–1000 |
| Method | 800–1200 |
| Results | 500–800 |
| Discussion | 600–900 |
| Conclusion | 200–400 |

### Jebakan Kognitif

1. "Lebih panjang = lebih lengkap" → conciseness lebih berharga
2. "Introduction harus ditulis pertama" → justru ditulis terakhir
3. "Jargon teknis = lebih ilmiah" → clarity lebih penting
4. "Discussion = ringkasan Results" → Discussion = interpretasi + konteks

---

## Template A.15 — Paper Structure Checklist

```
PAPER STRUCTURE CHECKLIST

Title   : Perbandingan Kinerja Algoritma Random Forest dan Decision Tree untuk Klasifikasi Penyakit Jantung
Target  : [ ] Jurnal  [ ] Konferensi  [ ✓ ] Laporan

Section Check:
  [ ✓ ] Abstract — masalah, metode, hasil utama, kontribusi (max 250 kata)
  [ ✓ ] Introduction — konteks → gap → RQ → kontribusi → struktur paper
  [ ✓ ] Related Work — concept-centric, gap positioning
  [ ✓ ] Method — reproducible: desain, variabel, metrik, setup, prosedur
  [ ✓ ] Results — tabel + grafik + observasi (tanpa interpretasi)
  [ ✓ ] Discussion — interpretasi, perbandingan, implikasi, limitation
  [ ✓ ] Conclusion — jawaban RQ, kontribusi, future work

Consistency Matrix:
  [ ✓ ] RQ di Introduction = RQ di Method = RQ di Conclusion
  [ ✓ ] Variabel di Method = variabel di Results
  [ ✓ ] Klaim di Discussion didukung data di Results
  [ ✓ ] Limitasi di Discussion di-address di Conclusion/Future Work

Writing Quality:
  [ ✓ ] Clarity — mudah dipahami tanpa re-read
  [ ✓ ] Precision — tidak ada istilah ambigu
  [ ✓ ] Conciseness — tidak ada kalimat redundan
```

---

## Latihan 1 — Paper Outline

Buat outline paper untuk riset Anda menggunakan struktur IMRAD.

| Section | Konten Utama (2-3 kalimat) | Target Kata |
|---------|---------------------------|------------|
| Abstract | Penelitian ini mengusulkan optimasi algoritma XGBoost menggunakan Particle Swarm Optimization (PSO) untuk meningkatkan deteksi serangan Denial of Service (DoS) pada jaringan Internet of Things berbasis MQTT. Model dievaluasi menggunakan metrik Accuracy, Precision, Recall, dan F1-Score serta dibandingkan dengan model baseline untuk mengetahui peningkatan performanya. | 200-250 |
| Introduction | Internet of Things (IoT) semakin banyak digunakan dalam berbagai sektor, namun protokol MQTT memiliki kerentanan terhadap serangan DoS. Penelitian sebelumnya masih memiliki keterbatasan dalam optimasi model deteksi. Oleh karena itu penelitian ini mengusulkan penggunaan XGBoost dengan optimasi PSO untuk meningkatkan akurasi deteksi serangan. | 500-700 |
| Related Work | Bagian ini membahas konsep IoT, protokol MQTT, karakteristik serangan DoS, algoritma XGBoost, Particle Swarm Optimization, serta penelitian terdahulu yang relevan. Selain itu dijelaskan research gap yang menjadi dasar pengembangan metode yang diusulkan. | 700-1000 |
| Method | Penelitian menggunakan dataset MQTT-IoT-IDS2020 yang melalui tahapan preprocessing, pembagian data latih dan data uji, pelatihan model XGBoost, serta optimasi hyperparameter menggunakan PSO. Kinerja model dievaluasi menggunakan Accuracy, Precision, Recall, F1-Score, dan dibandingkan dengan model baseline menggunakan uji statistik yang sesuai. | 800-1200 |
| Results | Bagian ini menyajikan hasil evaluasi model berupa tabel dan grafik performa tanpa memberikan interpretasi. Hasil meliputi nilai Accuracy, Precision, Recall, F1-Score, waktu komputasi, serta perbandingan performa antara model usulan dan model pembanding. | 500-800 |
| Discussion | Hasil penelitian diinterpretasikan dengan menjelaskan pengaruh optimasi PSO terhadap performa XGBoost serta membandingkannya dengan penelitian terdahulu. Selain itu dibahas implikasi hasil, keterbatasan penelitian, dan peluang pengembangan pada penelitian berikutnya. | 600-900 |
| Conclusion | Bagian ini menyimpulkan apakah tujuan penelitian berhasil dicapai dan menjawab rumusan masalah yang diajukan. Kesimpulan juga memuat kontribusi penelitian serta rekomendasi untuk pengembangan metode pada penelitian selanjutnya. | 200-400 |

---

## Latihan 2 — Consistency Matrix

Buat consistency matrix untuk memverifikasi internal consistency paper Anda.

|  | Intro | Method | Result | Discussion | Conclusion |
|--|-------|--------|--------|-----------|-----------|
| RQ1 | ✓ | ✓ | ✓ | ✓ | ✓ |
| RQ2 | ✓ | ✓ | ✓ | ✓ | ✓ |
| Metrik utama | ✓ | ✓ | ✓ | ✓ | ✓ |
| Variabel IV | ✓ | ✓ | ✓ | ✓ | ✓ |
| Variabel DV | ✓ | ✓ | ✓ | ✓ | ✓ |
| Klaim/kontribusi | ✓ | ✓ | ✓ | v | v |

**Isi setiap sel:** ✓ (ada & konsisten), ✗ (missing), ~ (ada tapi inkonsisten)

**Inkonsistensi yang ditemukan:**
> Tidak ditemukan inkonsistensi yang signifikan. Rumusan masalah, metode penelitian, metrik evaluasi, hasil, pembahasan, dan kesimpulan telah disusun secara konsisten sehingga setiap tujuan penelitian didukung oleh metode dan hasil yang diperoleh.___________________________________________________

**Tindakan perbaikan:**
> Memastikan seluruh hasil evaluasi yang disajikan pada bagian Results dibahas kembali pada bagian Discussion serta memastikan seluruh keterbatasan penelitian dicantumkan pada bagian Conclusion sebagai dasar penyusunan future work.___________________________________________________

---

## Latihan 3 — Writing Quality Check

Ambil satu paragraf dari tulisan Anda (atau tulis paragraf baru) dan evaluasi kualitasnya.

**Paragraf asli:**
> Internet of Things (IoT) berkembang sangat cepat dan digunakan di berbagai bidang. Namun perkembangan tersebut juga meningkatkan risiko serangan siber, terutama serangan Denial of Service (DoS) pada protokol MQTT. Oleh karena itu diperlukan metode deteksi yang mampu memberikan performa klasifikasi yang baik.

| Kriteria | Evaluasi | Perbaikan |
|----------|---------|-----------|
| Clarity | Kalimat sudah mudah dipahami, tetapi belum menjelaskan alasan pemilihan metode penelitian. | Tambahkan penjelasan mengenai penggunaan XGBoost dan PSO sebagai solusi penelitian. |
| Precision | Istilah "performa klasifikasi yang baik" masih bersifat umum. | Ubah menjadi "meningkatkan Accuracy, Precision, Recall, dan F1-Score dalam mendeteksi serangan DoS". |
| Conciseness | Terdapat pengulangan makna pada kalimat pertama dan kedua mengenai perkembangan IoT. | Gabungkan informasi agar lebih ringkas tanpa mengurangi makna. |

**Paragraf setelah perbaikan:**
> Perkembangan Internet of Things (IoT) meningkatkan pemanfaatan protokol MQTT dalam berbagai aplikasi, namun juga memperbesar risiko serangan Denial of Service (DoS). Untuk mengatasi permasalahan tersebut, penelitian ini mengusulkan optimasi algoritma XGBoost menggunakan Particle Swarm Optimization (PSO) guna meningkatkan Accuracy, Precision, Recall, dan F1-Score dalam mendeteksi serangan DoS pada jaringan IoT berbasis MQTT.

---

## Refleksi

> Apa perbedaan antara menulis "tentang" riset dan menulis sebagai "argumen" riset? Bagaimana urutan penulisan (Method → Discussion → Introduction) mengubah kualitas tulisan?

> Menulis tentang riset hanya menjelaskan aktivitas penelitian yang dilakukan, sedangkan menulis sebagai argumen riset bertujuan membangun alur logis yang menunjukkan mengapa penelitian perlu dilakukan, bagaimana metode yang dipilih dapat menjawab rumusan masalah, serta bagaimana hasil penelitian mendukung kesimpulan yang diambil. Oleh karena itu, setiap bagian dalam paper harus saling terhubung sehingga pembaca dapat mengikuti alur penelitian secara sistematis.

Urutan penulisan Method → Results → Discussion → Introduction membantu meningkatkan kualitas tulisan karena peneliti terlebih dahulu menyusun metode dan hasil yang benar-benar diperoleh. Setelah itu, pembahasan dapat disusun berdasarkan bukti yang tersedia, sedangkan pendahuluan dapat ditulis terakhir agar selaras dengan tujuan, metode, dan temuan penelitian. Pendekatan ini menghasilkan paper yang lebih konsisten, logis, dan mudah dipahami.___________________________________________________
> ___________________________________________________
