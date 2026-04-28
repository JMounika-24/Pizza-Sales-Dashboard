# Pizza-Sales-Dashboard

Recommended Structure and Order

1. Project Title / Headline
🍕 Pizza Sales Analytics: End-to-End SQL Server + Power BI Dashboard

A complete end-to-end data analytics project covering the full pipeline — from raw data ingestion and KPI validation in SQL Server, to building an interactive two-page sales performance dashboard in Power BI — analysing 48,620 pizza transactions across January to December 2015.

---

2.📌 Short Description / Purpose

The Pizza Sales Analytics project is a full-stack business intelligence solution built using MS SQL Server and Power BI Desktop. The project begins with importing and querying raw transactional data in SQL Server to validate all key business metrics, then connects that data directly to Power BI to deliver a visually rich, filterable two-page dashboard.

The dashboard covers revenue trends, order patterns, category and size breakdowns, and best/worst-performing product analysis. This project is intended for restaurant managers, food & beverage analysts, franchise owners, and aspiring data analysts looking to understand the complete workflow from SQL data preparation to Power BI reporting.

---

3. 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| 🗄️ MS SQL Server + SSMS | Backend database — import, store, and query raw CSV data. All KPI metrics validated here before connecting to Power BI |
| 📊 Power BI Desktop | Main visualization platform connected directly to SQL Server — KPI cards, charts, slicers, and report pages |
| 📂 Power Query | Data transformation inside Power BI — column formatting, data type corrections, table shaping |
| 🧠 DAX (Data Analysis Expressions) | Calculated measures: Total Revenue, Avg Order Value, Total Pizzas Sold, Total Orders, Avg Pizzas per Order |
| 📁 Pizza_Sale.csv | Original source file — imported into SQL Server for querying and connected to Power BI |

---

4. 📂 Data Source

Source: `Pizza_Sale.csv` — internal restaurant transactional data

Records: 48,620 pizza orders | Period: January – December 2015

Columns include: `order_id`, `order_date`, `pizza_name`, `pizza_category`, `pizza_size`, `quantity`, `unit_price`, `total_price`

 Pipeline:
```
📄 Pizza_Sale.csv  →  🗄️ SQL Server Import  →  ✅ SQL Query & Validate  →  📊 Power BI Connect  →  📈 Dashboard Build
```

---

5. 🗄️ Stage 1 — SQL Server: Data Import & KPI Validation

The raw CSV was first imported into MS SQL Server. All key business metrics were written as SQL queries and validated in SSMS before connecting to Power BI. This ensures every KPI visible in the dashboard is backed by verified SQL logic.

 ▶️ Steps to Set Up

1. Open SSMS → connect to your local server (e.g. `localhost` or `.\SQLEXPRESS`)
2. Right-click Databases → New Database → name it `pizza_db`
3. Right-click the database → Tasks → Import Flat File → select `Pizza_Sale.csv` → map columns → finish import
4. Table created: `pizza_sales`
5. Run the queries below to validate all KPIs

---

 📊 KPI Queries

```sql
-- Total Revenue
SELECT SUM(total_price) AS Total_Revenue
FROM pizza_sales;
```

```sql
-- Avg Order Value
SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_Order_Value
FROM pizza_sales;
```

```sql
-- Total Pizzas Sold
SELECT SUM(quantity) AS Total_Pizzas_Sold
FROM pizza_sales;
```

```sql
-- Total Orders
SELECT COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales;
```

```sql
-- Avg Pizzas Per Order
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) /
  CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2))
AS Avg_Pizzas_Per_Order
FROM pizza_sales;
```

---

 📅 Trend Queries

```sql
-- Daily Order Trend
SELECT DATENAME(DW, order_date) AS Order_Day,
  COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY DATENAME(DW, order_date)
ORDER BY Total_Orders DESC;
```

```sql
-- Monthly Order Trend
SELECT DATENAME(MONTH, order_date) AS Month_Name,
  COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY DATENAME(MONTH, order_date)
ORDER BY Total_Orders DESC;
```

---

 🍕 Category & Size Queries

```sql
-- % Sales by Pizza Category
SELECT pizza_category,
  CAST(SUM(total_price) AS DECIMAL(10,2)) AS Total_Revenue,
  CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS Pct_Sales
FROM pizza_sales
GROUP BY pizza_category
ORDER BY Pct_Sales DESC;
```

```sql
-- % Sales by Pizza Size
SELECT pizza_size,
  CAST(SUM(total_price) AS DECIMAL(10,2)) AS Total_Revenue,
  CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS Pct_Sales
FROM pizza_sales
GROUP BY pizza_size
ORDER BY Pct_Sales DESC;
```

---

 🏆 Best / Worst Sellers Queries

```sql
-- Top 5 Pizzas by Revenue
SELECT TOP 5 pizza_name,
  SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue DESC;
```

```sql
-- Bottom 5 Pizzas by Revenue
SELECT TOP 5 pizza_name,
  SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue ASC;
```

