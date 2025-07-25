# 🌞 Halo CME Detection from Aditya-L1 SWIS Data

This project detects Halo Coronal Mass Ejection (CME) events using plasma parameters measured by the Aditya-L1 SWIS instrument. The detection pipeline is built using time-series plasma data, matched CME windows, engineered features, threshold scoring, and a decision tree classifier.

## 📌 Problem Statement
Early detection of solar transients (Halo CMEs) is critical for predicting space weather events that impact satellites and communications. This project aims to detect such events based on solar wind variations measured in `.CDF` files from the SWIS payload.

---

## 📁 Data Sources

- **SWIS Level-2 CDF** files from [SWIS](https://pradan.issdc.gov.in/)
- **CACTUS CME Catalog**: LASCO CME detections from [CACTUS](https://www.sidc.be/cactus/)
- **Richardson-Cane ICME list**: Ground truth validation from [RC_ICME](https://izw1.caltech.edu/ACE/ASC/DATA/level3/icmetable2.htm)

---

## 🧪 Methods

- Extract SWIS parameters (density, speed, temperature) from `.CDF`
- Parse CACTUS timestamps of Halo CME events
- Match with ICME arrivals from RC list
- Plot parameter behavior around CME windows
- Derive thresholds for CME detection

---

## 📁 Project Structure

| Folder/File | Description |
|-------------|-------------|
| `cme_summary_features_per_window.csv` | 1-row-per-CME window summary for ML (mean, max, std of features) |
| `final_threshold_values_all_features.csv` | Thresholds (mean, median, 90th percentile, etc.) for all features |
| `improved_cme_detection_weighted.csv` | Rule-based CME detection scores and binary detection labels |
| `final_cme_predictions.csv` | Decision tree model predictions for each CME window |

---

## 🚀 Pipeline Overview

1. 📦 Input: Monthly `.CDF` files from Aditya-L1 SWIS
2. 🧪 Plasma Parameter Extraction: Extracts proton & alpha densities, speeds, thermal velocity
3. 🧮 Feature Engineering:
   - Moving Average (MA)
   - Spike above MA
   - First Derivative (gradient)
   - He++ / H⁺ Ratio
   - Velocity Magnitude (3D)
4. 📊 Visualization:
   - Plots per CME window showing raw + MA + spike + diff for 8 parameters
5. 📈 Threshold Computation:
   - Calculates 90th percentile threshold for each feature
   - Used in rule-based scoring
6. 🧠 Rule-Based Detection:
   - Each feature-window gets a score if it exceeds threshold
   - Windows with score ≥ 3 marked as “Detected CME”
7. 🤖 ML-Based Detection:
   - Uses summary features to train a Decision Tree Classifier
   - Outputs predictions + feature importances

---

## 🧾 Input Files

| File | Description |
|------|-------------|
| Monthly `.CDF` ZIP | Aditya-L1 SWIS bulk plasma data from ISRO |
| `combined_halo_cmes.csv` | Halo CME metadata matched from CACTus catalog |
| `matched_cactus_rc.csv` | Verified CME windows using Richardson & Cane ICME events |

---

## 🧾 Output Files

| File | Description |
|------|-------------|
| `swis_cme_featured_windows.csv` | Full 6-day plasma data around each CME timestamp |
| `final_threshold_values_all_features.csv` | All thresholds per feature |
| `cme_rule_based_detection_scores.csv` | Rule-based scores for each window |
| `improved_cme_detection_weighted.csv` | Weighted detection logic with scores and labels |
| `cme_summary_features_per_window.csv` | Mean, max, std per feature (window level) |
| `final_cme_predictions.csv` | ML predictions on CME occurrence |

---
