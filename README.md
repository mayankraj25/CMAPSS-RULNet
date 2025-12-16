# 📌 Project Overview

This project focuses on **Remaining Useful Life (RUL) prediction** for aircraft engines using the **NASA CMAPSS FD001 dataset**.  
The objective is to build a **reliable time-series model** that learns engine degradation patterns from **multivariate sensor data** and accurately estimates the remaining life of **previously unseen engines**.

This README documents **Version 1** of the modeling pipeline, covering:

- A strong **LSTM baseline**
- **Ensemble learning (bagging)** experiments
- A **hybrid CNN–LSTM architecture** that improves performance

Future versions will build on this foundation with further architectural and training refinements.

---

## 📂 Dataset

- **Dataset:** NASA CMAPSS (FD001)  
- **Source:**  
  https://data.nasa.gov/Aerospace/CMAPSS-Jet-Engine-Simulated-Data/ff5v-kuh6/

### Key Characteristics

- Multivariate **time-series sensor data**
- Single operating condition (**FD001**)
- **Run-to-failure** engine trajectories
- Widely used **benchmark dataset** for RUL prediction research

---

## 🧠 Problem Formulation

- **Input:** Multivariate sensor sequences of shape `(50, 24)`
- **Output:** Remaining Useful Life (RUL)
- **Task Type:** Supervised time-series regression
- **Evaluation Metrics:** RMSE, MAE

---

## Version 1 — LSTM Baseline (FD001)

**Goal:**  
Establish a simple LSTM baseline for CMAPSS FD001 to verify that degradation trends can be learned from raw sensor sequences.

**Setup:**  
- Dataset: NASA CMAPSS FD001  
- Input: `(50, 24)` time-series sequences  
- Model: 2-layer LSTM with dropout  
- Loss: Mean Squared Error  
- Metrics: RMSE, MAE  
- Split: Engine-wise sequence generation (no leakage)

**Results (Test Set):**  
- RMSE: ~17.54  
- MAE: ~12.84  

**Notes:**  
This version intentionally avoids architectural tricks (CNN, attention, ensembling) and serves as a reference baseline.  
Performance is comparable to early CMAPSS baseline methods and highlights the bias limitations of plain LSTMs, motivating later architectural improvements.

---

## Version 2 — Improved LSTM Training (FD001)

**Goal:**  
Refine the baseline LSTM model by improving training stability and convergence, while keeping the architecture intentionally simple.

**What Changed from Version 1:**  
- Increased model capacity (100 → 50 LSTM units)  
- Slightly deeper temporal representation  
- Longer, more stable training dynamics observed  
- Same data pipeline retained to ensure fair comparison

**Setup:**  
- Dataset: NASA CMAPSS FD001  
- Input: `(50, 24)` time-series sequences  
- Model: 2-layer LSTM with higher unit counts and dropout  
- Loss: Mean Squared Error  
- Metrics: RMSE, MAE  
- Split: Same engine-wise sequence generation (no leakage)

**Training Behavior:**  
- Smoother convergence compared to Version 1  
- Faster reduction in RMSE after mid-training epochs  
- Indicates improved temporal learning capacity

**Notes:**  
This version focuses purely on **training and capacity improvements**, without introducing architectural changes such as CNNs or ensembling.  
It serves as a stronger LSTM reference point and highlights that, while training refinements help, plain LSTMs still face bias limitations on CMAPSS FD001.
