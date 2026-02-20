# 🌍 Global Energy Consumption Analytics Platform  
### AWS S3 ➜ Snowflake ➜ Tableau Cloud

---

## 📌 Project Overview

This project demonstrates an end-to-end modern cloud data analytics pipeline built to analyze **global household renewable energy consumption**.

The solution integrates:

- **AWS S3** → Raw Data Storage (Data Lake)
- **Snowflake** → Cloud Data Warehouse & Transformation Layer
- **Tableau Desktop & Tableau Cloud** → Business Intelligence & Visualization

The final output is an interactive **Energy Consumption Dashboard** that enables stakeholders to analyze energy usage (kWh), cost savings (USD), and renewable adoption trends across regions, countries, and income groups.

---

# 🏗 End-to-End Architecture

```
AWS S3 (Raw CSV Data)
        │
        ▼
Snowflake Storage Integration (IAM Role)
        │
        ▼
External Stage
        │
        ▼
Snowflake Data Warehouse
        │
        ▼
SQL Transformations (Business Logic)
        │
        ▼
Tableau Dashboard
        │
        ▼
Tableau Cloud Deployment
```

![Recording2026-02-20120403-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/603bd124-030c-48a6-bbfd-ee6c5ac6992c)

![EnergyConsumptionAnalysisusingAWSSnowflakeTableau-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/15a4aa77-d6a7-4852-914a-d355a8b85a69)

---

# ⭐ STAR Methodology

## 🟡 Situation
Stakeholders required visibility into global household renewable energy usage, cost savings, and demographic patterns. The organization lacked a scalable cloud-based analytics pipeline to process and visualize this data securely.

## 🟢 Task
Design and implement a cloud-native data pipeline to:
- Extract raw CSV data from AWS S3
- Load and transform it in Snowflake
- Apply conditional business rules based on income levels
- Deliver six core KPIs through an interactive Tableau dashboard

## 🔵 Action
- Provisioned AWS S3 bucket and configured IAM role with trust policies
- Created Snowflake `STORAGE INTEGRATION` for secure credential-less access
- Built external stage and loaded data using `COPY INTO`
- Created analytics table and applied income-based transformation logic
- Designed and formatted six professional visualizations in Tableau
- Published final dashboard to Tableau Cloud

## 🟣 Result
Delivered a fully operational **Cloud-Based Energy Analytics Platform** enabling:

- Region-wise consumption tracking
- Energy source performance comparison
- Demographic-based usage analysis
- Financial impact measurement (Cost Savings)

---

# 🔄 Project Flow

## 1️⃣ Data Storage (AWS S3)

- Created S3 bucket: `s3://tableaufinal.project/`
- Uploaded renewable energy dataset (CSV)
- Configured IAM role for Snowflake integration

---

## 2️⃣ Snowflake Integration & Data Loading

### 🔐 Storage Integration

```sql
CREATE OR REPLACE STORAGE INTEGRATION tableau_Integration
TYPE = EXTERNAL_STAGE
STORAGE_PROVIDER = 'S3'
ENABLED = TRUE
STORAGE_AWS_ROLE_ARN = 'arn:aws:iam::107388582545:role/tableau.role'
STORAGE_ALLOWED_LOCATIONS = ('s3://tableaufinal.project/');
```

---

### 📦 Create Database & Schema

```sql
CREATE DATABASE tableau;
CREATE SCHEMA tableau_Data;
```

---

### 📊 Create Target Table

```sql
CREATE TABLE tableau_dataset (
    Household_ID STRING,
    Region STRING,
    Country STRING,
    Energy_Source STRING,
    Monthly_Usage_kWh FLOAT,
    Year INT,
    Household_Size INT,
    Income_Level STRING,
    Urban_Rural STRING,
    Adoption_Year INT,
    Subsidy_Received STRING,
    Cost_Savings_USD FLOAT
);
```

---

### 📂 Create External Stage & Load Data

```sql
CREATE STAGE tableau.tableau_Data.tableau_stage
URL = 's3://tableaufinal.project'
STORAGE_INTEGRATION = tableau_Integration;

COPY INTO tableau_dataset
FROM @tableau_stage
FILE_FORMAT = (TYPE=CSV FIELD_DELIMITER=',' SKIP_HEADER=1)
ON_ERROR = 'CONTINUE';
```

---

