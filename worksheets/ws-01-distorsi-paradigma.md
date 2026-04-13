# ws-01: Distorsi & Paradigma

> **Bab 1 — Research Mindset in IT**

---

## Ringkasan Materi

### Research Trust Model

Pengetahuan ilmiah tidak muncul langsung dari kenyataan. Ia melewati **6 tahap transformasi** yang masing-masing rawan distorsi:

```
Reality → Data → Processing → Analysis → Inference → Knowledge
```

Etika mencegah distorsi yang disengaja (fabrikasi, cherry-picking). Validitas mendeteksi distorsi yang tidak disengaja (confounding variable, sampling bias).

### Tiga Jenis Validitas

| Jenis | Pertanyaan | Contoh Ancaman |
|-------|-----------|----------------|
| **Internal Validity** | Apakah hubungan kausal benar ada? | Confounding variable |
| **External Validity** | Apakah bisa digeneralisasi? | Dataset terlalu homogen |
| **Construct Validity** | Apakah mengukur hal yang benar? | Metrik tidak sesuai klaim |

### Paradigma Riset

Mata kuliah ini menggunakan pendekatan **Positivist** (fenomena TI bisa diukur objektif melalui eksperimen terkontrol) diperkuat **Design Science Research** (artefak dibuat sebagai instrumen pengujian hipotesis, bukan tujuan akhir).

### Mode Berpikir Peneliti

**Curious** (mempertanyakan fenomena) → **Critical** (mengevaluasi klaim berdasarkan bukti) → **Systematic** (merancang investigasi terstruktur dan reproducible).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Membuat sistem yang bekerja | Menghasilkan pengetahuan yang valid |
| Pertanyaan khas | "Bagaimana membuatnya jalan?" | "Apakah klaim ini benar?" |
| Ukuran sukses | Sistem berfungsi, client puas | Hipotesis terjawab, temuan tervalidasi |
| Kegagalan | Harus dihindari | Harus dilaporkan (negative result = kontribusi) |

### Istilah Penting

- **Research Mindset** — Pola pikir yang menuntut bukti dan mempertanyakan asumsi
- **Research Ethics** — Prinsip perilaku: kejujuran, objektivitas, keterbukaan, akuntabilitas
- **HARKing** — Hypothesizing After Results are Known — merumuskan hipotesis setelah melihat data
- **Falsifiability** — Hipotesis harus bisa dibuktikan salah

---

## Template A.1 — Research Mindset Self-Assessment

```
Nama Peneliti    : __Husain Stefano__________________
Tanggal          : ____10 April 2026________________

1. Ketika membaca klaim "metode X 95% akurat":
   - Pertanyaan pertama saya: ___Dataset apa yang digunakan? Apakah data
     uji benar-benar terpisah dari data latih? Kelas serangan apa yang
     paling banyak di dataset tersebut?_________________
   - Data yang dibutuhkan untuk verifikasi: __Distribusi kelas dalam
     dataset KDD Cup 99, confusion matrix per kelas, waktu inferensi
     di lingkungan jaringan nyata, dan apakah eksperimen bisa
     direproduksi oleh peneliti lain.__________________

2. Posisi paradigma:
   - Pendekatan: [ ] Positivis  [ ] Interpretivis  [ ] Design Science  [ ] Mixed
   - Alasan: ___Paper mengukur performa secara objektif melalui angka
     akurasi, AUC, dan waktu pelatihan — sesuai positivisme. Sekaligus
     membangun artefak (model SVM & ANN) untuk menguji hipotesis
     perbandingan — sesuai Design Science Research.
_________________

3. Identifikasi distorsi:
   - Asumsi tersembunyi: _____Dataset KDD Cup 99 dianggap cukup representatif
     untuk jaringan komputer masa kini, padahal dataset ini dibuat
     tahun 1999._______________
   - Sumber bias potensial: __________ Kelas trafik normal jauh lebih banyak dari
     kelas serangan (class imbalance), sehingga akurasi tinggi bisa
     dicapai hanya dengan memprediksi "normal" terus.__________
   - Langkah mitigasi: ____Laporkan F1-score, precision, dan recall per
     kelas serangan; uji juga pada dataset yang lebih baru seperti
     CICIDS2017.________________

4. Komitmen etika:
   - Data yang tidak akan dimanipulasi: __Hasil confusion matrix, false
     positive rate, dan waktu komputasi harus dilaporkan apa adanya.__________________
   - Batasan yang diakui sejak awal: _______ Model hanya diuji pada satu
     dataset lama; hasilnya belum tentu berlaku pada trafik jaringan
     nyata masa kini._____________
```