```sql
-- Top 5 Pizzas by Quantity Sold
SELECT TOP 5 pizza_name,
  SUM(quantity) AS Total_Qty_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Qty_Sold DESC;
```

```sql
-- Bottom 5 Pizzas by Quantity Sold
SELECT TOP 5 pizza_name,
  SUM(quantity) AS Total_Qty_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Qty_Sold ASC;
```

```sql
-- Top 5 Pizzas by Total Orders
SELECT TOP 5 pizza_name,
  COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders DESC;
```

```sql
-- Bottom 5 Pizzas by Total Orders
SELECT TOP 5 pizza_name,
  COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders ASC;
```

---

 📊 Stage 2 — Power BI: Dashboard & Visualization

 ▶️ Steps to Connect SQL Server to Power BI

1. Open Power BI Desktop → click Get Data
2. Select SQL Server → enter server name (e.g. `localhost`) and database `pizza_db` → click **OK**
3. Select the `pizza_sales` table from Navigator → click Load
4. Open Power Query if any column formatting or data type fixes are needed
5. Build KPI cards, charts, and slicers using DAX measures
6. Cross-check: Verify all Power BI KPI values match SQL query outputs from SSMS exactly

---

 🔢 Key KPIs (Top Row — Both Pages)

| Metric | Value |
|--------|-------|
| 💰 Total Revenue | $817.9K |
| 🧾 Avg Order Value | $38.31 |
| 🍕 Total Pizzas Sold | 49,574 |
| 📦 Total Orders | 21,350 |
| 📊 Avg Pizzas / Order | 2.32 |

Insight Ticker highlights:
`Busiest Day: Friday` · `Peak Month: July` · `Top Category: Classic` · `Top Size: Large (45.9%)` · `Best Seller: Thai Chicken Pizza` · `Worst Seller: Brie Carre Pizza`

---

 🖥️ Dashboard Pages

 Page 1 — Overview

| Visual | Type | Insight |
|--------|------|---------|
| Daily Trend — Total Orders | Bar Chart | Fridays & Thursdays peak; Sundays slowest |
| Monthly Trend — Total Orders | Line Chart | July highest; year-end decline Oct–Dec |
| % Sales by Pizza Category | Donut Chart | Classic leads revenue share |
| % Sales by Pizza Size | Donut Chart | Large dominates at 45.9% |
| Pizzas Sold by Category | Bar Chart | Classic & Supreme outsell Chicken & Veggie |

 Page 2 — Best / Worst Sellers

| Visual | Type | Insight |
|--------|------|---------|
| Top 5 by Revenue | Bar Chart | Thai Chicken Pizza leads at ~$43K |
| Bottom 5 by Revenue | Bar Chart | Brie Carre, Spinach Supreme lowest |
| Top 5 by Quantity Sold | Bar Chart | Classic Deluxe, Hawaiian, Thai Chicken |
| Bottom 5 by Quantity Sold | Bar Chart | Brie Carre, Calabrese, Soppressata |
| Top 5 by Total Orders | Bar Chart | Classic Deluxe, Pepperoni, Thai Chicken |
| Bottom 5 by Total Orders | Bar Chart | Brie Carre, Calabrese, Chicken Pesto |

---

💡 Business Impact & Insights

- Menu Optimisation — Brie Carre Pizza ranks last across all three Bottom 5 metrics (revenue, quantity, orders) — a strong candidate for menu review or discontinuation.
- Revenue Growth — Thai Chicken and California Chicken are the top earners. Featuring them in promotions or combo deals can directly lift average order value.
- Staffing & Operations — Friday and Thursday demand peaks enable precise scheduling of kitchen staff and delivery resources on high-volume days.
- Promotional Planning — Monthly trend dips in September–October and December signal ideal windows for targeted discount campaigns to sustain revenue.
- Upsell Opportunities — Large size drives nearly 46% of revenue, highlighting a clear upsell path from Medium to Large for improved margin per order.
- Data Credibility — All Power BI KPI values were cross-verified against SQL Server query outputs, ensuring 100% accuracy across the dashboard.

---

 📸 Dashboard Preview

 Page 1 — Overview
  Pizza  Sales/Screenshort1.png

 Page 2 — Best / Worst Sellers
  Pizza  Sales/Screenshort2.png

---

📁 Files in this Repository

| File | Description |
|------|-------------|
| `Pizza_Sales.sql`     | SQL queries for KPI validation — Revenue, Orders, Trends, Top/Bottom 5 sellers |
| `Pizza_Sale.csv`      | Raw transactional data source |
| `Pizza_Sales.pbix`    | Power BI report file |
| `Screenshort1.png`    | Dashboard preview — Overview page |
| `Screenshot2.png`     | Dashboard preview — Best/Worst Sellers page |
| `README.md`           | Project documentation |

---

Data Period: Jan – Dec 2015 · 48,620 transactions analysed · Built with SQL Server + Power BI