# ⚙️ Data Transformation Layer

To preserve raw data integrity:

```sql
CREATE TABLE energy_consumption AS
SELECT * FROM tableau_dataset;
```

---

## 📈 Business Logic Transformations

### 🔹 Monthly Usage Adjustment by Income Level

```sql
-- Low Income
UPDATE energy_consumption
SET monthly_usage_kwh = monthly_usage_kwh * 1.1
WHERE income_level = 'Low';

-- Middle Income
UPDATE energy_consumption
SET monthly_usage_kwh = monthly_usage_kwh * 1.2
WHERE income_level = 'Middle';

-- High Income
UPDATE energy_consumption
SET monthly_usage_kwh = monthly_usage_kwh * 1.3
WHERE income_level = 'High';
```

---

### 🔹 Cost Savings Adjustment by Income Level

```sql
-- Low Income
UPDATE energy_consumption
SET cost_savings_usd = cost_savings_usd * 0.9
WHERE income_level = 'Low';

-- Middle Income
UPDATE energy_consumption
SET cost_savings_usd = cost_savings_usd * 0.8
WHERE income_level = 'Middle';

-- High Income
UPDATE energy_consumption
SET cost_savings_usd = cost_savings_usd * 0.7
WHERE income_level = 'High';
```

---

# 📊 Key Performance Indicators (KPIs)

The final Tableau Dashboard includes six major visualizations:

1. **kWh by Country**
2. **CSU (Cost Savings USD) by Country**
3. **kWh by Region**
4. **kWh by Energy Source**
5. **CSU by Region**
6. **CSU by Energy Source**

Additional Analytical Metrics:

- Total Energy Consumption (kWh)
- Total Cost Savings (USD)
- Income-Level Consumption Patterns
- Subsidy Adoption Impact
- Urban vs Rural Usage Trends
- Energy Source Contribution Analysis

---

# 📈 Key Insights

- 🌍 Europe and Africa show the highest regional energy consumption.
- 🌬 Wind and Hydro dominate global renewable energy usage.
- 💰 High-income households consume 20–30% more energy on average.
- 🏙 Urban households demonstrate stronger renewable adoption.
- 🎯 Subsidy programs significantly impact renewable uptake.

---

# 📖 Dataset Dictionary

| Column | Description |
|--------|-------------|
| Household_ID | Unique identifier |
| Region / Country | Geographic location |
| Energy_Source | Biomass, Geothermal, Hydro, Solar, Wind |
| Monthly_Usage_kWh | Energy consumption per month |
| Household_Size | Number of residents |
| Income_Level | Low / Middle / High |
| Urban_Rural | Household classification |
| Adoption_Year | Year renewable source adopted |
| Subsidy_Received | Government subsidy indicator |
| Cost_Savings_USD | Financial savings from renewable usage |

---

# 🧠 Skills Demonstrated

## ☁️ Cloud Engineering
- AWS S3 provisioning
- IAM role configuration
- Secure Snowflake storage integration

## 🏢 Data Warehousing
- Snowflake external stages
- COPY INTO bulk ingestion
- Table creation & cloning strategy

## 🧮 SQL Engineering
- Conditional transformations
- Data scaling logic
- Business rule implementation

## 📊 Business Intelligence
- Tableau dashboard design
- Clean UX formatting (no gridlines, hidden axes)
- Publishing to Tableau Cloud
- Layout container optimization

---

# 🚀 Business Impact

✔ Enables region-level renewable strategy planning  
✔ Identifies high-consumption demographic groups  
✔ Measures subsidy program effectiveness  
✔ Provides executive-ready dashboard for decision-making  
✔ Demonstrates scalable cloud-native analytics architecture  

---

# 🖼 Dashboard Preview


<img width="1874" height="795" alt="Screenshot 2026-02-20 115911" src="https://github.com/user-attachments/assets/7c3293ef-d3e6-46d0-8861-2d049fe3b54e" />



---

# 🔮 Future Improvements

- Implement Snowflake Streams & Tasks for automation
- Add incremental S3 data ingestion
- Build predictive energy consumption forecasting model
- Integrate real-time IoT energy sensor data
- Develop advanced demographic segmentation analysis

---

# 👨‍💻 Author

**Vignesh Raveendran**  
Data Engineer | Data Analyst

🔗 LinkedIn  
🔗 Portfolio  