---

## Latihan 1 — Identifikasi Distorsi

Pilih satu paper riset di bidang TI yang mengklaim "metode X meningkatkan performa." Telusuri setiap tahap Research Trust Model.

**Paper yang dipilih:**
> Judul: ______________ Studi Perbandingan Deteksi Intrusi Jaringan Menggunakan Machine Learning: (Metode SVM dan ANN)_________________________________
> Penulis (Tahun): ______Tan, T., Sama, H., Wijaya, G., & Aboagye, O. E. (2023)
>  https://ojs.unikom.ac.id/index.php/jati/article/view/10484
________________________________

| Tahap | Apa yang Dilakukan | Potensi Distorsi |
|-------|-------------------|-----------------|
| Reality → Data | Mengunduh dataset KDD Cup 99 dari repositori publik sebagai representasi trafik jaringan nyata| Dataset sudah berumur 25+ tahun (dibuat 1999). Jenis serangan modern seperti ransomware, zero-day exploit, dan serangan terenkripsi tidak ada di dalamnya. Tidak merepresentasikan realita ancaman siber masa kini |
| Data → Processing | Melatih dan menguji SVM dan ANN di Google Colaboratory menggunakan dataset tersebut | Tidak disebutkan apakah dilakukan penanganan class imbalance. Jika trafik normal jauh lebih banyak dari serangan, model bisa mendapat akurasi tinggi hanya dengan selalu memprediksi "normal" |
| Processing → Analysis | Membandingkan akurasi, waktu pelatihan, ROC curve, dan kecepatan jaringan | Parameter perbandingan yang digunakan adalah skor akurasi pelatihan dan pengujian, waktu pelatihan dan pengujian, ROC Curve, dan kecepatan jaringan Unikom — namun tidak ada laporan F1-score atau recall per kelas serangan spesifik |
| Analysis → Inference | Menyimpulkan SVM lebih unggul dari ANN berdasarkan angka akurasi keseluruhan | Akurasi keseluruhan (overall accuracy) tidak cukup untuk menilai kemampuan deteksi serangan yang jarang muncul. Seharusnya dievaluasi per kelas serangan |
| Inference → Knowledge | Mengklaim SVM sebagai metode yang lebih efektif untuk sistem deteksi intrusi secara umum | Overgeneralisasi: kesimpulan ini hanya berlaku pada dataset KDD Cup 99, bukan pada jaringan nyata atau dataset yang lebih baru. Tidak ada uji validasi eksternal |

**Distorsi paling besar di tahap:** Reality → Data (dataset sangat usang) dan Inference → Knowledge (overgeneralisasi kesimpulan)________________________

**Dua distorsi spesifik yang teridentifikasi:**
1. Construct Validity — Dataset Usang (KDD Cup 99): Dataset ini dibuat berdasarkan simulasi jaringan militer AS tahun 1998. Banyak peneliti telah membuktikan bahwa KDD Cup 99 memiliki banyak duplikasi data dan tidak mencerminkan trafik jaringan modern. Akurasi 99,87% kemungkinan besar adalah hasil dari model yang "menghafal" pola lama, bukan kemampuan deteksi intrusi sesungguhnya (construct validity — alat ukur tidak mengukur hal yang diklaim)._____________________________________________
2. External Validity — Tidak Ada Uji Generalisasi: Penelitian menggunakan metode eksperimen dengan melatih dan menguji SVM dan ANN pada Dataset KDD Cup 99 di Google Colaboratory Unikom, tanpa pengujian pada dataset lain atau lingkungan jaringan yang berbeda. Temuan tidak bisa digeneralisasi ke jaringan nyata, jaringan IoT, atau ancaman siber masa kini.___________________________________________________

---

## Latihan 2 — Analisis Kasus Etika

Skenario: Seorang peneliti menemukan bahwa jika 3 data point outlier dihapus, hasil eksperimennya menjadi signifikan. Dengan outlier, hasilnya tidak signifikan.

