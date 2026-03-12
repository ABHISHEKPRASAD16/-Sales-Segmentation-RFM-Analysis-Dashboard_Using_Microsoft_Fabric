
# 🛒 Sales Segmentation – RFM Analysis Dashboard

> **Segmenting customers using Recency, Frequency, and Monetary value to drive targeted marketing strategies for a US-based Superstore (2021–2025).**

---

## 📌 Project Overview

This end-to-end data analytics project performs **RFM (Recency, Frequency, Monetary) Analysis** on the Superstore 2025 dataset. Customers are scored and segmented into meaningful groups — Champions, Loyal Customers, Big Spenders, At Risk, Lost, and Others — to help businesses make data-driven marketing decisions.

---

## 🖼️ Dashboard Preview

![RFM Dashboard](dashboard_preview.png)

---

## 🧰 Tech Stack

| Tool | Purpose |
|---|---|
| **Microsoft SQL Server (SSMS)** | Data storage & RFM SQL calculations |
| **Microsoft Fabric** | Cloud lakehouse, dataflow & pipeline orchestration |
| **Power Query (M Language)** | Data transformation & cleaning |
| **Power BI** | Interactive dashboard & visualizations |
| **DAX** | KPI measures (Total Sales, YoY, Avg Recency, etc.) |

---

## 📁 Project Structure

```
📦 RFM-Analysis
 ┣ 📂 Data
 ┃ ┗ 📄 Superstore_2025.csv          # Raw dataset (8,511 rows)
 ┣ 📂 SQL
 ┃ ┣ 📄 superstore_import.sql        # Table creation & BULK INSERT script
 ┃ ┗ 📄 RFM_Calculation_View.sql     # RFM scoring & segmentation logic
 ┣ 📂 PowerQuery
 ┃ ┗ 📄 Superstore_PowerQuery.txt    # M language transformation code
 ┣ 📂 DAX
 ┃ ┗ 📄 measures.dax                 # All DAX measures used in Power BI
 ┣ 📂 Dashboard
 ┃ ┗ 📄 RFM_Dashboard.pbix           # Power BI report file
 ┗ 📄 README.md
```

---

## 🔄 Project Architecture

```
CSV File
   │
   ▼
SQL Server (SSMS)
   │  Import & store raw data
   ▼
Microsoft Fabric Lakehouse
   │  RFM_Dataflow → RFM_Calculation → Semantic Model Refresh
   ▼
Power BI Semantic Model
   │  Calendar + Superstore 2025 + RFM_Matrix
   ▼
Power BI Dashboard
```

---

## 📊 RFM Scoring Logic

Each customer is scored **1–5** across three dimensions using `NTILE(5)`:

| Dimension | Description | Best Score |
|---|---|---|
| **Recency** | Days since last purchase (lower = better) | 1 |
| **Frequency** | Number of unique orders (higher = better) | 1 |
| **Monetary** | Total spend (higher = better) | 1 |

### Customer Segments

| Segment | Description |
|---|---|
| 🏆 **Champions** | Bought recently, buy often, spend the most |
| 💙 **Loyal Customers** | Frequent buyers with good recency |
| 💰 **Big Spenders** | High spend but less frequent |
| ⚠️ **At Risk** | Haven't purchased recently |
| ❌ **Lost** | Lowest scores across all dimensions |
| 🔵 **Others** | Everything else |

---

## 🧮 Key SQL Logic

```sql
-- RFM Scoring using NTILE
NTILE(5) OVER (ORDER BY Recency ASC)    AS Recency_Score,
NTILE(5) OVER (ORDER BY Frequency DESC) AS Frequency_Score,
NTILE(5) OVER (ORDER BY Monetary DESC)  AS Monetary_Score
```

---

## 📐 Key DAX Measures

```dax
-- Total Sales
Total Sales = SUM('Superstore 2025'[Sales])

-- Year-over-Year Growth
Sales YoY % = 
VAR LY = CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Calendar'[Date]))
RETURN DIVIDE([Total Sales] - LY, LY)

-- Dynamic Color for YoY
Sales YoY Color = 
VAR LY = CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Calendar'[Date]))
VAR YoY = DIVIDE([Total Sales] - LY, LY)
RETURN
    SWITCH(
        TRUE(),
        OR(ISBLANK(LY), LY = 0), "Orange",
        YoY > 0, "#00B200",
        "#FF0000"
    )
```

---

## 📈 Dashboard Features

- 💰 **Total Sales KPI** with YoY % change and color indicator
- 📅 **Monthly Trend Overview** — Sales & Customer count by month
- 🥧 **Segment Split by Customer** — Bar chart of all 6 segments
- 🛍️ **Category Split by Customers** — Office Supplies, Furniture, Technology
- 📋 **Customer Detail Table** — Filterable by City and Year
- 🎛️ **Slicers** — City & Year filters for dynamic exploration

---

## 🗂️ Dataset

- **Source:** Superstore 2025 (US Sales Data)
- **Rows:** 8,511 transactions
- **Period:** December 2021 – August 2025
- **Columns:** 20 (Order ID, Customer, Product, Sales, Profit, etc.)

---

## 🚀 How to Run

1. Clone this repository
2. Import `Superstore_2025.csv` into SQL Server using `superstore_import.sql`
3. Run `RFM_Calculation_View.sql` to create the RFM table
4. Upload data to Microsoft Fabric Lakehouse
5. Connect Power BI to the Fabric Semantic Model
6. Open `RFM_Dashboard.pbix` in Power BI Desktop

---

## 👤 Author

**Abhishek** — Data Analyst  
📧 Connect on [LinkedIn](#) | 🐙 [GitHub](#)

---

## 📄 License

This project is for portfolio and educational purposes only.
