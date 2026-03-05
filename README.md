# Smart Semiconductor Manufacturing Using IIoT Architecture

## 🎯 Project Summary

This project implements an **Industrial Internet of Things (IIoT) system** for real-time defect detection in semiconductor wafer manufacturing. The system uses a **5-layer IIoT architecture** with edge-based machine learning to predict wafer defects before expensive downstream processing and inspection.

**Key Performance Metrics:**
- **79.8% Recall** - Catches 67 out of 84 defects
- **69.8% Precision** - 70% of alarms are real defects  
- **<50ms Latency** - Real-time edge inference
- **94.5% Accuracy** - Overall prediction accuracy

**Business Impact:** Predicts defective wafers before inspection stage, saving costs in resources, manual labor, and downstream processing.

**Tech Stack:** `Python` `MQTT` `XGBoost` `PostgreSQL` `Streamlit` `Edge Computing`

---

## 🏗️ System Architecture

![system architecture](<IIOT Architecture.png>)

**5-Layer IIoT Architecture:**  
`Perception Layer (Sensors) → Network Layer (MQTT) → Edge Layer (ML Inference) → Platform Layer (PostgreSQL) → Application Layer (Dashboard)`

**Data Flow:**  
Sensor Data → MQTT (JSON) → Edge ML Processing → Database Storage → Real-time Visualization

---

## 📊 Machine Learning Model Performance

**Dataset:** 4,219 wafers total (60% train, 20% validation, 20% simulation)  
**Model:** XGBoost Classifier  
**Validation Set:** 844 wafers

### Performance Metrics

| Metric | Score | Interpretation |
|:-------|:-----:|:---------------|
| **Recall** | 79.8% | Catches 67 out of 84 defects |
| **Precision** | 69.8% | 70% of alarms are real
