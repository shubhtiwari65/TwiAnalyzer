# TwiAnalyzer: Twitter Sentiment Analysis Pipeline

> An end-to-end **NLP sentiment classification pipeline** with a **Tkinter GUI** — trained on 160,000+ tweets using TF-IDF vectorization and KNN, achieving **85% accuracy** on binary sentiment classification.

---

## Overview

TwiAnalyzer classifies tweets as **Positive**, **Negative**, or **Neutral** using a machine learning pipeline built on the [Sentiment140 dataset](https://www.kaggle.com/datasets/kazanova/sentiment140) (1.6 million labeled tweets).

The project is structured into three independent modules:
- **Training pipeline** — data loading, preprocessing, vectorization, model training
- **Inference engine** — serialized model + vectorizer for reuse without retraining
- **Tkinter GUI** — interactive desktop app for real-time tweet sentiment prediction

---

## Features

- Real-time sentiment prediction via desktop GUI
- Filter tweets by sentiment (Positive / Negative / Neutral)
- Sort tweets alphabetically or by length
- Light / Dark mode toggle
- Evaluation metrics exported to structured Excel report (accuracy, precision, recall, confusion matrix)

---

## Results

| Metric | Score |
|--------|-------|
| Accuracy | 85% |
| Precision | Weighted avg |
| Recall | Weighted avg |

Evaluation outputs saved to `model_evaluation_metrics.xlsx` with separate sheets for metrics and confusion matrix.

---

## Tech Stack

| Component | Library |
|-----------|---------|
| NLP / Vectorization | scikit-learn (TF-IDF) |
| Classification | scikit-learn (KNN, k=5) |
| Data Handling | Pandas |
| Model Serialization | joblib (.pkl) |
| Dataset Download | kagglehub |
| GUI | Tkinter |
| Reporting | openpyxl / Pandas ExcelWriter |

---

## Project Structure

```
TwiAnalyzer/
│
├── main.py                        # Full pipeline: training + GUI
├── dataset.py                     # Dataset loading and preprocessing
├── report.py                      # Evaluation metrics and Excel export
│
├── sentiment_model_knn.pkl        # Serialized KNN classifier
├── sentiment_model.pkl            # Serialized baseline classifier
├── vectorizer.pkl                 # Serialized TF-IDF vectorizer
│
└── model_evaluation_metrics.xlsx  # Accuracy, Precision, Recall + Confusion Matrix
```

---

## Pipeline Architecture

```
Sentiment140 Dataset (1.6M tweets)
            ↓
   Text Preprocessing
   - Remove URLs, @mentions, hashtags
   - Strip non-alphabetic characters
   - Lowercase normalization
            ↓
  TF-IDF Vectorization (top 5000 features)
            ↓
   KNN Classifier (k=5)
   trained on 50,000 sampled tweets
            ↓
  ┌─────────────────────────────┐
  │  Serialized Model + Vectorizer (.pkl) │
  └─────────────────────────────┘
            ↓
   Tkinter GUI — real-time inference
   + filter / sort / dark mode
```

---

## How to Run

### 1. Install dependencies

```bash
pip install scikit-learn pandas kagglehub joblib openpyxl
```

### 2. Run the pipeline

```bash
python main.py
```

This will:
- Download the Sentiment140 dataset via `kagglehub`
- Preprocess and vectorize tweets
- Train the KNN classifier
- Save model and vectorizer as `.pkl` files
- Export evaluation metrics to `model_evaluation_metrics.xlsx`
- Launch the Tkinter GUI

### 3. Use the GUI

- Type any tweet into the input box
- Click **Analyze Sentiment** to get a Positive / Negative / Neutral prediction
- Use the **Filter** and **Sort** dropdowns to browse the dataset
- Toggle **🌙 / ☀️** for dark/light mode

---

## Key Design Decisions

- **TF-IDF over Bag-of-Words** — captures term importance relative to the corpus, not just raw frequency, improving separation between sentiment classes
- **50,000 sample for KNN** — KNN is memory and compute intensive at scale; sampling 50K from 1.6M keeps training feasible while maintaining accuracy
- **Serialized vectorizer alongside model** — ensures inference uses identical feature space as training; plug-and-play on unseen tweets without retraining
- **Separate report module** — decouples evaluation logic from training, reflecting production-oriented code design

---

## License

MIT License
