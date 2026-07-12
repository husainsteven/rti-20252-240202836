# WS-16: Presentation & Defense (UAS)

> **Bab 16 — Presentasi & Pertahanan Ilmiah**

---

## Ringkasan Materi

### Scientific Defense Model

```
Research Work → Presentation → Questioning → Defense → Evaluation → Acceptance
```

### Presentasi ≠ Ringkasan Paper

| Paper | Presentasi |
|-------|-----------|
| Dibaca (self-paced) | Didengar (presenter-paced) |
| Detail lengkap | Ide kunci + highlight |
| Tabel numerik detail | Grafik visual + angka kunci |
| Pembaca bisa re-read | Audiens dengar sekali |

**Prinsip:** Presentasi membutuhkan **reformulasi**, bukan kompresi. Medium berbeda = pendekatan berbeda.

### Claim-Evidence-Reasoning (CER)

Setiap jawaban defense harus memiliki:
1. **Claim** — Pernyataan yang dijawab
2. **Evidence** — Data/fakta pendukung
3. **Reasoning** — Logika yang menghubungkan evidence ke claim

**Contoh:**
| Pertanyaan | Bad Answer | Good Answer (CER) |
|-----------|-----------|-------------------|
| "Kenapa hanya 3 dataset?" | "Tiga sudah cukup" | "3 dataset mewakili variasi: small-clean, medium-clean, medium-noisy [E]. Generalisasi perlu validasi lanjut — listed as limitation [R]" |
| "Hasil DS-3 menurun?" | "Itu outlier" | "Ya, karena distribusi heavy-tail melanggar asumsi Gaussian [E]. Ini menunjukkan boundary condition metode [R]" |
| "Effect size?" | "p=0.003, jadi signifikan" | "Cohen's d=1.2 (large effect) [E] — bukan hanya signifikan tapi substansial [R]" |

### Slide Design — One Slide, One Message

**Optimal 9-Slide Plan (15 menit):**

| # | Slide | Waktu | Pesan |
|---|-------|-------|-------|
| 1 | Title + context | 1 min | Apa ini tentang apa |
| 2 | Problem + motivation | 2 min | Mengapa penting |
| 3 | Gap + RQ | 1.5 min | Apa yang belum terjawab |
| 4 | Method overview | 2 min | Bagaimana dijawab (diagram) |
| 5 | Key result — tabel | 2 min | Temuan utama |
| 6 | Key result — grafik | 2 min | Pola visual |
| 7 | Interpretation + failure | 2 min | Apa artinya |
| 8 | Limitation + future | 1.5 min | Batasan & arah |
| 9 | Conclusion + contribution | 1 min | Closing message |

### Anticipatory Defense

Prediksi pertanyaan berdasarkan kategori:

| Kategori | Contoh Pertanyaan |
|---------|------------------|
| Problem | "Mengapa masalah ini penting?" |
| Gap | "Bagaimana dengan studi X yang sudah menjawab ini?" |
| Method | "Mengapa metode ini, bukan Y?" |
| Results | "Bagaimana menjelaskan anomali di DS-3?" |
| Generalization | "Apakah bisa diterapkan di domain lain?" |

### Tiga Prinsip Jawaban

1. **Direct** — Jawab dulu, elaborasi kemudian
2. **Data-based** — Tunjuk evidence spesifik
3. **Honest** — Akui limitasi jika memang ada

### Jebakan Kognitif

1. "Presentasi = semua yang ada di paper" → terlalu padat
2. "Slide cantik = presentasi bagus" → konten > estetika
3. "Tidak bisa jawab = gagal" → "I don't know, but..." menunjukkan kejujuran
4. "Tidak perlu latihan — saya paham riset saya" → latihan = menemukan celah

---

## Template A.16 — Defense Preparation Sheet

```
DEFENSE PREPARATION

Slide Deck Plan:
  Total slides   : ____ (target: 10-12 konten + title/closing)
  Time per slide : ~2 min
  Total time     : ____ menit

Slide Outline:
| # | Pesan Utama | Visual | Waktu |
|---|-------------|--------|-------|
| 1 | Title       |        | 30s   |
| 2 | Problem     |        | 2min  |
| 3 | Gap + RQ    |        | 2min  |
| ..|             |        |       |

Anticipatory Defense Matrix:
| Kategori | Pertanyaan Potensial | Jawaban (CER) |
|----------|---------------------|---------------|
| Problem  |                     |               |
| Gap      |                     |               |
| Method   |                     |               |
| Results  |                     |               |
| Generalization |               |               |

Latihan:
  Latihan 1: [tanggal] — [catatan timing & feedback]
  Latihan 2: [tanggal] — [catatan timing & feedback]
  Latihan 3: [tanggal] — [catatan timing & feedback]
```

---

## Latihan 1 — Slide Outline

Rencanakan presentasi 15 menit untuk riset Anda.

