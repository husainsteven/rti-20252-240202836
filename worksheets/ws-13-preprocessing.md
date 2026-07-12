# WS-13: Data Preprocessing

> **Bab 13 — Preprocessing & Persiapan Data untuk Analisis**

---

## Ringkasan Materi

### Data Refinement Pipeline

```
Raw Data → Cleaning → Transformation → Normalization → Processed Data → Analysis Ready
```

Setiap tahap memiliki tujuan berbeda. **Preprocessing bukan langkah teknis biasa** — setiap keputusan preprocessing adalah keputusan riset yang bisa mengubah kesimpulan.

### Empat Prinsip Preprocessing

| Prinsip | Deskripsi |
|---------|----------|
| **Consistency** | Metode sama untuk data yang sama |
| **Transparency** | Setiap langkah terdokumentasi |
| **Reproducibility** | Orang lain bisa mengulang dengan hasil sama |
| **Minimal Distortion** | Ubah sesedikit mungkin; jika normalisasi tidak perlu, jangan lakukan |

### Cleaning Triad

| Masalah | Strategi | Risiko |
|---------|---------|--------|
| **Missing values** | | |
| — Listwise deletion | Missing < 5%, random | Data loss |
| — Mean/median imputation | Sedikit missing, dist. normal | Mengurangi variabilitas |
| — Model-based imputation | Banyak missing, pola sistematis | Introduces dependency |
| — Flag & separate | Missing karena alasan substantif | Kompleksitas analisis |
| **Duplikat** | Identifikasi → verifikasi → hapus | False positive (data mirip ≠ duplikat) |
| **Error format** | Standardisasi tipe, encoding | Kehilangan informasi saat konversi |

### Normalisasi — Kapan & Metode Mana

| Metode | Formula | Output | Sensitif Outlier? |
|--------|---------|--------|-------------------|
| Min-max | (x-min)/(max-min) | [0, 1] | Ya |
| Z-score | (x-mean)/std | Unbounded | Lebih robust |
| Robust scaling | (x-median)/IQR | Unbounded | Paling robust |

**Kunci:** Parameter normalisasi harus dihitung dari **training set saja** — bukan seluruh data. Pelanggaran = **data leakage**.

### Data Leakage Prevention

Data leakage terjadi ketika informasi dari test set "bocor" ke preprocessing:
- Normalisasi parameter dari seluruh dataset ← **SALAH**
- Cross-validation dilakukan sebelum split ← **SALAH**
- Feature selection menggunakan label test set ← **SALAH**

### Jebakan Kognitif

1. "Preprocessing cuma teknis — tidak perlu detail" → bisa ubah kesimpulan
2. "Lebih banyak preprocessing = lebih bersih = lebih baik" → over-processing distorsi data
3. "Normalisasi selalu diperlukan" → belum tentu, tergantung metode analisis
4. "Imputation sama untuk semua situasi" → strategi harus sesuai konteks

---

## Template A.13 — Preprocessing Documentation Log

```
PREPROCESSING LOG

Dataset           : MQTT Dataset (mqttdataset_reduced.xlsx)
Jumlah data awal  : 330.926 data

Cleaning:
| Masalah | Jumlah Kasus | Penanganan | Justifikasi |
|---------|-------------|------------|-------------|
| Missing |   0          | Tidak ada tindakan           | Dataset tidak memiliki missing value.            |
| Duplikat|   0          | Tidak ada tindakan           | Tidak ditemukan data duplikat.            |
| Error   |   0          | Pemeriksaan tipe data           | Format data konsisten dan dapat diproses.            |

Transformation:
| Transformasi | Variabel | Detail | Alasan |
|-------------|----------|--------|--------|
| Label Encoding            | Seluruh kolom pada dataset         | Setiap kolom diubah menjadi representasi numerik menggunakan LabelEncoder       | Agar seluruh data dapat diproses oleh algoritma Random Forest yang memerlukan input numerik.       |

Normalization:
  Metode    : Random Forest
  Alasan    : Random Forest tidak sensitif terhadap skala fitur sehingga preprocessing berupa normalisasi tidak diperlukan.
  Parameter : Tidak diterapkan.

Leakage Check:
  [ ✓ ] Parameter normalisasi dari training set saja
  [ ✓ ] Tidak ada informasi test set dalam preprocessing
  [ ✓ ] Cross-validation dilakukan setelah split

Jumlah data akhir :  330.926 records
Script tersedia   : [ ✓ ] Ya → path: Tugas_RTI.ipynb | [ ] Belum
```

