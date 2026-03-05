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
| **Precision** | 69.8% | 70% of alarms are real defects |
| **Accuracy** | 94.5% | 798 out of 844 correct predictions |
| **F1-Score** | 0.744 | Balanced performance |

### Confusion Matrix

|  | **Predicted: No Defect** | **Predicted: Defect** |
|:---|:---:|:---:|
| **Actual: No Defect** | 731 (TN) | 29 (FP) |
| **Actual: Defect** | 17 (FN) | 67 (TP) |

**Results:**
- ✅ **True Positives (67):** Defects correctly identified
- ❌ **False Negatives (17):** Missed defects  
- ⚠️ **False Positives (29):** False alarms
- ✅ **True Negatives (731):** Good wafers correctly identified

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- PostgreSQL 13+
- MQTT Broker (HiveMQ Cloud)

### Installation

**1. Clone Repository**
```bash
git clone https://github.com/Sjschhabra/IIOT-Semiconductor-Manufacturing.git
cd IIOT-Semiconductor-Manufacturing
```

**2. Install Dependencies**
```bash
pip install pandas numpy scikit-learn xgboost paho-mqtt psycopg2-binary streamlit plotly
```

**3. Setup PostgreSQL Database**

Create database and run schema:
```sql
-- In PostgreSQL Run Query window
-- Paste entire query from database.txt
```

**4. Configure Database Credentials**

Update **both** `edge_gateway.py` (line 15) and `dashboard.py` (line 30):
```python
DB_CONFIG = {
    'host': 'localhost',
    'port': 5432,
    'database': 'your_database_name',     # Change this
    'user': 'postgres',
    'password': 'your_password'           # Change this
}
```

**5. Configure MQTT Credentials**

Update **both** `device_publisher.py` and `edge_gateway.py`:
```python
MQTT_BROKER = "your-broker.hivemq.cloud"
MQTT_PORT = 8883
MQTT_USERNAME = "your_username"
MQTT_PASSWORD = "your_password"
```

---

## ▶️ How to Run

**Prerequisites:**
- PostgreSQL database setup and connected
- Pretrained ML model (`edge_model.pkl`) is included
- Sensor data (`data_simulation.csv`) is included

**Run 3 Terminals Simultaneously:**

### Terminal 1: Edge Gateway (ML Inference Engine)
```bash
python edge_gateway.py
```
**Expected Output:**
```
Connected to MQTT Broker
Subscribed to topic: factory/line1/lithography
Subscribed to topic: factory/line2/etching
Subscribed to topic: factory/line3/deposition
Waiting for sensor data...
```

### Terminal 2: Sensor Simulator (Data Publisher)
```bash
python device_publisher.py
```
**Expected Output:**
```
Connected to MQTT Broker
Publishing to factory/line1/lithography
Published: {"wafer_id": "WAF12345", ...}
```

### Terminal 3: Real-time Dashboard
```bash
streamlit run dashboard.py
```
**Expected Output:**
```
Local URL: http://localhost:8501
```

---

## 📱 Dashboard Features

![Dashboard](<Dashboard.png>)

**Real-time Monitoring:**
1. **Production Line Status** - Live Idle/Running state with current wafer ID
2. **Active Alerts** - Red notification boxes for detected defects with acknowledgment
3. **Parameter Trends** - Live charts for chamber temperature, vacuum pressure, gas flow rate
4. **System Statistics** - Total wafers processed, defects detected, alert metrics

**Sample MQTT Payload:**
```json
{
  "wafer_id": "WAF12345",
  "production_line": "Lithography",
  "chamber_temperature": 245.3,
  "vacuum_pressure": 0.0023,
  "gas_flow_rate": 152.8,
  "rf_power": 485.2,
  "deposition_time": 185.4,
  "etch_rate": 42.7,
  "thickness": 1.85,
  "timestamp": "2025-12-01 19:45:32"
}
```

---

## 💰 Business Value & ROI

### Cost Savings Analysis

**Assumptions:**
- Factory processes 10,000 wafers/month
- Defect rate: 10% (1,000 defects/month)
- Optical inspection cost: $5 per wafer
- Model recall: 79.8%

| Scenario | Calculation | Monthly Cost |
|:---------|:------------|:-------------|
| **Without IIoT** | 1,000 defects × $5 | $5,000 |
| **With IIoT - Savings** | 798 defects caught × $5 | $3,990 saved |
| **With IIoT - False Alarm Cost** | 300 false alarms × $5 | $1,500 |
| **Net Monthly Savings** | $3,990 - $1,500 | **$2,490** |

**Annual ROI: ~$30,000**

---

## 📁 Project Structure
```
IIOT-Semiconductor-Manufacturing/
│
├── dashboard.py                # Streamlit real-time dashboard
├── device_publisher.py         # MQTT sensor data simulator
├── edge_gateway.py             # MQTT subscriber + ML inference
├── trained_model.py            # XGBoost model training script
├── edge_model.pkl              # Pretrained XGBoost model
│
├── data_train.csv              # Training set (2,531 wafers, 60%)
├── data_validation.csv         # Validation set (844 wafers, 20%)
├── data_simulation.csv         # Simulation set (844 wafers, 20%)
├── wafer_fault_detection.csv   # Complete dataset (4,219 wafers)
│
├── database.txt                # PostgreSQL schema
├── requirements.txt            # Python dependencies
└── README.md                   # Documentation
```

---

## ⚙️ Configuration

### Adjust Simulation Speed
**File:** `device_publisher.py` (line 85)
```python
time.sleep(8)  # Seconds per wafer (default: 8)
```

### Change Dashboard Refresh Rate
**File:** `dashboard.py` (line 45)
```python
refresh_rate = 2  # Seconds (range: 1-10)
```

---

## 📚 Dataset

**Source:** [Kaggle - Semiconductor Wafer Fault Detection](https://www.kaggle.com/datasets/programmer3/semiconductor-sensor-data-for-predictive-quality)

**Features:**
- Chamber temperature, vacuum pressure, gas flow rate
- RF power, deposition time, etch rate, thickness
- Binary classification: Defect / No Defect

---

## 📄 License

Educational project developed for MFG 598 (Industrial Internet of Things) course at Arizona State University.

---

## 👤 Contact

**Sameerjeet Singh Chhabra**  
MS Mechatronics, Robotics & Automation Engineering  
Arizona State University  

📧 sameerjeetsinghchhabra@gmail.com  
🌐 [Portfolio](https://sjschhabra.github.io/)  
💼 [LinkedIn](https://www.linkedin.com/in/sjschhabra/)  
💻 [GitHub](https://github.com/Sjschhabra)
