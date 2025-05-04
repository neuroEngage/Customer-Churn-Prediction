# 🚀 Customer Churn Prediction & Analysis

A full‑stack analytics solution that **extracts**, **models**, and **visualizes** telecom customer data to identify churn drivers and **predict** future churners—all in one seamless workflow.

---

## 🎯 Project Goals

1. **End‑to‑end ETL** in Microsoft SQL Server  
   - Create database, staging & production tables  
   - Cleanse, transform, and enrich raw churn data  
   - Define Views for “Churn Data” and “Joiner Data”

2. **Interactive Power BI Dashboard**  
   - Executive Summary with KPIs:  
     - **Total Customers**, **Total Churn**, **Churn Rate**, **New Joiners**  
   - Deep‑dive pages:  
     - **Summary** (demographics, geography, services)  
     - **Churn Reason** (tooltip‑driven breakdown)  
     - **Churn Prediction** (ML‑powered profile)

3. **Machine Learning Churn Model**  
   - **Random Forest** algorithm in Python (Jupyter)  
   - Label‑encoded features: demographics, services, usage, revenue, tenure  
   - 80/20 train‑test split, 84% overall accuracy  
   - Feature importance to guide business interventions

4. **Actionable Insights**  
   - Identify high‑risk segments (e.g. > 50 yrs, female, fiber‑optic users)  
   - Pinpoint churn drivers (competitor offers, device quality, service gaps)  
   - Deliver targeted marketing & retention strategies

---

## 📊 Power BI Dashboard Highlights

<p align="center">![Screenshot 2025-05-04 133541](https://github.com/user-attachments/assets/5a04f8ca-7a2f-4979-9633-70ca697843e1)

  <img src="images/summa![Screenshot 2025-05-04 133541](https://github.com/user-attachments/assets/edeaf492-38e4-41cd-a42a-e4192905a90c)
ry_screenshot.png" alt="Churn Analysis — Summary Page" width="800"/>
</p>

> **Summary Page**  
> • **New Joiners:** 411 • **Churn Rate:** 26.99%  
> • Demographic & Tenure buckets with churn overlays  
> • Top‑5 states by churn rate • Service‑level churn grid with heat bars  

<p align="![Churn_Analysis - Prediction Pafe](https://github.com/user-attachments/assets/c48f3532-520d-47b9-8900-9339e2e74716)
center">
  <img src="images/predic![Churn_Analysis - Prediction Page](https://github.com/user-attachments/assets/f9337f02-de49-40fe-95a7-c9022136411f)
png" alt="Churn Analysis — Prediction Page" width="800"/>
</p>

> **Prediction Page**  
> • **Predicted Churner Profile**: 246 female vs 132 male at risk  
> • **Total Predicted Churners:** 378  
> • Drill‑through grid: Customer ID, monthly charge, referral count  

---

## ⚙️ Architecture & Workflow

1. **Data Ingestion (SQL Server)**  
   ```sql
   -- Create staging table & import CSV via Flat File Wizard
   CREATE DATABASE db_churn;
   USE db_churn;
   -- Staging table
   CREATE TABLE STG_Churn (...);
   -- Import raw data, allow NULLs, set CustomerID as PK
   -- Clean & transform into PROD_Churn
   CREATE TABLE PROD_Churn AS
     SELECT
       COALESCE(...),
       CASE WHEN CustomerStatus='Churned' THEN 1 ELSE 0 END AS ChurnFlag,
       ...
     FROM STG_Churn;
   -- Create Views
   CREATE VIEW VW_Churn_Data AS
     SELECT * FROM PROD_Churn WHERE CustomerStatus IN ('Churned','Stayed');
   CREATE VIEW VW_Join_Data AS
     SELECT * FROM PROD_Churn WHERE CustomerStatus='Joined';
   ```

2. **Data Modeling (Power BI)**  
   - Connect to SQL Server in **Import** mode  
   - Custom columns: `ChurnFlag`, `MonthlyChargeRange`, `AgeGroup`, `TenureGroup`  
   - Explicit measures:  
     ```DAX
     TotalCustomers = COUNT( PROD_Churn[CustomerID] )
     TotalChurn     = SUM( PROD_Churn[ChurnFlag] )
     ChurnRate      = DIVIDE( [TotalChurn], [TotalCustomers] )
     NewJoiners     = CALCULATE( [TotalCustomers], PROD_Churn[CustomerStatus] = "Joined" )
     ```  
   - Pages & visuals, slicers, tooltips, theme aligned to brand.

3. **Machine Learning (Jupyter Notebook)**  
   ```python
   # Load views as pandas DataFrame
   df = pd.read_excel("Prediction_Data.xlsx", sheet_name="VW_Churn_Data")
   # Drop non‑predictive cols, encode categoricals
   X = df.drop(['CustomerID','ChurnCategory','ChurnReason','CustomerStatus'], axis=1)
   y = df['CustomerStatus'].map({'Stayed':0,'Churned':1})
   # Train-test split
   X_train, X_test, y_train, y_test = train_test_split(X,y, test_size=0.2, random_state=42)
   # Random Forest
   rf = RandomForestClassifier(n_estimators=100, random_state=42)
   rf.fit(X_train, y_train)
   y_pred = rf.predict(X_test)
   print(classification_report(y_test, y_pred))
   ```
   - **84% accuracy**, detailed precision/recall/f1 scores  
   - **Feature Importance** plot to prioritize interventions.

4. **Integration**  
   - Export prediction results to CSV  
   - Import into Power BI as **`prod_Predictions`** table  
   - Build **Churn Prediction** page using slicers & drill-through.

---

## 📂 Repository Structure

```
customer-churn-prediction/
├── data/
│   ├── Prediction_Data.csv
│   ├── Prediction_Data.xlsx
│   └── Customer_Data.csv
├── notebooks/
│   └── rf_customer_churn.ipynb
├── powerbi/
│   └── Customer_Churn_Prediction.pbix
├── documentation/
│   ├── Power_Query_Transformations.docx
│   └── Python_RF_Model.docx
├── queries/
│   └── SQL_Queries.docx
├── images/
│   ├── summary_screenshot.png
│   └── prediction_screenshot.png
├── README.md
└── .gitignore
```

---

## 🚀 How to Run

1. **Clone** the repo & navigate inside:  
   ```bash
   git clone https://github.com/yourusername/customer-churn-prediction.git
   cd customer-churn-prediction
   ```
2. **Power BI**: Open `powerbi/Customer_Churn_Prediction.pbix` in Power BI Desktop.  
3. **Jupyter**:  
   ```bash
   cd notebooks
   jupyter notebook rf_customer_churn.ipynb
   ```
4. **SQL Server**:  
   - Run the queries in `queries/SQL_Queries.docx` to recreate staging & prod tables.  

---



Feel free to explore, fork, and submit issues. Let’s turn insights into retention!  