| # | Pesan Utama | Visual yang Digunakan | Waktu |
|---|-------------|----------------------|-------|
| 1 | Judul penelitian dan identitas peneliti | Title slide + logo universitas | 1 min |
| 2 | Latar belakang pentingnya deteksi DoS pada IoT berbasis MQTT | Diagram IoT dan ilustrasi serangan DoS | 2 min |
| 3 | Research gap, rumusan masalah, dan tujuan penelitian | Tabel research gap |  1.5 min |
| 4 | Metodologi penelitian | Flowchart penelitian | 2 menit |
| 5 | Dataset dan preprocessing | Diagram preprocessing | 2 menit |
| 6 | Hasil evaluasi model | Tabel dan grafik Accuracy, Precision, Recall, F1-Score | 2 menit |
| 7 | Pembahasan hasil penelitian | Grafik perbandingan model | 2 menit |
| 8 | Keterbatasan dan penelitian selanjutnya | Diagram limitation & future work | 1,5 menit |
| 9 | Kesimpulan dan kontribusi penelitian | Ringkasan poin utama | 1 menit |

**Total waktu estimasi:** 15 menit

---

## Latihan 2 — Anticipatory Defense

Prediksi 5 pertanyaan yang mungkin diajukan penguji, lalu siapkan jawaban CER.

| # | Kategori | Pertanyaan | Claim | Evidence | Reasoning |
|---|----------|-----------|-------|----------|-----------|
| 1 | Problem | Mengapa fokus pada serangan DoS di jaringan IoT? | DoS merupakan ancaman penting pada IoT. | MQTT memiliki keterbatasan keamanan sehingga rentan terhadap DoS. | Penelitian diperlukan untuk meningkatkan kemampuan deteksi serangan. |
| 2 | Method | Mengapa memilih XGBoost? | XGBoost memiliki performa klasifikasi yang tinggi. | XGBoost menggunakan teknik boosting dan regularisasi yang meningkatkan akurasi model. | Model dipilih karena sesuai untuk klasifikasi data serangan jaringan. |
| 3 | Method | Mengapa menggunakan PSO? | PSO mampu mengoptimalkan hyperparameter secara efisien. | PSO mencari kombinasi parameter terbaik melalui iterasi partikel. | Parameter optimal dapat meningkatkan performa XGBoost |
| 4 | Results | Bagaimana membuktikan model lebih baik? | Performa dievaluasi menggunakan metrik standar. | Accuracy, Precision, Recall, dan F1-Score dibandingkan dengan model pembanding. | Perbandingan dilakukan secara objektif menggunakan hasil evaluasi dan analisis statistik. |
| 5 | Generalization | Apakah hasil dapat diterapkan pada dataset lain? | Potensi ada, tetapi perlu validasi lanjutan. | Penelitian hanya menggunakan satu dataset.  | Pengujian pada dataset lain diperlukan untuk memastikan kemampuan generalisasi. |

---

## Latihan 3 — Simulasi Q&A

Minta teman/kolega mengajukan 3 pertanyaan tentang riset Anda. Catat pertanyaan dan evaluasi jawaban Anda.

| # | Pertanyaan | Jawaban Saya | Evaluasi |
|---|-----------|-------------|---------|
| 1 | Mengapa menggunakan PSO dibandingkan Genetic Algorithm? | PSO dipilih karena memiliki proses optimasi yang lebih sederhana, jumlah parameter lebih sedikit, dan mampu menemukan solusi optimal dengan waktu komputasi yang efisien. | [✓] Direct [✓] Data-based [✓] Honest |
| 2 | Bagaimana jika dataset tidak seimbang? | Penelitian dapat menggunakan teknik penyeimbangan data seperti SMOTE atau evaluasi menggunakan F1-Score agar hasil lebih representatif. | [ ✓ ] Direct [ ✓ ] Data-based [ ✓ ] Honest |
| 3 | Bagaimana implementasi penelitian pada dunia nyata? | Model dapat diintegrasikan ke dalam Intrusion Detection System (IDS) untuk memantau lalu lintas MQTT dan mendeteksi indikasi serangan DoS secara otomatis. | [ ✓ ] Direct [ ✓ ] Data-based [ ✓ ] Honest |

**Pertanyaan yang paling sulit dijawab:**
> Bagaimana membuktikan bahwa metode XGBoost + PSO tetap memiliki performa yang baik ketika diuji pada dataset IoT lain yang memiliki karakteristik berbeda?

**Apa yang perlu disiapkan lebih baik:**
> Menyiapkan referensi penelitian terdahulu yang menggunakan dataset berbeda, memperdalam pemahaman mengenai proses optimasi PSO, serta menyiapkan analisis terkait keterbatasan penelitian dan peluang pengembangan pada penelitian selanjutnya.
---

## Refleksi

> Dari seluruh proses WS-01 sampai WS-16 — dari paradigma riset hingga presentasi — bagian mana yang paling mengubah cara Anda berpikir tentang riset? Apa satu hal yang akan selalu Anda terapkan di riset berikutnya?

**Insight terbesar:**
> Selama mengikuti WS-01 hingga WS-16, saya memahami bahwa penelitian bukan hanya menghasilkan model dengan performa yang baik, tetapi juga menyusun argumen ilmiah yang didukung oleh metodologi yang tepat, analisis yang objektif, dan kemampuan mempertahankan hasil penelitian melalui presentasi serta sesi tanya jawab.

**Yang akan selalu diterapkan:**
> Pada penelitian berikutnya saya akan selalu memulai dengan identifikasi research gap yang jelas, memilih metode yang sesuai dengan permasalahan, melakukan evaluasi menggunakan analisis statistik yang tepat, serta mempersiapkan presentasi dan jawaban berdasarkan pendekatan Claim–Evidence–Reasoning (CER) agar setiap kesimpulan dapat dipertanggungjawabkan secara ilmiah.
