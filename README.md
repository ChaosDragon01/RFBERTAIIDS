# Hybrid BERT-Random Forest DDoS Detection System

This repository hosts the code, data ingestion pipelines, and experimental evaluation reports for a hybrid intrusion detection system (IDS) designed to mitigate zero-day distributed denial of service (DDoS) attacks. By combining **BERT (Bidirectional Encoder Representations from Transformers)** for semantic log parsing with an optimized **Random Forest classifier**, the system effectively translates raw network telemetry into linguistic sequences to detect complex, signatureless anomalies.

---

## Architecture Overview

The system processes multi-generational network traffic datasets by serializing raw network records into structured natural text sentences. These narratives are evaluated through a multi-stage pipeline:

1. **Data Ingestion & Stratification**: Ingests multi-year datasets (CICIDS2017 and CSE-CIC-IDS2018) using memory-safe Parquet and CSV chunks via PyArrow.
2. **Contextual Tokenization Layer**: Maps features such as destination ports, protocols, flow durations, and packet counts into text strings tokenized via the `bert-base-uncased` model.
3. **Bidirectional Self-Attention Processing**: Computes semantic dependencies across tokens to extract a dense 768-dimensional hidden vector representation.
4. **Random Forest Classification Ensemble**: Routes the high-dimensional feature matrices into an optimized 100-estimator ensemble to classify traffic as Benign or Attack Anomaly with low latency.

```
Raw Network Log Telemetry ──> BERT Contextual Tokenization ──> Bidirectional Self-Attention ──> Random Forest Ensemble ──> Benign / Malicious Alert
```
*(See `model-diagram.png` for complete flowchart visualization.)*

---

## Experimental Results

Evaluated on a rigorous test pack of 4,500 network log instances, the hybrid model achieved strong classification performance while maintaining fast per-packet inference speeds:

* **Overall Model Accuracy**: $83.80\%$
* **Total Processing Latency**: $0.0421$ seconds
* **Average Per-Packet Speed**: $0.0094$ milliseconds/packet

### Detailed Classification Performance Profile

| Class | Precision | Recall | F1-Score | Support |
| :--- | :---: | :---: | :---: | :---: |
| **Benign (0)** | $0.88$ | $0.90$ | $0.89$ | $3151$ |
| **Attack Anomaly (1)** | $0.70$ | $0.74$ | $0.72$ | $1349$ |
| **Macro Average** | $0.81$ | $0.80$ | $0.80$ | $4500$ |
| **Weighted Average** | $0.84$ | $0.84$ | $0.84$ | $4500$ |

---

## Results Visualizations (`results/`)

The evaluation generated performance matrices stored in the results folder:
* **`ddos_confusion_matrix.png` / `final_testing.png` / `improvement.png` / `initial_testing.png`**: Details the distribution of True Negatives ($2,823$), True Positives ($948$), False Positives ($328$), and False Negatives ($401$).
* **`model-diagram.png`**: Visualizes the sequential flow from raw telemetry to final threat classification.

---

## Repository Structure & Documentation

* `results/`: Contains generated confusion matrices and performance plots.
* `SS.CSCI350.FINALREPORT.pdf`: Complete research documentation detailing literature reviews, methodology, data collection strategies, and hardware resource constraints.