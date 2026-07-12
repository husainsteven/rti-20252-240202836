# Arsitektur dan Skema Penelitian

## 1. Diagram Alur Penelitian

```text
Dataset MQTT-IoT
        │
        ▼
Preprocessing Data
- Missing Value
- Encoding
- Normalisasi
        │
        ▼
Seleksi Fitur
Particle Swarm Optimization
        │
        ▼
Model XGBoost
        │
        ▼
Prediksi
Normal / DoS Attack
        │
        ▼
Evaluasi
Accuracy
Precision
Recall
F1-Score
ROC-AUC
```

---

## 2. Workflow Seleksi Fitur Menggunakan PSO

```text
Inisialisasi Partikel
        │
        ▼
Generate Kombinasi Fitur
        │
        ▼
Training XGBoost
        │
        ▼
Hitung Fitness
(Accuracy/F1-Score)
        │
        ▼
Update Velocity
        │
        ▼
Update Position
        │
        ▼
Stopping Criteria
        │
        ▼
Subset Fitur Terbaik
```

---

## 3. Arsitektur Sistem

```text
Dataset MQTT
      │
      ▼
Preprocessing
      │
      ▼
Seleksi Fitur PSO
      │
      ▼
Model XGBoost
      │
      ▼
Deteksi
Normal / DoS
      │
      ▼
Evaluasi
```

---

## 4. Komponen Penelitian

| Komponen | Fungsi |
|----------|--------|
| Dataset MQTT-IoT | Data lalu lintas jaringan |
| Preprocessing | Membersihkan dan menyiapkan data |
| PSO | Memilih fitur terbaik |
| XGBoost | Mengklasifikasikan trafik normal dan DoS |
| Evaluasi | Mengukur performa model |

---

## 5. Parameter PSO

- Population Size
- Maximum Iteration
- Inertia Weight
- Cognitive Coefficient (c1)
- Social Coefficient (c2)

---

## 6. Hyperparameter XGBoost

- learning_rate
- max_depth
- n_estimators
- subsample
- colsample_bytree
- gamma

---

## 7. Metrik Evaluasi

- Accuracy
- Precision
- Recall
- F1-Score
- ROC-AUC
- Confusion Matrix