# Base Error Detection Pipeline for Illumina 16S Sequences

This repository contains a complete preprocessing and machine learning pipeline designed to classify base-level sequencing errors in 16S rRNA Illumina data. It takes raw `.qual` and `.error` files as input, processes them into structured CSVs, engineers meaningful per-base features, and trains machine learning models to predict whether each base is a true match or a sequencing error.

---

## 📁 Project Structure

### 1. **Input Conversion**
- Converts `.qual` files into CSV format with per-sequence quality scores.
- Converts `.error` files into CSV format containing the actual base sequences.
- Merges both files to align quality scores with base sequences for each read.

### 2. **Feature Engineering**
- Parses per-base PHRED scores for forward, reverse, and overlapping regions.
- Assigns homopolymer position labels and lengths.
- Calculates GC content and Shannon entropy in local windows.
- Detects motifs such as the ‘GCC’ pattern.
- Computes base-level window abundance and sequence-level abundances.
- Normalizes base positions (forward and reverse) within each sequence.

### 3. **Dataset Construction**
- Outputs a large base-level dataset with 20+ engineered features.
- Encodes ground-truth labels: `"s"` for sequencing error, `"m"` for match.

### 4. **Machine Learning Pipeline**
- Balances highly imbalanced classes (errors ~1%) via downsampling.
- Trains a baseline **Random Forest classifier**.
- Performs **threshold sweeping** to tune sensitivity/specificity tradeoff.

### 5. **Model Ensembling**
- Combines predictions from **Random Forest**, **XGBoost**, and **MLPClassifier** using probability averaging.
- Evaluates ensemble combinations across thresholds.

### 6. **Deployment on Validation Data**
- Applies saved models on new mock community datasets.
- Outputs predictions and compares them with true labels for benchmarking.

---

## 🧪 Dependencies

Ensure you have the following Python libraries installed:

```bash
pandas
numpy
scikit-learn
xgboost
joblib
