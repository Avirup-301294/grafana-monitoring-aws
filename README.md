# 🏦 Banking Data Generator & Observability Suite

This project provides a complete pipeline to generate synthetic banking data using **Python (Faker)**, ingest it into an **AWS RDS PostgreSQL** instance, and visualize the results via a **Grafana** dashboard.

---

## 🚀 Features
*   **Realistic Synthetic Data:** Generates customers, accounts, and transactions (Wire, ATM, POS).
*   **AWS RDS Integration:** Automated schema creation and batch data ingestion.
*   **Live Observability:** Pre-configured Grafana dashboard for transaction monitoring and KPI tracking.

---

## 🛠 Prerequisites
- **Python 3.10+**
- **AWS RDS PostgreSQL** instance (accessible via security groups).
- **Grafana** (Cloud or self-hosted).
- **Python Libraries:** `faker`, `psycopg2-binary`, `pandas`.

---

## 📂 Project Structure
```text
├── app.ipynb              # Python script for data generation & upload
├── queries.txt           # SQL file to initialize RDS tables
├── requirements.txt     # Python dependencies
└── dashboard.png       # Grafana dashboard export file
└── README.md  
```

## 🔧 Setup Instructions
### 1. Database Initialization
Connect to your RDS instance and run the following to create the target schema:
sql

```sql
CREATE TABLE customers (
    customer_id UUID PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    country VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE transactions (
    tx_id UUID PRIMARY KEY,
    customer_id UUID REFERENCES customers(customer_id),
    amount DECIMAL(12, 2),
    tx_type VARCHAR(20), -- 'DEBIT', 'CREDIT'
    status VARCHAR(20),  -- 'SUCCESS', 'FAILED', 'PENDING'
    tx_time TIMESTAMP
);
```

### 2. Install Dependencies

```bash
conda create -p venv python==3.10 -y
pip install -r requirements.txt
```
### 3. Run the Generator
The app.ipynb script uses Faker to create records and Psycopg2 to push them to RDS.
bash
# Set your environment variables
    export DB_HOST="://aws.com"
    export DB_NAME="postgres"
    export DB_USER="postgres"
    export DB_PASS="postgres"


## 📊 Grafana Dashboard Setup
1. Add Data Source: Navigate to Connections > Data Sources in Grafana and select PostgreSQL.
2. Configuration: Enter your RDS endpoint, database name, and credentials. Ensure "SSL Mode" is set to require for AWS RDS.
3. Import Dashboard:
    - Go to Dashboards > New > Import.
    - Upload the dashboard.json file.
    - Visualize: View real-time panels for:
    - Transaction Volume: Time-series graph of flow.
    - Failure Rates: Pie chart of Success vs. Failed transactions.
    - High-Value Alerts: Table of transactions over $10,000.

## 🛡 Disclaimer
This tool generates 100% synthetic data. It is intended for stress testing, demoing, and dashboard development only. Do not use for production financial modeling.
