Built a production-grade predictive maintenance system that transforms raw, highly imbalanced vehicle sensor telematics into actionable fleet routing decisions. Engineered an automated ETL stream (Bronze -> Silver -> Gold) to capture real-time mechanical degradation patterns, deployed an XGBoost engine to pinpoint at-risk assets with 99% confidence, and established a secure cloud data pipelines directly into Snowflake and Power BI to eliminate field breakdown overhead.

🏗️ System Architecture

                  +-----------------------------------+
                  |   Raw Telemetry CSV Ingestion     |
                  +-----------------------------------+
                                    |
                                    v

+-----------------------------------------------------------------------+
| SQLite3 Medallion DB |
| - BRONZE: Raw staging table & initial column mapping |
| - SILVER: DateTime parsing, 12h lookback windows, failure categorizing|
| - GOLD: Rolling averages, 1-step lags, multi-sensor delta metrics |
+-----------------------------------------------------------------------+
|
v
+-----------------------------------+
| ML Inference Engine (XGBoost) |
| - Isolates latest telemetry data |
| - Classifies "At Risk" vehicles |
+-----------------------------------+
|
v
+-----------------------------------+
| Snowflake Enterprise Data Warehouse|
| - Secure RSA Key-Pair Auth |
| - Optimized Pandas Bulk IO |
+-----------------------------------+
|
v
+-----------------------------------+
| Power BI Operational Dashboard |
| - Fleet-wide Risk Prioritization |
+-----------------------------------+

🛠️ Tech Stack
Data Engineering: Python, Pandas, NumPy, SQLite3

Machine Learning: XGBoost, Scikit-Learn, Joblib

Cloud Infrastructure: Snowflake Data Warehouse (via Private Key Authentication)

Business Intelligence: Power BI Desktop

📂 Repository Structure
Based on your exact layout, the repository is organized as follows:

├── dashboard_screenshots/
│ ├── brake.png # Micro-telemetry snapshot
│ ├── page1.png # Power BI: Fleet Overview
│ └── page2.png # Power BI: Machine Detail Deep-Dive
├── notebooks/
│ ├── 01_data_exploration.ipynb # EDA and initial profile validation
│ ├── 02_medallion_etl.ipynb # Bronze & Silver data modeling
│ ├── 03_feature_engineering.ipynb # Gold layer lag, delta, and rolling transforms
│ ├── 04_snowflake_push.ipynb # Initial warehouse staging script
│ ├── 05_model_training.ipynb # Model training & optimization with XGBoost
│ ├── 06_inference_pipeline.ipynb # Automated operational risk scoring engine
│ └── 07_loading_into_snowflake.ipynb# Secure key-pair data production pipeline
├── .gitignore # Prevents tracking credentials & large cache files
├── Fleet overview.pbix # Core Power BI development file
├── README.md # Documentation
└── requirements.txt # Project dependency map

⚙️ Step-by-Step Implementation
Exploration & Medallion Ingestion (01 - 02): Processes highly imbalanced telematics records (e.g., singular failure events vs. massive blocks of operational data). Maps labels to high-level system categories (Engine, Brake, Battery) and isolates a 12-hour lookback target window before historical failures occur.

Feature Engineering (03): Generates structural time-series signals including 1-step sensor shifts (\_lag1), rate-of-change states (\_delta1), 3-reading local rolling means, and a capped engine idling hour accumulator.

Modeling & Inference (05 - 06): Maximizes true positive alarms via an XGBClassifier. Filters data matrices to evaluate real-time deployment profiles, uncovering hidden risk states with high probability scores.

Secure Data Staging (04, 07): Bypasses clear-text credentials by utilizing an asymmetric cryptographic signature (rsa_key.p8) to batch push scored records directly into remote Snowflake environments via write_pandas.

Dashboard Layer: Bridges live warehouse views directly into desktop visualizations to surface instantaneous fleet updates.

🚀 Quick Start

1. Clone the Project
   git clone https://github.com/reddy-shreya/cat-fleet-predictive-maintenance.git
   cd cat-fleet-predictive-maintenance

2. Setup Environment
   pip install -r requirements.txt

3. Pipeline Execution Run
   The processing notebooks can be stepped through sequentially in numerical order (01 to 07) inside your Jupyter Notebook or Conda environment to rebuild the data layers, evaluate model weights, and stream inferences up to your analytical cloud layer.

Developed by Reddy-Shreya — 2026 Predictive Maintenance Systems Portfolio
