# WS-11: Data Validation & Integrity

> **Bab 11 — Validasi Data & Integritas**

---

## Ringkasan Materi

### Data Trust Model

```
Raw Data → Data Cleaning → Consistency Check → Validation Process → Trusted Data
```

Data mentah belum bisa dipercaya. Harus melewati pipeline validasi sebelum siap untuk analisis statistik.

### Empat Pilar Data Quality

| Pilar | Deskripsi | Contoh Pelanggaran |
|-------|----------|-------------------|
| **Accuracy** | Nilai dalam range masuk akal | Akurasi = 1.5 (di luar [0,1]) |
| **Consistency** | Format seragam di semua run | Run 1: CSV, Run 2: JSON |
| **Completeness** | Tidak ada data hilang dari plan | 97 dari 100 run tercatat |
| **Validity** | Data sesuai desain eksperimen | Parameter baseline tercampur treatment |

### Proses Validasi Progresif

1. **Format validation** — Tipe file, header, kolom
2. **Range validation** — Nilai dalam batas logis
3. **Consistency validation** — Format seragam antar-run
4. **Logic validation** — Data cocok dengan desain eksperimen

Jika gagal di langkah awal → tidak perlu lanjut.

### Anomaly Detection — 3 Jenis

| Jenis | Deskripsi | Deteksi |
|-------|----------|---------|
| **Statistical outlier** | Nilai di luar distribusi normal | IQR: < Q1-1.5×IQR atau > Q3+1.5×IQR |
| **Contextual anomaly** | Normal absolut, abnormal dalam konteks | Run 1-10: ~91%, Run 11-20: ~88% |
| **Pattern anomaly** | Pola sistematis (bukan random) | Performa menurun berurutan |

**Prinsip:** Detect → Investigate → Document → Decide — **JANGAN langsung hapus.**

### Engineering vs Research Validation

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan | Data sesuai spesifikasi bisnis | Data layak untuk analisis statistik |
| Missing data | Impute / set default | Investigasi penyebab → dokumentasi |
| Outlier | Bug → fix | Mungkin temuan → investigasi |
| Dokumentasi | Minimal (log error) | Komprehensif (anomali + keputusan) |

### Jebakan Kognitif

1. "Logging otomatis ≠ data benar" → bisa ada bug di logger
2. "Outlier = hapus" → bisa jadi temuan penting
3. "Dataset kecil tidak perlu validasi" → justru lebih rentan
4. "Mean normal = data benar" → [94, 95, 93, **44**, 94] → mean 84% terlihat wajar

---

## Template A.11 — Data Validation Checklist

```
DATA VALIDATION CHECKLIST

Completeness:
  [ ✓ ] Semua skenario tercakup
  [ ✓ ] Jumlah run sesuai rencana
  [ ✓ ] Tidak ada file output hilang
  Missing: 0 dari 5 data points

Format Consistency:
  [ ✓ ] Semua file format sama (CSV/JSON/...)
  [ ✓ ] Header konsisten
  [ ✓ ] Tipe data konsisten (numerik tetap numerik)

Range & Logic:
  [ ✓ ] Nilai dalam range masuk akal
  [ ✓ ] Tidak ada waktu negatif
  [ ✓ ] Metrik 0–100%, tidak di luar range
  Anomali ditemukan: Tidak ditemukan anomali.

Cross-Validation:
  [ ✓ ] Run identik → hasil mendekati
  [ ✓ ] Trend konsisten dengan ekspektasi teori

Keputusan:
  [ ✓ ] Data siap analisis
  [ ] Perlu cleaning
  [ ] Perlu re-run (skenario:-)
```

---

## Latihan 1 — Completeness Check

Verifikasi apakah semua data yang direncanakan sudah terkumpul.

| Skenario | Run Direncanakan | Run Tercatat | Missing | Alasan |
|----------|-----------------|-------------|---------|--------|
| Random Forest + MQTT Dataset | 5 | 5 | 0 | Semua eksperimen berhasil dijalankan |



**Total expected:** 5 | **Total actual:** 5 | **Missing:** 0

**Keputusan untuk data missing:**
> Tidak terdapat data yang hilang. Seluruh eksperimen berhasil dijalankan sesuai rencana sehingga seluruh hasil dapat digunakan pada tahap analisis berikutnya.___________________________________________________

---

## Latihan 2 — Anomaly Investigation

Periksa data Anda untuk anomali. Gunakan metode IQR atau z-score.

**Dataset sampel (atau data Anda sendiri):**

| Run | Accuracy (%) |
|-----|-------------|
| 1 | 0.939156 |
| 2 | 0.936845 |
| 3 | 0.938099 |
| 4 | 0.939594 |
| 5 | 0.938824 |

**Deteksi outlier:**
- Q1 = 0.938099 | Q3 = 0.939156 | IQR = 0.001057
- Batas bawah (Q1 - 1.5×IQR) = 0.936514
- Batas atas (Q3 + 1.5×IQR) = 0.940742
- Outlier terdeteksi: Tidak ada.

**Investigasi (untuk setiap outlier):**

| Outlier | Nilai | Kemungkinan Penyebab | Keputusan |
|---------|-------|---------------------|-----------|
| Tidak ada | - | Seluruh nilai masih berada dalam rentang normal | Data dipertahankan untuk analisis |

---

## Latihan 3 — Validation Report

Buat laporan validasi ringkas untuk dataset eksperimen Anda.

**1. Completeness:** ✓ 100% data terkumpul
**2. Format:** [ ✓ ] Konsisten / [ ] Ada inkonsistensi: ____
**3. Range check (anomali):**Tidak ditemukan nilai yang berada di luar rentang logis.

Seluruh nilai accuracy, precision, recall, dan F1-Score berada pada rentang 0–1.
**4. Logic check:** [ ✓ ] Parameter sesuai plan / [ ] Ada ketidaksesuaian: ____

**Kesimpulan:** [ ✓ ] Data siap analisis / [ ] Perlu tindakan: ____

---

## Refleksi

> Apa perbedaan antara "data yang benar" dan "data yang dipercaya"? Mengapa proses validasi formal diperlukan meskipun data dikumpulkan secara otomatis?

> Data yang benar belum tentu dapat langsung dipercaya karena masih mungkin terdapat kesalahan pencatatan, inkonsistensi format, ataupun anomali yang tidak terlihat secara langsung. Oleh karena itu, proses validasi formal diperlukan untuk memastikan bahwa data memiliki kualitas yang baik, lengkap, konsisten, dan sesuai dengan rancangan eksperimen. Walaupun data dikumpulkan secara otomatis menggunakan program, validasi tetap diperlukan untuk mendeteksi kemungkinan kesalahan sistem maupun hasil eksperimen yang tidak wajar sebelum dilakukan analisis lebih lanjut.___________________________________________________
> ___________________________________________________