---

## Latihan 1 — Cleaning Plan

Periksa dataset Anda (atau dataset contoh) dan dokumentasikan masalah yang ditemukan.

| Masalah | Jumlah Kasus | Penanganan | Justifikasi |
|---------|-------------|------------|-------------|
| Missing Value | 0 | Tidak ada tindakan | Dataset tidak memiliki missing value sehingga seluruh data dapat digunakan untuk proses pelatihan dan pengujian model. |
| Data Duplikat | 0 | Tidak ada tindakan | Tidak ditemukan data duplikat sehingga tidak diperlukan penghapusan data. |
| Error Format | 0 | Pemeriksaan tipe data | Seluruh atribut memiliki format yang konsisten sehingga tidak memerlukan perbaikan tambahan. |


**Jumlah data sebelum cleaning:**330.926 records
**Jumlah data setelah cleaning:**330.926 records
**Persentase data yang hilang/berubah:**0%

---

## Latihan 2 — Normalisasi Decision

Tentukan apakah data Anda perlu normalisasi, dan jika ya, metode apa yang tepat.

| Variabel | Range Asli | Distribusi | Outlier? | Metode Normalisasi | Alasan |
|----------|-----------|-----------|----------|-------------------|--------|
| Seluruh fitur numerik | Beragam sesuai atribut | Beragam | Tidak dianalisis secara khusus | Tidak dilakukan | Random Forest tidak bergantung pada jarak antar data sehingga normalisasi tidak diperlukan. |
| Label | Hasil Label Encoding | Kategorikal menjadi numerik | Tidak | Tidak dilakukan | Label merupakan target klasifikasi sehingga tidak perlu dinormalisasi. |

**Apakah normalisasi diperlukan?** [ ] Ya / [ ✓ ] Tidak
**Justifikasi:**
> Normalisasi tidak dilakukan karena algoritma Random Forest merupakan algoritma berbasis pohon keputusan yang tidak dipengaruhi oleh skala data. Oleh karena itu, penggunaan normalisasi tidak memberikan peningkatan performa yang signifikan dan justru dapat menambah proses yang tidak diperlukan.___________________________________________________

**Leakage check:**
- [ ✓ ] Parameter dihitung dari training set saja
- [ ✓ ] Normalisasi diterapkan setelah train-test split

---

## Latihan 3 — Preprocessing Report

Buat ringkasan preprocessing lengkap — dokumentasi yang cukup bagi orang lain untuk mereplikasi.

```
PREPROCESSING SUMMARY

1. Dataset:  mqttdataset_reduced.xlsx
2. Data awal: 330.926 records records, 33 features
3. Cleaning:
   - Missing values: 0 kasus, metode: tidak ada tindakan.
   - Duplikat: 0 kasus, tindakan:  tidak ada penghapusan data.
   - Error: 0 kasus, tindakan:  pemeriksaan format data.
4. Transformation: Label Encoding pada kolom target untuk mengubah label kategorikal menjadi numerik.
5. Normalisasi: Tidak dilakukan. (metode), parameter dari tidak diterapkan karena algoritma Random Forest tidak memerlukan normalisasi.

6. Data akhir:  330.926 records, 33 features
7. Leakage check: [ ✓ ] Lulus / [ ] Ada masalah
```

---

## Refleksi

> Apakah Anda pernah melakukan normalisasi "karena biasa dilakukan" tanpa mempertimbangkan apakah benar-benar diperlukan? Apa risiko over-preprocessing?

> Pada awal mempelajari machine learning, saya menganggap bahwa normalisasi harus selalu dilakukan pada setiap dataset karena merupakan langkah yang umum dalam proses preprocessing. Namun setelah mempelajari karakteristik algoritma yang digunakan, saya memahami bahwa kebutuhan preprocessing bergantung pada metode analisis yang dipilih. Random Forest merupakan algoritma berbasis decision tree yang tidak dipengaruhi oleh perbedaan skala data sehingga normalisasi tidak diperlukan. Melakukan preprocessing yang tidak dibutuhkan dapat menyebabkan proses menjadi lebih kompleks tanpa memberikan peningkatan performa model. Oleh karena itu, setiap tahapan preprocessing harus didasarkan pada karakteristik data dan algoritma yang digunakan agar hasil penelitian tetap valid dan dapat dipertanggungjawabkan.___________________________________________________
> ___________________________________________________
