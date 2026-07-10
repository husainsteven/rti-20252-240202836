# WS-10: Experiment Execution & Data Collection

> **Bab 10 — Eksekusi Eksperimen & Pengumpulan Data**

---

## Ringkasan Materi

### Experiment Execution Pipeline

```
Design → Execution Plan → Controlled Execution → Data Collection → Data Logging → Dataset for Analysis
```

### Multiple Run = Non-Negotiable

Single run **tidak pernah cukup** untuk klaim ilmiah. Minimum 5-10 run per skenario dengan seed berbeda. Multiple run menghasilkan:
- Mean, std, confidence interval
- Distribusi hasil → uji statistik
- Variabilitas → error bar di grafik

### Execution Plan

Setiap eksperimen harus memiliki plan sebelum eksekusi:
- Daftar skenario
- Jumlah run per skenario
- Random seed per run (pre-determined!)
- Urutan eksekusi (randomisasi/counterbalancing)
- Pre-execution checklist

### Data Logging Komprehensif

Setiap run menghasilkan log terstruktur:
1. **Identitas** — Run ID, timestamp, skenario
2. **Konfigurasi** — Semua parameter, seed, code version
3. **Hasil** — Semua metrik, output detail
4. **Metadata** — Waktu eksekusi, resource usage, warning/error

Format: CSV/JSON/database — **bukan stdout yang di-copy-paste**.

### Engineering vs Research Execution

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Run | Sekali (deploy) | Multiple (min 5-10, seed berbeda) |
| Logging | Error log, access log | Semua parameter, metrik, metadata |
| Anomali | Bug → fix → redeploy | Investigasi → dokumentasi → analisis |
| Urutan | Tidak penting | Bisa bias — perlu randomisasi |

### Anomali = Dokumentasi, Bukan Hapus

Run gagal/anomali tidak boleh dihapus tanpa dokumentasi. Bisa jadi:
- **Bug** → fix & re-run (dokumentasikan!)
- **Batas kemampuan metode** → DNF = temuan
- **Data yang bias** jika hanya simpan run "berhasil"

### Jebakan Kognitif

1. "Satu angka cukup" → tanpa distribusi, tidak bisa diuji
2. "Seed tidak penting" → bahkan algoritma deterministik bisa dipengaruhi library stokastik
3. "Run gagal langsung hapus" → kehilangan temuan potensial
4. "Semua run harus hari ini" → thermal throttling, fatigue

---

## Template A.10 — Execution Plan & Data Log

```
EXECUTION PLAN

| Run # | Skenario | Seed | Parameter | Status | Waktu | Output File |
|-------|----------|------|-----------|--------|-------|-------------|
| 1     |          |      |           |        |       |             |
| 2     |          |      |           |        |       |             |
| 3     |          |      |           |        |       |             |
| ...   |          |      |           |        |       |             |

Jumlah runs per skenario : ____
Total runs               : ____

DATA LOG (per run):
  Run ID    : ____________________
  Timestamp : ____________________
  Skenario  : ____________________
  Input     : ____________________
  Output    : ____________________
  Anomali   : ____________________
  Catatan   : ____________________
```

---

## Latihan 1 — Execution Plan

Susun execution plan untuk eksperimen Anda. Tentukan skenario, jumlah run, dan seed sebelum eksekusi.

| Run # | Skenario | Seed | Parameter Kunci | Status |
|-------|----------|------|----------------|--------|
| 1 | Random Forest - MQTT Dataset | 42 | n_estimators=100, test_size=0.2 | Completed |
| 2 | Random Forest - MQTT Dataset | 123 | n_estimators=100, test_size=0.2 | Planned |
| 3 | Random Forest - MQTT Dataset | 999 | n_estimators=100, test_size=0.2 | Planned |
| 4 | Random Forest - MQTT Dataset | 2025 | n_estimators=100, test_size=0.2 | Planned |
| 5 | Random Forest - MQTT Dataset | 777 | n_estimators=100, test_size=0.2 | Planned |

**Total skenario:__1__
**Run per skenario:__5__
**Total run keseluruhan: __5__

---

## Latihan 2 — Data Log Terstruktur

Desain format data log untuk eksperimen Anda. Tentukan field apa saja yang akan dicatat.

**Identitas:**
| Field | Contoh |
|-------|--------|
| Run ID | run-001 |
| Timestamp | 2026-07-08 11:24 WIB |
| Dataset | MQTT Dataset Reduced |
| Model | Random Forest Classifier |
| Peneliti | Husain Stefano |

**Konfigurasi:**
| Field | Contoh |
|-------|--------|
| Seed | 42 |
| Code version | Google Colab Notebook v1 |
| Train-Test Split | 80 : 20 |
| Random State | 42 |
| Estimator | 100 |

**Hasil:**
| Metrik | Tipe Data | Range Valid |
|--------|----------|-------------|
| Accuracy | float | 0 – 1 |
| Precision | float | 0 – 1 |
| Recall | float | 0 – 1 |
| F1-Score | float | 0 – 1 |
| Execution Time | float | >0 detik |

**Format output:** [ ✓ ] CSV / [ ✓ ] Notebook Google Colab [ ] JSON / [ ] Database / [ ] Lainnya: ____

---

## Latihan 3 — Anomaly Protocol

Rencanakan bagaimana menangani anomali. Untuk setiap jenis, tentukan langkah yang diambil.

| Jenis Anomali | Contoh | Tindakan |
|---------------|--------|----------|
| Run gagal (crash) | Runtime Colab terputus | Jalankan kembali eksperimen dan catat penyebabnya |
| Hasil ekstrem | Accuracy jauh di bawah 80% | Periksa preprocessing dan parameter model |
| Waktu eksekusi anomali | Training jauh lebih lama dari biasanya | Cek penggunaan RAM dan CPU Colab |
| Inkonsistensi dengan run lain | Accuracy berubah jauh antar seed | Lakukan minimal 5 kali eksperimen kemudian hitung rata-rata hasil |

**Prinsip:** Detect → Investigate → Document → Decide

---

## Refleksi

> Pernahkah Anda melaporkan hasil riset/tugas dari single run? Apa risikonya? Bagaimana multiple run mengubah kepercayaan terhadap hasil?

**Pengalaman sebelumnya:**
> Sebelumnya saya sering hanya menjalankan model satu kali kemudian langsung menggunakan hasil accuracy sebagai hasil penelitian. Cara tersebut ternyata kurang tepat karena hasil yang diperoleh dapat dipengaruhi oleh proses pembagian data maupun kondisi eksperimen sehingga belum tentu mewakili performa model yang sebenarnya.___________________________________________________
**Yang akan dilakukan berbeda:**
> Untuk penelitian ini saya akan menjalankan eksperimen beberapa kali menggunakan seed yang berbeda, kemudian mencatat seluruh parameter, hasil evaluasi, waktu eksekusi, dan apabila terjadi anomali akan didokumentasikan. Dengan demikian hasil penelitian menjadi lebih valid, dapat direproduksi, dan lebih dapat dipertanggungjawabkan secara ilmiah.___________________________________________________
