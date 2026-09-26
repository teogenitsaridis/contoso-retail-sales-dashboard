Markdown
# Contoso Retail Sales & Performance Dashboard

An executive-level Power BI report delivering comprehensive visibility into retail performance, profit margins, and regional sales distribution. Designed with a custom Dark Theme with high-contrast Mint Green accents for optimal visual hierarchy and executive reporting.

---

## 📸 Dashboard Preview

<img width="100%"  alt="Screenshot 2026-09-26 213218" src="https://github.com/user-attachments/assets/03dff8b1-2e3c-45ea-b74b-08dfb645ebc5" />
<img width="100%"  alt="Screenshot 2026-09-26 205115" src="https://github.com/user-attachments/assets/2cbae493-074f-4731-973e-3a8b8f2eace7" />

<details>
<summary><b>🖼️ Click to view all report views & analysis pages</b></summary>
<br>
<img width="100%"  alt="Screenshot 2026-09-26 195809" src="https://github.com/user-attachments/assets/87a088f1-4d6d-4e2f-85cb-e88931459002" />
<img width="100%"  alt="Screenshot 2026-09-26 195840" src="https://github.com/user-attachments/assets/72dfe08c-6d69-4d29-bbb1-3dbc75dcee24" />
<img width="100%"  alt="Screenshot 2026-09-26 195923" src="https://github.com/user-attachments/assets/8feb036c-c9c4-4d8e-820d-b85a305a5e05" />  
<img width="100%"  alt="Screenshot 2026-09-26 200022" src="https://github.com/user-attachments/assets/9587ae85-f0d0-4b9f-add5-204dcc10679c" />
<img width="100%"  alt="Screenshot 2026-09-26 213256" src="https://github.com/user-attachments/assets/bbd778aa-1516-4cfe-9f39-3559ce12df92" />
<img width="100%"  alt="Screenshot 2026-09-26 213352" src="https://github.com/user-attachments/assets/80f9e2d3-1d5e-4c8b-8cdd-c36996d9fcf8" />
<img width="100%"  alt="Screenshot 2026-09-26 213430" src="https://github.com/user-attachments/assets/31649421-3eb3-49c7-8274-e608431eb8f5" />
<img width="100%"  alt="Screenshot 2026-09-26 213620" src="https://github.com/user-attachments/assets/527b867b-38cd-4f43-b616-a78fe30b0e39" />
<img width="100%"  alt="Screenshot 2026-09-26 194751" src="https://github.com/user-attachments/assets/6492cd42-cc79-4f2f-9622-bfb5b68ddc6d" />


</details>

---

## 🎯 Business Problem & Key Objectives
Retail management required a dynamic reporting tool to:
* **Track Revenue & Profitability:** Monitor top-line sales, cost breakdown, and gross margin trends across regions.
* **Identify Regional Drivers:** Pinpoint top-performing geographic areas via interactive filled maps and dynamic KPIs.
* **Granular Drill-Through Analysis:** Enable drill-through from high-level country summaries into regional and state-level drilldowns.

---

## 🧹 Data Pipeline, Extraction & Cleaning (Power Query)
The underlying dataset was sourced as raw, disjointed **CSV files** requiring extensive ETL processing prior to modeling:
* **Data Extraction:** Ingested multi-table transactional and master data from raw CSV files (`Sales`, `Stores`, `Customers`, `Products`).
* **Data Cleaning & Transformation:** 
  * Handled missing values, null handling, and removed redundant records.
  * Corrected and strictly defined data types (Dates, Currencies, Categorical IDs, Integers).
  * Standardized geographic entities and normalized text headers across store and customer tables to resolve location ambiguity.
* **Feature Engineering:** Derived operational calculated columns in Power Query and created a dedicated, continuous `Date Dimension` table to support Time Intelligence.

---

## 🏗️ Data Architecture & Star Schema
The cleaned data was loaded into a robust semantic model following Kimball Star Schema principles:
* **Fact Table:** `Sales` (transaction amounts, order quantities, unit costs, revenue).
* **Dimension Tables:** `Customer`, `Store`, `Product`, and `Date`.
* **Relationship Strategy:** Clean one-to-many (`1:*`) relationships with single-direction filtering for performant calculation paths and query optimization.

---


## 📊 DAX Calculations & Business Logic

### Core Business Metrics


```dax
Total Sales = SUM(Sales[SalesAmount])
```

```dax
Total Cost = SUM(Sales[TotalCost])
```

```dax
Total Profit = [Total Sales] - [Total Cost]
```

```dax
Profit Margin % = DIVIDE([Total Profit], [Total Sales], 0)
```

---

<details>
<summary><b>🔍 Click to expand: Time Intelligence & Advanced DAX Measures</b></summary>
<br>

```dax
Sales LY = 
CALCULATE(
    [Total Sales], 
    SAMEPERIODLASTYEAR('Date'[Date])
)
```

```dax
Sales YoY Growth % = 
VAR SalesDiff = [Total Sales] - [Sales LY]
RETURN
    DIVIDE(SalesDiff, [Sales LY], 0)
```

```dax
Total Orders = DISTINCTCOUNT(Sales[OrderKey])
```

```dax
Average Order Value = DIVIDE([Total Sales],[Total Orders],0)
```

```dax
Total Units Sold = SUM(Sales[Quantity])
```
</details>

---


## 🛠️ Tech Stack & Design Highlights
* **BI Tool:** Microsoft Power BI Desktop
* **ETL & Data Modeling:** Power Query (M), Data Cleansing & Transformation, Star Schema Architecture
* **Visuals & UX:** Filled Map with 3-point conditional formatting gradient (`#1E3A3A` to `#00F5A0`), custom dark KPI cards, cross-filtering, and dynamic drill-through pages.
