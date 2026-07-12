# WS-14: Analysis, Interpretation & Failure Analysis

> **Bab 14 — Analisis Data, Interpretasi & Failure Analysis**

---

## Ringkasan Materi

### Data → Knowledge Model

```
Data → Analysis → Interpretation → Explanation → Knowledge
```

Tiga level yang berbeda:
- **Analysis** — "Apa yang terjadi?" (deskriptif + inferensial)
- **Interpretation** — "Apa artinya?" (konteks RQ + literatur)
- **Failure Analysis** — "Mengapa tidak berhasil?" (boundary conditions)

### Beyond p-value

**Statistical significance ≠ practical significance.** Selalu laporkan:
1. p-value (signifikansi statistik)
2. Effect size (besarnya efek)
3. Confidence interval (rentang ketidakpastian)

| Effect Size (Cohen's d) | Interpretasi |
|-------------------------|-------------|
| < 0.2 | Small |
| 0.2 – 0.8 | Medium |
| > 0.8 | Large |

### Pemilihan Uji Statistik

| Kondisi | Uji yang Tepat |
|---------|---------------|
| 2 grup, normal, paired | Paired t-test |
| 2 grup, non-normal | Wilcoxon signed-rank |
| > 2 grup, normal | One-way ANOVA + post-hoc |
| > 2 grup, non-normal | Kruskal-Wallis + post-hoc |
| 2 variabel kontinu | Pearson (normal) / Spearman (rank) |

### Failure Analysis as Contribution

Hipotesis yang ditolak adalah **temuan yang berharga**:

| Dataset | New (F1) | Baseline (F1) | p-value | Cohen's d |
|---------|---------|--------------|---------|-----------|
| DS-1 (small, clean) | 94.2±1.1 | 89.3±1.5 | <0.001 | **3.7** |
| DS-4 (medium, noisy) | 78.3±3.2 | 82.1±2.8 | 0.008 | **-1.3** |
| DS-5 (large, noisy) | 71.6±4.1 | 80.5±3.0 | <0.001 | **-2.5** |

**Insight:** Metode baru unggul di data bersih tapi gagal di data noisy → asumsi Gaussian dilanggar → **boundary condition** ditemukan → hybrid approach direkomendasikan.

**Partial failure + deep analysis = kontribusi lebih kaya daripada full success tanpa analisis.**

### Limitation Types

| Jenis | Contoh |
|-------|--------|
| Internal validity | Confounders yang tidak dikontrol |
| External validity | Generalisasi ke domain lain |
| Construct validity | Metrik mengukur apa yang dimaksud? |
| Statistical limitation | Sample size, asumsi distribusi |

### Jebakan Kognitif

1. "Signifikan statistik = penting secara praktis" → cek effect size
2. "Hipotesis tidak didukung → cari sudut baru" → p-hacking
3. "Kegagalan tidak perlu dilaporkan detail" → missed insight
4. "Limitasi cukup disebutkan, tidak perlu dianalisis" → kedalaman hilang

---

## Template A.14 — Analysis & Interpretation Report

```
ANALYSIS & INTERPRETATION

1. Statistik Deskriptif:
   | Skenario | Mean | Std | Median | Min | Max | n |
   |----------|------|-----|--------|-----|-----|---|
   | Random Forest | 0.938504 | 0.001076 | 0.938824 | 0.936845 | 0.939594 | 5 |
   | Decision Tree | 0.938107 | 0.001064 | 0.938899 | 0.936845 | 0.939670 | 5 |

2. Uji Hipotesis:
   Uji yang digunakan  : Paired Sample t-test
   Justifikasi          : Eksperimen membandingkan dua algoritma klasifikasi pada dataset yang sama dengan lima kali pengujian menggunakan pasangan seed yang sama. Hasil uji Shapiro-Wilk menunjukkan seluruh metrik memiliki p-value > 0,05 sehingga asumsi normalitas terpenuhi. Oleh karena itu digunakan Paired Sample t-test.
   Hasil: p = 0.9375, effect size (d/r/η²) = -0.0373
   CI 95%               : [0.937167, 0.939840]

3. Keputusan:
   [ ✓ ] H₀ ditolak → H₁ diterima
   [ ✓ ] H₀ tidak ditolak

4. Interpretasi:
   Hubungan ke RQ       : Penelitian bertujuan membandingkan performa Random Forest dan Decision Tree dalam klasifikasi. Berdasarkan hasil pengujian, kedua algoritma memberikan performa yang hampir sama pada metrik Accuracy, Recall, dan F1-Score. Namun Random Forest memberikan nilai Precision yang secara statistik lebih baik dibandingkan Decision Tree.
   Practical significance: Walaupun Accuracy kedua model hampir identik, peningkatan Precision pada Random Forest menunjukkan model menghasilkan prediksi positif yang lebih akurat. Hal ini bermanfaat ketika kesalahan prediksi positif perlu diminimalkan.
   Perbandingan literatur: Hasil penelitian sejalan dengan berbagai penelitian yang menunjukkan bahwa Random Forest umumnya memiliki kemampuan generalisasi lebih baik dibandingkan Decision Tree karena menggunakan mekanisme ensemble yang mampu mengurangi overfitting dan meningkatkan stabilitas model.

5. Limitation:
   | Jenis | Ancaman | Dampak | Mitigasi |
   |-------|---------|--------|----------|
   | Statistical | Jumlah pengujian hanya 5 kali | Power statistik relatif rendah | Menambah jumlah pengulangan pada penelitian berikutnya |
   | External Validity | Hanya menggunakan satu dataset | Hasil belum tentu berlaku pada dataset lain | Menguji pada beberapa dataset berbeda |
   | Model | Hanya membandingkan dua algoritma | Perbandingan belum mewakili semua metode klasifikasi | Menambahkan algoritma lain seperti XGBoost atau SVM |

6. Failure Analysis (jika H₀ tidak ditolak):
   Penyebab potensial  : Karakteristik dataset relatif sederhana sehingga kedua algoritma mampu membangun pola klasifikasi yang hampir sama.
   Boundary condition   : Keunggulan Random Forest kemungkinan akan lebih terlihat pada dataset yang lebih kompleks, memiliki noise tinggi, atau jumlah fitur yang lebih banyak.
   Insight              : Walaupun Precision Random Forest lebih tinggi, secara keseluruhan kedua algoritma memberikan performa yang hampir setara. Oleh karena itu pemilihan algoritma dapat mempertimbangkan kebutuhan aplikasi, kompleksitas model, dan waktu komputasi.
```

---

## Latihan 1 — Pemilihan Uji Statistik

Tentukan uji statistik yang tepat untuk eksperimen Anda.

| Pertanyaan | Jawaban |
|-----------|---------|
| Berapa grup yang dibandingkan? | 2 (Random Forest dan Decision Tree) |
| Apakah data berpasangan (paired)? | Ya |
| Apakah distribusi normal? (uji normalitas) | Ya, berdasarkan uji Shapiro-Wilk seluruh metrik memiliki p-value > 0,05 |
| Uji yang dipilih | Paired Sample t-test |
| Justifikasi | Membandingkan dua model pada data yang sama dengan distribusi normal dan pasangan observasi yang sesuai |

**Effect size yang akan dilaporkan:** [ ✓ ] Cohen's d / [ ] Eta-squared / [ ] Lainnya: ____

---

## Latihan 2 — Interpretasi Hasil

Gunakan data berikut (atau data riil Anda) untuk berlatih interpretasi.

**Data:**
| Model | Accuracy (mean ± std) | n |
|-------|----------------------|---|
| A | 89.2 ± 1.5 | 10 |
| B | 87.8 ± 2.1 | 10 |

p = 0.045, Cohen's d = 0.74, CI 95% = [0.03, 2.77]

| Aspek | Interpretasi |
|-------|-------------|
| Signifikansi statistik | Precision memiliki p-value 0,0002 sehingga terdapat perbedaan signifikan antara Random Forest dan Decision Tree. Accuracy, Recall, dan F1-Score tidak menunjukkan perbedaan signifikan. |
| Effect size | Cohen's d menunjukkan efek sangat kecil pada Accuracy dan Recall, efek besar pada Precision, serta efek sedang pada F1-Score. |
| Practical significance | Random Forest lebih unggul pada Precision sehingga lebih baik dalam mengurangi prediksi positif yang salah. |
| Hubungan ke RQ | Random Forest memberikan performa yang sedikit lebih baik dibandingkan Decision Tree terutama pada metrik Precision. |
| Perbandingan literatur | Hasil sesuai dengan penelitian sebelumnya yang menyatakan Random Forest memiliki kemampuan generalisasi yang lebih baik dibandingkan Decision Tree karena menggunakan pendekatan ensemble. |

---

## Latihan 3 — Failure Analysis

Latih kemampuan failure analysis: hipotesis TIDAK didukung. Apa yang bisa dipelajari?

**Skenario:** Metode baru Anda mendapat F1 = 83.2%, baseline = 84.7%. p = 0.12 (tidak signifikan).

| Pertanyaan | Jawaban |
|-----------|---------|
| Apakah ini "gagal"? | Tidak. Walaupun sebagian besar metrik tidak berbeda signifikan, hasil tersebut tetap memberikan informasi mengenai performa kedua algoritma. |
| Kemungkinan penyebab? | Dataset tidak terlalu kompleks sehingga kedua algoritma menghasilkan performa yang hampir sama. |
| Boundary condition? | Random Forest diperkirakan akan lebih unggul pada dataset yang lebih besar atau memiliki tingkat noise yang tinggi. |
| Insight yang bisa diambil? | Tidak semua peningkatan performa harus signifikan secara statistik. Evaluasi perlu mempertimbangkan p-value, effect size, dan konteks penggunaan model. |
| Apakah layak dilaporkan? Mengapa? | Ya. Hasil yang tidak signifikan tetap merupakan temuan ilmiah yang dapat menjadi referensi penelitian selanjutnya. |

**Limitation terkait:**
| Jenis | Ancaman | Dampak |
|-------|---------|--------|
| Statistical | Pengujian hanya dilakukan sebanyak 5 run per model | Statistical power lebih rendah sehingga hasil uji kurang stabil. |
| External Validity | Penelitian hanya menggunakan satu dataset | Hasil belum tentu dapat digeneralisasikan pada dataset lain. |


---

## Refleksi

> Apakah "failure" dalam riset benar-benar gagal, atau justru kontribusi? Bagaimana failure analysis mengubah cara Anda melihat hasil negatif?

> Melalui praktikum ini saya memahami bahwa analisis data tidak hanya berfokus pada perolehan nilai akurasi tertinggi, tetapi juga pada interpretasi hasil menggunakan uji statistik. Perbedaan performa antar model perlu didukung oleh p-value, effect size, dan confidence interval agar kesimpulan yang diambil lebih valid. Selain itu, hasil yang tidak signifikan bukan berarti penelitian gagal, melainkan dapat memberikan wawasan mengenai kondisi ketika suatu algoritma bekerja dengan baik maupun ketika performanya setara dengan metode lain.___________________________________________________
> ___________________________________________________
