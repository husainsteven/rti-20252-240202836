# WS-12: Result Presentation & Visualization

> **Bab 12 — Penyajian Hasil & Visualisasi**

---

## Ringkasan Materi

### Data → Insight Model

```
Validated Data → Structured Presentation → Visualization → Pattern Recognition → Insight
```

Penyajian **mendahului** analisis. Tabel dan grafik membantu peneliti "melihat" data sebelum menghitung. Langsung ke uji statistik tanpa visualisasi berisiko kesimpulan yang secara teknis benar tapi kontekstual salah (Anscombe's Quartet, 1973).

### Tabel = Presisi, Grafik = Pola

Keduanya **saling melengkapi**:
- Tabel: angka presisi, self-contained (dipahami tanpa teks), sortable
- Grafik: pola visual, tren, perbandingan cepat

### Jenis Grafik Berdasarkan Tujuan

| Tujuan | Jenis Grafik |
|--------|-------------|
| Perbandingan antar-skenario | Bar chart (grouped/stacked) |
| Distribusi per-skenario | Box plot / violin plot |
| Tren temporal | Line chart |
| Korelasi dua variabel | Scatter plot |
| Proporsi (total = 100%) | Pie chart (hati-hati!) |

### Contoh Tabel Hasil yang Baik

| Model | Accuracy (%) | F1-Score (%) | Training Time (min) |
|-------|-------------|-------------|---------------------|
| BERT | 88.4 ± 1.2 | 87.1 ± 1.4 | 45.2 ± 3.1 |
| LSTM | 86.1 ± 1.8 | 84.5 ± 2.0 | 12.8 ± 1.2 |
| SVM | 82.3 ± 0.9 | 80.7 ± 1.1 | 0.3 ± 0.1 |

*N=10 per model. Mean ± std. Diurutkan berdasarkan Accuracy.*

### Visualization Bias — Yang Harus Dihindari

| Bias | Deskripsi | Dampak |
|------|----------|--------|
| Truncated axis | Y tidak dari 0 | Memperbesar perbedaan kecil |
| Inconsistent scale | Dua grafik skala beda | Perbandingan menyesatkan |
| Cherry-picked data | Hanya tampilkan yang "menang" | Selektif, tidak jujur |
| 3D effects | Efek 3D tanpa dimensi data ke-3 | Distorsi tanpa informasi |
| Missing error bar | Tidak ada variabilitas | Menyembunyikan ketidakpastian |

### Engineering vs Research Presentation

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan grafik | Dashboard monitoring | Mendukung argumen ilmiah |
| Informasi wajib | KPI, threshold | Mean, std, CI, N, p-value |
| Bias handling | Less critical | Wajib dihindari (peer-review) |

---

## Template A.12 — Result Presentation Plan

```
RESULT PRESENTATION PLAN

Research Question : Bagaimana performa algoritma Random Forest dalam mengklasifikasikan dataset MQTT berdasarkan metrik Accuracy, Precision, Recall, F1-Score, dan waktu eksekusi?____________________
Metrik Utama      : Accuracy____________________

Tabel Hasil:
| Skenario | Metrik 1 (mean ± std) | Metrik 2 (mean ± std) | n |
|----------|----------------------|----------------------|---|
| Random Forest (5 Run)          | Accuracy = 93.85% ± 0.11%                     | Execution Time = 20.07 ± 1.56 detik                     | 5  |

Visualisasi yang Direncanakan:
| # | Jenis Grafik | Pesan Utama | Metrik |
|---|-------------|-------------|--------|
| 1 | Bar Chart            | Membandingkan accuracy pada setiap run            | Accuracy       |
| 2 | Bar Chart            | Menampilkan rata-rata performa model            | Accuracy, Precision, Recall, F1-Score       |

Bias Check:
  [ x ] Y-axis mulai dari 0 (atau dijustifikasi)
  [ ] Error bar/CI ditampilkan
  [ x ] Semua data disertakan (tidak cherry-picked)
  [ x ] Tidak menggunakan 3D tanpa alasan
```

---

## Latihan 1 — Tabel Hasil

Buat tabel hasil eksperimen Anda (boleh dengan data simulasi jika belum punya data riil).

| Skenario | Metrik 1 (mean ± std) | Metrik 2 (mean ± std) | n |
|----------|----------------------|----------------------|---|
| Random Forest (5 Run) | 93.85% ± 0.11% | 20.07 ± 1.56 detik | 5 |


**Checklist tabel:**
- [ ✓ ] Self-contained (judul jelas, satuan ada, N tercantum)
- [ ✓ ] Mean ± std (bukan single number)
- [ ✓ ] Diurutkan berdasarkan metrik utama
- [ ✓ ] Format konsisten di semua baris

---

## Latihan 2 — Rencana Visualisasi

Rencanakan 2-3 grafik untuk menyajikan data dari Latihan 1. Setiap grafik = satu pesan.

| # | Jenis Grafik | Pesan | Data yang Digunakan |
|---|-------------|-------|---------------------|
| 1 | Bar Chart Accuracy | Membandingkan nilai accuracy pada setiap run eksperimen | Accuracy setiap run |
| 2 | Bar Chart Performa Model | Menampilkan rata-rata Accuracy, Precision, Recall, dan F1-Score | Nilai rata-rata seluruh metrik |
| 3 | Bar Chart Waktu Eksekusi | Membandingkan waktu eksekusi pada setiap run | Execution Time setiap run |

---

## Latihan 3 — Bias Detection

Evaluasi visualisasi berikut untuk bias (skenario dari contoh):

**Skenario:** Metode A = 91.2%, Metode B = 90.8%. Bar chart dengan Y-axis mulai dari 90%.

| Pertanyaan | Jawaban |
|-----------|---------|
| Apakah Y-axis menyesatkan? | Ya. Karena sumbu Y dimulai dari 90%, sehingga perbedaan kecil terlihat jauh lebih besar. |
| Apakah error bar ditampilkan? | Tidak |
| Apakah semua kondisi ditampilkan? | Ya, tetapi visualisasinya tetap dapat menyesatkan karena skala sumbu Y tidak dimulai dari nol. |
| Apa solusinya? | Gunakan sumbu Y mulai dari 0 atau berikan alasan ilmiah jika menggunakan skala yang dipotong. Tambahkan error bar agar variasi data terlihat. |

**Evaluasi grafik Anda sendiri dari Latihan 2:**
- [ ✓ ] Semua bias check lulus
- [ ] Ada yang perlu diperbaiki: ____

---

## Refleksi

> Mengapa tabel dan grafik keduanya diperlukan — tidak cukup salah satu saja? Pernahkah Anda membuat grafik yang (tanpa sengaja) menyesatkan?

> Tabel dan grafik memiliki fungsi yang saling melengkapi. Tabel menyajikan nilai secara rinci sehingga pembaca dapat mengetahui angka yang tepat, sedangkan grafik membantu melihat pola, tren, dan perbandingan dengan lebih cepat. Dengan menggunakan keduanya, hasil penelitian menjadi lebih mudah dipahami dan lebih informatif. Pada eksperimen ini saya belum pernah membuat grafik yang sengaja menyesatkan, namun saya menyadari bahwa penggunaan skala sumbu yang tidak tepat atau tidak menampilkan seluruh data dapat menyebabkan pembaca memperoleh interpretasi yang keliru. Oleh karena itu, visualisasi harus dibuat secara objektif dan mengikuti prinsip penyajian data ilmiah.___________________________________________________
> ___________________________________________________
