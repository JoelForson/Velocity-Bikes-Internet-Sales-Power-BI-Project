# Velocity Bikes Internet Sales Power BI Project
==============================================

Project Overview
----------------

This project focuses on transforming Velocity Bikes' internet sales reporting from static Excel reports into dynamic, interactive dashboards using **Power BI**. The goal is to provide **Sales Managers** and **Sales Representatives** with actionable insights on product performance, customer behavior, and budget comparisons. The dashboards allow users to filter by customer, product, and time, enabling improved follow-ups, trend analysis, and performance tracking against budgeted goals.

* * * * *

Business Context
----------------

Velocity Bikes is a mid-sized bicycle manufacturer and retailer specializing in high-quality bikes and accessories sold both in-store and online. While online sales have grown steadily, the reporting process remained manual and Excel-based. Sales managers could not track trends effectively, and sales representatives lacked visibility into top-performing customers or products.

**Problem Statement:**

-   Static Excel reports hinder timely insights.

-   Sales trends, customer segmentation, and product performance are difficult to track.

-   Budget comparisons are disconnected and not visualized.

**Solution:**

-   Interactive **Power BI dashboards** integrated with the company CRM system.

-   Daily updates on sales metrics, product performance, and customer behavior.

-   Visual KPIs and trend graphs for budget vs actual comparison.

-   Filtering capabilities for sales reps by customer or product.

* * * * *

User Stories
------------

| No | Role | Request | Value | Acceptance Criteria |
| --- | --- | --- | --- | --- |
| 1 | Sales Manager | Dashboard overview of internet sales | Track which customers and products sell best | Power BI dashboard updates daily |
| 2 | Sales Representative | Detailed internet sales per customer | Prioritize high-value customers | Dashboard allows filtering per customer |
| 3 | Sales Representative | Detailed internet sales per product | Focus on top-selling products | Dashboard allows filtering per product |
| 4 | Sales Manager | Compare sales over time against budget | Monitor performance vs targets | Dashboard includes graphs/KPIs vs budget |

* * * * *

Data Sources
------------

1.  **CRM System:**

    -   Customer and sales transaction data

    -   Daily updated tables for internet sales

2.  **Excel Budgets:**

    -   2023 budget for products and customers

    -   Historical 2-year data for trend analysis

3.  **Dimensional Tables (SQL):**

    -   `DimCalendar2023-2025` for dates

    -   `DimProducts` for product metadata

    -   `DimCustomers` for customer metadata

4.  **Fact Table (SQL):**

    -   `FactInternetSales` containing transaction-level data including quantities, sales amounts, discounts, and date keys

* * * * *

Project Setup
-------------

### 1\. SQL Database Preparation

-   Extracted and cleaned sales data from the CRM system using SQL queries.

-   Joined sales, product, and customer tables to create a consolidated fact table.

-   Created dimensional tables (Date, Product, Customer) to enable filtering and aggregation.

**Example SQL Query to Pull Customer Data:**

```
-- Cleansed DIM_Customers Table --
SELECT 
  c.customerkey AS CustomerKey, 
  --      ,[GeographyKey]
  --      ,[CustomerAlternateKey]
  --      ,[Title]
  c.firstname AS [First Name], 
  --      ,[MiddleName]
  c.lastname AS [Last Name], 
  c.firstname + ' ' + lastname AS [Full Name], 
  -- Combined First and Last Name
  --      ,[NameStyle]
  --      ,[BirthDate]
  --      ,[MaritalStatus]
  --      ,[Suffix]
  CASE c.gender WHEN 'M' THEN 'Male' WHEN 'F' THEN 'Female' END AS Gender,
  --      ,[EmailAddress]
  --      ,[YearlyIncome]
  --      ,[TotalChildren]
  --      ,[NumberChildrenAtHome]
  --      ,[EnglishEducation]
  --      ,[SpanishEducation]
  --      ,[FrenchEducation]
  --      ,[EnglishOccupation]
  --      ,[SpanishOccupation]
  --      ,[FrenchOccupation]
  --      ,[HouseOwnerFlag]
  --      ,[NumberCarsOwned]
  --      ,[AddressLine1]
  --      ,[AddressLine2]
  --      ,[Phone]
  c.datefirstpurchase AS DateFirstPurchase, 
  --      ,[CommuteDistance]
  g.city AS [Customer City] -- Joined in Customer City from Geography Table
FROM 
  [AdventureWorksDW2022].[dbo].[DimCustomer] as c
  LEFT JOIN dbo.dimgeography AS g ON g.geographykey = c.geographykey 
ORDER BY 
  CustomerKey ASC -- Ordered List by CustomerKey

```

### 2\. Power BI Dashboard Construction

-   Imported fact and dimension tables into Power BI.

-   Created relationships:

    -   Date table → FactInternetSales (one-to-many)

    -   Customer table → FactInternetSales (one-to-many)

    -   Product table → FactInternetSales (one-to-many)

-   Imported **Excel budgets** and linked them to the Date and Product tables.

-   Designed visuals including:

    -   KPI cards for total sales, total budget, and variance

    -   Trend line graphs for sales over time

    -   Bar charts for product and customer performance

    -   Filters for month, product, and customer

### 3\. Measures (DAX)

-   Example measure to calculate Budget Amount without double-counting:

```
Budget Amount =
SUMX(
    VALUES('DimCalendar2023-2025'[DateKey]),
    CALCULATE(SUM(Budget[Budget]))
)

```

-   Example measure to calculate total sales:

```
Total Sales = SUM(FactInternetSales[SalesAmount])

```

-   Measures respect slicers and filters for month, product, and customer.

* * * * *

Dashboard Features
------------------

1.  **Daily Updated Dashboards:**

    -   Automatic refresh once per day to ensure timely insights.

2.  **Customer Analysis:**

    -   Filterable by individual customers to identify high-value clients.

    -   Displays top customers by sales volume and revenue.

3.  **Product Analysis:**

    -   Filterable by product to monitor top-selling items.

    -   Highlights sales trends and product performance over time.

4.  **Budget Comparison:**

    -   Shows variance between actual sales and budgeted amounts.

    -   Supports visual KPI cards and line graphs to quickly identify over- or under-performance.

5.  **Time Trend Analysis:**

    -   Historical comparison over 2 years to identify seasonal trends and patterns.

* * * * *

Business Impact
---------------

-   Sales managers now have a **real-time overview** of internet sales and can monitor performance against budget.

-   Sales representatives can **prioritize customers and products** based on actual performance data.

-   Reduction in manual reporting, freeing up resources for strategy and sales follow-up.

-   Enhanced decision-making based on interactive visuals and accurate data.

* * * * *

Tools & Technologies
--------------------

-   **Power BI:** Dashboard creation and visualization.

-   **SQL Server / SQL Queries:** Data extraction and transformation.

-   **CRM System:** Source of customer and sales data.

-   **Excel:** Budget data import for comparison.

-   **DAX:** Measures for dynamic calculations respecting filter context.

* * * * *

Future Enhancements
-------------------

-   Add predictive analytics to forecast sales per product and customer.

-   Implement drill-through pages for deeper insight into customer behavior.

-   Integrate with marketing campaigns to measure sales lift per campaign.

-   Expand KPI tracking to include inventory and supply chain metrics.

* * * * *

Contributors
------------

-   **Joel**, Data Analyst -- SQL queries, data modeling, and Power BI dashboard development

* * * * *

This README provides a **complete project context**, including business goals, data sources, methodology, dashboard features, DAX measures, and business impact.

* * * * *