| Perspektif | Analisis |
|------------|---------|
| Kejujuran ilmiah | Wajib melaporkan kedua versi hasil — dengan dan tanpa outlier. Keputusan menghapus outlier harus dibuat berdasarkan kriteria yang ditentukan sebelum analisis dimulai (misalnya: error sensor atau data korup), bukan karena alasan statistik setelah melihat hasil. Dalam konteks paper ini: jika beberapa sampel serangan langka dihapus karena "mengganggu" akurasi, itu adalah manipulasi data |
| Transparansi | Semua langkah preprocessing — termasuk penghapusan data — harus dicantumkan secara eksplisit di bagian metode, lengkap dengan alasan yang dapat diverifikasi. Peneliti sebaiknya menyertakan kode eksperimen (misalnya via GitHub atau Google Colab) agar dapat direproduksi oleh peneliti lain |
| Peer review | Reviewer yang kritis akan menanyakan: "Kapan dan berdasarkan apa keputusan penghapusan outlier dibuat?" Jika dilakukan setelah melihat hasil, ini adalah HARKing (Hypothesizing After Results are Known) dan p-hacking — dua pelanggaran etika riset yang serius. Paper seharusnya ditolak atau diminta revisi mayor |

**Keputusan akhir dan justifikasi:**
> Laporkan kedua versi hasil. Penghapusan outlier hanya dapat dibenarkan secara etis apabila diputuskan sebelum analisis dan memiliki alasan teknis yang kuat — bukan karena hasilnya tidak signifikan. Hasil yang tidak signifikan tetap merupakan kontribusi ilmiah yang valid (negative result), karena mencegah peneliti lain membuang sumber daya untuk menguji hipotesis yang sama. Menyembunyikan hasil negatif adalah pelanggaran prinsip objektivitas dan kejujuran dalam penelitian.___________________________________________________

---

## Latihan 3 — Posisi Paradigma

**Topik riset:** Perbandingan efektivitas algoritma SVM dan ANN untuk Sistem Deteksi Intrusi Jaringan Komputer_______________________________________

| Kriteria | Positivis | Interpretivis | Design Science |
|----------|-----------|---------------|----------------|
| Kesesuaian dengan topik (1–5) | 5 | 1 | 4 |
| Jenis data yang dikumpulkan | Data numerik terukur: akurasi, AUC, waktu pelatihan, false positive rate dari eksperimen terkontrol menggunakan dataset benchmark | Persepsi administrator jaringan tentang seberapa "aman" sistem mereka — bersifat kualitatif dan subyektif | Artefak berupa model SVM/ANN yang dibangun, dikonfigurasi, dan diuji performanya; metriks performa digunakan sebagai validasi |
| Limitasi paradigma | Mengabaikan konteks nyata: apakah model yang akurat di lab benar-benar mudah diimplementasikan dan dioperasikan oleh tim IT? | Sangat sulit menghasilkan angka perbandingan yang objektif; tidak bisa menjawab "mana yang lebih akurat?" | Risiko engineering trap: peneliti bisa terlalu fokus membangun model sebaik mungkin, lupa bahwa tujuan utamanya adalah menghasilkan pengetahuan yang tervalidasi dan dapat digeneralisasi |

**Paradigma yang dipilih:** Positivis diperkuat Design Science Research_____________________________
**Alasan:** Pertanyaan utama paper ini — "apakah SVM lebih efektif dari ANN?" — adalah pertanyaan empiris yang hanya bisa dijawab melalui pengukuran objektif, sesuai paradigma Positivis. Namun karena peneliti juga membangun model sebagai instrumen pengujian, Design Science Research relevan sebagai pelengkap — asalkan artefak yang dibangun diperlakukan sebagai alat untuk menguji hipotesis, bukan sebagai tujuan akhir.____________________________________________

---

## Refleksi

> Sebelum membaca materi ini, apakah pernah mempertanyakan klaim "95% akurat"? Setelah memahami rantai distorsi, pertanyaan apa yang sekarang akan diajukan saat membaca paper?

**Jawaban:**
> Kemungkinan besar langsung percaya — angka mendekati 100% terasa sangat meyakinkan dan tidak ada alasan untuk meragukannya. Belum terpikir untuk bertanya: dataset mana, tahun berapa, distribusi kelasnya bagaimana?__________________________________________________
> Setelah memahami rantai distorsi, pertanyaan apa yang sekarang akan diajukan saat membaca paper?


1. Reality → Data: Dataset ini dibuat kapan dan oleh siapa? Apakah jenis serangannya masih relevan dengan ancaman masa kini?
2. Data → Processing: Apakah distribusi kelas seimbang? Bagaimana cara peneliti menangani class imbalance?
3. Processing → Analysis: Apakah hanya dilaporkan akurasi keseluruhan, atau juga F1-score dan recall per kelas serangan?
4. Analysis → Inference: Apakah kesimpulan "SVM lebih baik" hanya berlaku pada dataset ini, atau diuji juga pada dataset lain?
5. Inference → Knowledge: Apakah eksperimen ini bisa direproduksi? Apakah kode dan data tersedia secara publik?___________________________________________________
