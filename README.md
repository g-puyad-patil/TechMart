# TechMart Retail Operations Analysis using Advanced SQL 📈

## Project Overview

This capstone project applies advanced SQL skills to analyze the operational and sales data of **TechMart**, a growing retail chain in North America and Europe. The goal of this analysis was to transform complex, multi-table datasets into clear, actionable business insights to support decision-making in areas like sales optimization, inventory management, and employee performance.

The project involved extensive data cleaning, complex joining of four interconnected tables, and the application of advanced SQL techniques, including **Common Table Expressions (CTEs)** and **Window Functions**.

***

## Business Understanding

The primary stakeholder is the **TechMart Leadership Team**. As a rapidly expanding business, TechMart needs to optimize its operations to sustain growth.

The key business questions addressed by this analysis include:
1.  Identifying the **top-performing product categories** and their correlation with stock levels.
2.  Assessing **employee sales performance** across different geographical locations.
3.  Analyzing **customer purchasing behavior** and loyalty program effectiveness.
4.  Tracking **sales trends** over time to predict demand.

***

## Data Understanding & Cleaning

The analysis utilized four interconnected tables from TechMart’s database:

| Table Name | Description | Key Challenges Handled |
| :--- | :--- | :--- |
| **Sales\_Transactions** | Detailed records of individual sales. | Missing values, incorrect data formats (e.g., "three" instead of 3). |
| **Product\_Details** | Product inventory, categories, and pricing. | Non-numeric values in the price column, missing stock levels. |
| **Employee\_Records** | Workforce details and store locations. | Missing values, non-numeric representations of sales figures. |
| **Customer\_Demographics** | Customer age, location, and loyalty status. | "nan" or "N/A" values in age and loyalty program fields. |

**Data Preparation:** Before analysis, a rigorous cleaning process was executed to standardize formats, convert non-numeric entries, and handle `NULL` values using techniques appropriate for each column.

***

## Data Analysis & Key Findings

The core of the analysis involved writing and optimizing complex SQL queries to combine and aggregate data across the four tables.

### Key Analytical Techniques Used:
* **Joins:** Extensive use of `INNER`, `LEFT`, and `FULL OUTER JOIN` to connect sales data with employee, customer, and product details.
* **CTEs (Common Table Expressions):** Used to modularize complex calculations, such as calculating average sales per employee or monthly revenue.
* **Window Functions:** Utilized to calculate **running totals of sales**, **moving averages**, and **ranking** employee performance within specific store locations.

### Main Insight
The most significant finding relates to **[Specific Topic, e.g., Regional Sales Performance / Product Category Profitability]**.

**Insight Summary:** [One-two sentences explaining the main insight. *Example: The Electronics category accounts for 60% of total revenue but is severely understocked in European stores, leading to significant lost sales opportunities.*]

### Key SQL Query
The following query was central to revealing this insight:

```sql
[-- Replace with your most significant SQL query]
WITH Monthly_Sales AS (
    SELECT
        strftime('%Y-%m', SaleDate) AS sale_month,
        Category,
        SUM(TotalAmount) AS monthly_revenue
    FROM
        Sales_Transactions AS st
    JOIN
        Product_Details AS pd ON st.ProductID = pd.ProductID
    GROUP BY 1, 2
)
SELECT
    sale_month,
    Category,
    monthly_revenue,
    SUM(monthly_revenue) OVER (PARTITION BY Category ORDER BY sale_month) AS running_total
FROM
    Monthly_Sales
ORDER BY
    Category, sale_month;
