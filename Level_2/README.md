# Level 2: SQL Exploratory Data Analysis (PostgreSQL)

## 📌 Project Overview
This module uses **PostgreSQL** (`pgAdmin`) to query, aggregate, and extract core business intelligence from the cleaned Global Superstore dataset (`sales_data`). Through 9 structured analytical queries, this phase uncovers top-performing markets, seasonal demand peaks, high-value customers, and product-level profitability bottlenecks.

---

## 🔍 Summary of SQL Queries & Exact Findings

| Query | Analytical Focus | Key Result / Finding |
| :--- | :--- | :--- |
| **Query 1** | Top 10 Profitable Products | **Canon Imageclass 2200 Advanced Copier** leads all SKUs with **$25,199.94** in total profit. |
| **Query 2** | Top 10 Customers by Sales | **Tom Ashbrook** is the #1 customer by revenue (**$40,489.00**), followed by Tamara Chand (**$37,453.00**). |
| **Query 3** | Region-Wise Total Sales | **Central** leads all 13 sub-regions with **$2,821,740.00** in sales, followed by **South** (**$1,600,600.00**). |
| **Query 4** | Category-Wise Average Profit | **Technology** achieves the highest average profit per order (**$65.42**), while **Office Supplies** is lowest (**$16.57**). |
| **Query 5** | Highest Discount Category | **Furniture** carries the highest average discount rate at **17.08%** (`0.1708`), eroding profit margins. |
| **Query 6** | Orders with Negative Profit | Identifies the top 20 loss-making transactions, led by order `CA-2013-108196` (**-$6,599.98** loss). |
| **Query 7** | Monthly Sales Trend | Revenue accelerates in H2 and peaks in **December ($1,580,816.00)** and **November ($1,551,319.00)**. |
| **Query 8** | Market-Wise Revenue & Profit | **APAC** generates the highest total revenue (**$3,581,785.00**) and profit (**$436,991.33**), followed by **EU** (**$2,937,166.00**). |
| **Query 9** | Sub-Category Performance | **Phones** leads revenue (**$1,706,842.00**), **Copiers** leads profit (**$258,174.42**), and **Tables** is the sole loss-making sub-category (**-$64,113.61**). |
| **Query 10** | Ship Mode Usage Analysis | **Standard Class** dominates shipping with **30,764 orders** (**$7,574,395.00** in sales), followed by **Second Class** (**10,304 orders**). |

---

## 💡 Key Business Insights Extracted via SQL
* **Highest Revenue Market:** **APAC** generates the highest total revenue at **$3,581,785.00** (`$436,991.33` profit), leading all global markets.
* **Margin Compression in Furniture & Tables:** **Furniture** receives the steepest average discount (**17.08%**), directly driving **Tables** into a net loss of **-$64,113.61** despite **$756,031.00** in sales.
* **Least Profitable Category per Order:** **Office Supplies** averages only **$16.57** in profit per order due to smaller basket sizes, shipping costs, and discounting.
* **Seasonal Q4 Surge:** Monthly sales climb steadily in the second half of the year, reaching peak volume in **November ($1.55M)** and **December ($1.58M)**.
* **Shipping Mode Preference:** **Standard Class** accounts for the overwhelming majority of shipments (**30,764 line items** generating **$7,574,395.00** in sales), showing customers prioritize lower shipping costs over expedited delivery (`Same Day` has only **2,699** orders).

---

## 🛠️ Complete SQL Queries Executed (Queries 1–10)

```sql
-- 1. Top 10 Profitable Products
SELECT Product_Name, SUM(Profit) AS Total_Profit
FROM sales_data
WHERE Product_Name IS NOT NULL
GROUP BY Product_Name
ORDER BY Total_Profit DESC
LIMIT 10;

-- 2. Top 10 Customers by Sales
SELECT Customer_Name, SUM(Sales) AS Total_Sales
FROM sales_data
WHERE Customer_Name IS NOT NULL
GROUP BY Customer_Name
ORDER BY Total_Sales DESC
LIMIT 10;

-- 3. Region-Wise Total Sales
SELECT Region, SUM(Sales) AS Total_Sales
FROM sales_data
WHERE Region IS NOT NULL
GROUP BY Region
ORDER BY Total_Sales DESC;

-- 4. Category-Wise Average Profit
SELECT Category, ROUND(AVG(Profit), 2) AS Avg_Profit
FROM sales_data
WHERE Category IS NOT NULL
GROUP BY Category
ORDER BY Avg_Profit DESC;

-- 5. Highest Discount Category
SELECT Category, ROUND(AVG(Discount), 4) AS Avg_Discount
FROM sales_data
WHERE Category IS NOT NULL
GROUP BY Category
ORDER BY Avg_Discount DESC
LIMIT 1;

-- 6. Orders with Negative Profit (Top 20 Losses)
SELECT Order_Id, Product_Name, Sales, Profit
FROM sales_data
WHERE Profit < 0
ORDER BY Profit ASC
LIMIT 20;

-- 7. Monthly Sales Trend
SELECT EXTRACT(Month FROM Order_Date) AS Month_Num,
       TO_CHAR(Order_Date, 'Month') AS Month_Name,
       SUM(Sales) AS Total_Sales
FROM sales_data
WHERE Order_Date IS NOT NULL
GROUP BY Month_Num, Month_Name
ORDER BY Month_Num;

-- 8. Market-Wise Revenue & Profit Analysis
SELECT Market, 
       SUM(Sales) AS Total_Revenue,
       SUM(Profit) AS Total_Profit
FROM sales_data
WHERE Market IS NOT NULL
GROUP BY Market
ORDER BY Total_Revenue DESC;

-- 9. Top Performing Sub-Categories
SELECT Sub_Category, 
       SUM(Sales) AS Total_Sales,
       SUM(Profit) AS Total_Profit
FROM sales_data
WHERE Sub_Category IS NOT NULL
GROUP BY Sub_Category
ORDER BY Total_Sales DESC;

-- 10. Ship Mode Usage Analysis
SELECT Ship_Mode, 
       COUNT(*) AS Total_Orders,
       SUM(Sales) AS Total_Sales
FROM sales_data
WHERE Ship_Mode IS NOT NULL
GROUP BY Ship_Mode
ORDER BY Total_Orders DESC;

```

---

## 📂 Folder Contents
* **`Task_1/`**: Core SQL aggregation and ranking scripts (`.sql`).
* **`Task_2/`**: Query execution screenshots (`Query_1.png` – `Query_10.png`) and data outputs.

* **`Task_3/`**: SQL Analysis Insights executive summary document.

*  📂 **[View All 10 SQL Query Output Screenshots (Level_2\Task_2)](./Task_2/)**

## ✅ Conclusion
This SQL analysis phase establishes a reliable analytical foundation for downstream visualization by querying *10 key business dimensions* across the sales_data table in *PostgreSQL. The findings reveal that **APAC ($3.58M)* and *EU ($2.94M)* drive the bulk of global revenue, *Standard Class* handles the majority of shipping volume (*30,764 items), and Q4 (November–December) delivers peak monthly sales. Most importantly, SQL aggregation isolates **Tables (-$64,113.61 net loss)* and high *Furniture discounting (17.08%)* as the primary threats to overall profitability—insights that directly guide the interactive *Power BI* executive dashboard in *Level 3*.

