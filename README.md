# 📊 Sales Insights Data Analysis – AtliQ Hardware

> A Power BI Sales Analytics project that transforms transactional sales data into meaningful business insights using **MySQL, Power Query, DAX and Power BI**.

This project is based on the **AtliQ Hardware Sales Insights learning case study**. I used the learning material to understand the business problem, dataset and analytics workflow, while independently implementing the **data exploration, SQL analysis, data cleaning, data modeling, DAX measures, dashboard design and presentation**.

> **Transparency:** The original AtliQ Hardware business case and learning material are not my original creation. This repository represents my own practical implementation and portfolio presentation based on publicly available learning resources.

---

## 🛠️ Tools Used

**MySQL • MySQL Workbench • Power Query • DAX • Power BI • GitHub**

---

# ❗ Business Problem

AtliQ Hardware is presented in the learning case study as an India-based company that supplies computer hardware and peripherals across different markets.

As the business grows, the Sales Director needs a better way to understand sales performance. Sales information is spread across different markets, and reporting involves manual data gathering.

This makes it difficult to get a clear view of:

* Sales performance
* Market performance
* Customer performance
* Product performance
* Revenue trends
* Profitability

### Business Need

The project aims to create an interactive Power BI report that helps users:

* Monitor sales performance
* Compare market performance
* Analyze customers and products
* Understand revenue and profitability trends
* Compare current performance with the previous year
* Compare profit margin with a selected target
* Analyze zone-level performance
* Reduce manual reporting effort

---

# 🎯 Project Objectives

The main objectives of this project are to:

1. Explore and understand the available sales data.
2. Analyze customers, products, markets, transactions and dates using MySQL.
3. Identify data-quality issues in the source data.
4. Clean and transform the data using Power Query.
5. Normalize USD transactions into INR using the case-study assumption.
6. Build a structured, star-schema-oriented data model.
7. Create reusable DAX measures for sales and profitability analysis.
8. Perform year-over-year analysis.
9. Create profit-target analysis.
10. Build an interactive four-page Power BI report.
11. Present the analysis in a simple and business-friendly format.

---

# 🧭 Project Planning

The project follows the **AIMS Grid approach** used in the learning case study.

### Purpose

Create a centralized sales analysis report that makes sales and profitability information easier to understand and explore.

### Stakeholders

* Sales Director
* Sales Team
* Marketing Team
* Customer Service Team
* Data & Analytics Team
* IT / Technical Team

### End Result

An interactive Power BI report containing:

* KPIs
* Charts
* Tables
* Filters
* Year-over-year analysis
* Profit-target analysis
* Page navigation

### Project Workflow

```text
Business Problem
       ↓
Project Planning
       ↓
Data Discovery
       ↓
SQL Analysis
       ↓
Data Cleaning & ETL
       ↓
Data Modeling
       ↓
DAX Measures
       ↓
Power BI Dashboard
       ↓
Business Analysis
```

### Project Execution Flow

![Project Execution Flow](./image/Flowchart.png)

---

# 🔎 Data Discovery

The dataset contains transaction-level sales information along with customer, product, market and date tables.

The first step was to understand the available tables and identify data-quality issues before creating the Power BI report.

## Available Tables

| Table                | Type      | Purpose                           |
| -------------------- | --------- | --------------------------------- |
| `sales.transactions` | Fact      | Transaction-level sales data      |
| `sales.customers`    | Dimension | Customer information              |
| `sales.products`     | Dimension | Product information               |
| `sales.markets`      | Dimension | Market and geographic information |
| `sales.date`         | Dimension | Date, month and year information  |

### Database Import

The database was imported into **MySQL Workbench** for initial exploration and SQL analysis.

![Data Import](./image/Data%20Import.png)

![Import Completed](./image/Import%20completed.png)

---

# 🧮 SQL Analysis

**MySQL** was used to explore the dataset, understand the table structure and identify data-quality issues before the Power BI transformation stage.

### Analysis Performed

#### Customer Analysis

* Viewed customer records
* Counted customers
* Explored customer-level data

#### Market Analysis

* Checked available markets
* Compared sales across markets
* Investigated blank or invalid market records

#### Product Analysis

* Explored available products
* Checked product codes
* Analyzed product-level transaction data

#### Currency Analysis

* Identified USD transactions
* Checked INR and USD values
* Investigated inconsistent currency values

#### Time-Based Analysis

* Analyzed 2019 and 2020 transactions
* Calculated yearly revenue
* Calculated monthly revenue
* Compared revenue across different periods

### SQL Files

* `sales_insights.sql`
* `sales_SQL_analysis.sql`

---

# 🧹 Data Cleaning & ETL

After the initial SQL analysis, the data was connected to **Power BI Desktop** and transformed using **Power Query**.

### ETL Workflow

```text
MySQL
  ↓
Power BI
  ↓
Power Query
  ↓
Data Cleaning
  ↓
Currency Normalization
  ↓
Close & Apply
  ↓
Data Model
```

## Key Cleaning Steps

### 1. Market Data

Blank and invalid market records identified during data exploration were filtered during the Power Query transformation.

### 2. Transaction Data

The transaction data was checked for:

* Zero sales values
* Negative sales values
* Invalid records
* Currency inconsistencies

Zero and negative sales records were removed from the analysis.

### 3. Currency Normalization

The source data contained both **INR and USD** transactions.

For this project, USD values were converted into INR using the fixed case-study assumption:

> **1 USD = 75 INR**

The Power Query transformation used:

```powerquery
= Table.AddColumn(
    #"Filtered Rows",
    "norm_sales_amount",
    each if [currency] = "USD"
         then [sales_amount] * 75
         else [sales_amount]
)
```

### 4. Currency Standardization

The source data contained variations such as:

```text
INR
INR\r
USD
USD\r
```

These variations were investigated during SQL analysis and handled during the data-cleaning process.

### 5. Load the Transformed Data

After completing the required transformations, **Close & Apply** was used to load the cleaned data into Power BI.

---

# 🧩 Data Modeling

The cleaned dataset was organized into a **star-schema-oriented data model**.

The transaction table acts as the central fact table, while customers, products, markets and dates act as dimension tables.

### Fact Table

* `sales.transactions`

### Dimension Tables

* `sales.customers`
* `sales.products`
* `sales.markets`
* `sales.date`

### Supporting Tables

* `Key Measures`
* `Profit Target 2`

### Data Model & Key Measures

![Data Model and Key Measures](./image/Data%20Model%20%2B%20Key%20Measures.png)

The model supports analysis across:

* Markets
* Customers
* Products
* Zones
* Dates

---

# 📐 DAX Measures

DAX was used in Power BI to create reusable measures for **sales KPIs, profitability, contribution analysis, year-over-year comparison, growth analysis and profit-target analysis**.

## 1. Core KPI Measures

### Revenue

Calculates total sales revenue.

```DAX
Revenue =
SUM('Sales transactions'[sales_amount])
```

### Sales Quantity

Calculates the total quantity of products sold.

```DAX
Sales Qty =
SUM('sales transactions'[sales_qty])
```

### Total Profit Margin

Calculates the total profit margin.

```DAX
Total Profit Margin =
SUM('Sales transactions'[Profit_Margin])
```

### Profit Margin %

Calculates profit margin as a percentage of revenue.

```DAX
Profit Margin % =
DIVIDE(
    [Total Profit Margin],
    [Revenue],
    0
)
```

---

## 2. Contribution Analysis

These measures are used to understand the contribution of different markets, customers and products to overall revenue and profit margin.

### Revenue Contribution %

```DAX
Revenue Contribution % =
DIVIDE(
    [Revenue],
    CALCULATE(
        [Revenue],
        ALL('sales products'),
        ALL('sales customers'),
        ALL('sales markets')
    )
)
```

### Profit Margin Contribution %

```DAX
Profit Margin Contribution % =
DIVIDE(
    [Total Profit Margin],
    CALCULATE(
        [Total Profit Margin],
        ALL('sales products'),
        ALL('sales customers'),
        ALL('sales markets')
    )
)
```

---

## 3. Year-over-Year Analysis

These measures compare the selected period with the corresponding period from the previous year.

### Revenue LY

```DAX
Revenue LY =
CALCULATE(
    [Revenue],
    SAMEPERIODLASTYEAR('sales date'[date])
)
```

### Sales Qty LY

```DAX
Sales Qty LY =
CALCULATE(
    [Sales Qty],
    SAMEPERIODLASTYEAR('sales date'[date])
)
```

### Profit Margin LY

```DAX
Profit Margin LY =
CALCULATE(
    [Total Profit Margin],
    SAMEPERIODLASTYEAR('sales date'[date])
)
```

---

## 4. Growth Analysis

These measures calculate the percentage change between the current period and the previous year.

### Revenue Growth %

```DAX
Revenue Growth % =
DIVIDE(
    [Revenue] - [Revenue LY],
    [Revenue LY]
)
```

### Sales Growth %

```DAX
Sales Growth % =
DIVIDE(
    [Sales Qty] - [Sales Qty LY],
    [Sales Qty LY]
)
```

### Profit Margin Growth %

```DAX
Profit Margin Growth % =
DIVIDE(
    [Total Profit Margin] - [Profit Margin LY],
    [Profit Margin LY]
)
```

---

## 5. Profit Target Analysis

A selectable profit-target range was created using a DAX-generated table.

### Profit Target 2

Creates target values from **-5% to 15%** with a **1% interval**.

```DAX
Profit Target 2 =
GENERATESERIES(
    -0.05,
    0.15,
    0.01
)
```

### Profit Target Value 2

Returns the currently selected profit target.

```DAX
Profit Target Value 2 =
SELECTEDVALUE(
    'Profit Target 2'[Profit Target]
)
```

### Target Difference

Calculates the difference between actual profit margin and the selected target.

```DAX
Target Diff =
[Profit Margin %]
    - 'Profit Target 2'[Profit Target Value 2]
```

---

# 📊 Power BI Dashboard

The final report contains **four pages**, with each page designed for a specific analytical purpose.

### Dashboard Pages

1. **Home** – Landing and navigation
2. **Key Insights** – Overall sales analysis
3. **Profit Analysis** – Profitability and contribution analysis
4. **Performance Insights** – Comparative and target analysis

---
# 🏠 Home View

In the **Home View**, buttons for all report views are available. Users can click on a button to navigate directly to the corresponding view page.

### Available Views

* **Key Insights**
* **Profit Analysis**
* **Performance Insights**

# 🎬 Overall Dashboard
![Overall Dashboard Walkthrough](./image/overall%20Dashboard-.gif)

---

# 📈 Key Insights 

The **Key Insights** page provides an overall view of sales performance.

### KPIs

* Revenue
* Sales Quantity
* Total Customers
* Total Markets
* Vs Last Year

### Visuals

* Revenue by Market
* Sales Quantity by Market
* Revenue Trend
* Sales Quantity Trend
* Top 5 Products
* Top 5 Customers
* Year Filter
* Time-period analysis

### Purpose

This page provides an overall view of **sales, markets, products, customers and sales trends**.

### Dashboard

![Key Insights Dashboard](./image/Key%20Insights.png)

---

# 💰 Profit Analysis

The **Profit Analysis** page focuses on revenue contribution and profitability.

### KPIs

* Revenue
* Sales Quantity
* Total Profit Margin
* Vs Last Year

### Visuals

* Revenue Contribution % by Market
* Profit Margin % by Market
* Profit Margin Contribution % by Market
* Revenue Trend
* Customer-level profitability analysis
* Year Filter
* Time-period analysis

### Purpose

This page focuses on **market contribution, profitability and customer-level performance**.

### Dashboard

![Profit Analysis Dashboard](./image/Profit%20Analysis.png)

---

# 🎯 Performance Insights

The **Performance Insights** page focuses on comparative and target-based analysis.

### KPIs

* Revenue
* Sales Quantity
* Total Profit Margin
* Vs Last Year
* Profit Target

### Visuals

* Profit Margin % by Market
* Revenue Trend
* Revenue LY comparison
* Revenue by Zone
* Profit Margin % by Zone
* Customer performance analysis
* Profit Target analysis

### Purpose

This page helps analyze **year-over-year performance, profit targets, market performance, zone performance and customer performance**.

### Dashboard

![Performance Insights Dashboard](./image/Performance%20Insights.png)

---

# 🔍 Business Questions

The dashboard was designed to answer practical business questions.

### Sales

* What is the overall revenue?
* What is the total sales quantity?
* How are sales changing over time?
* How does current performance compare with last year?

### Markets

* How is revenue distributed across markets?
* How does sales quantity vary across markets?
* How does profit margin differ across markets?

### Customers & Products

* Which customers contribute to revenue?
* How does profitability vary across customers?
* Which products appear in the Top 5?

### Zones

* How is revenue distributed across zones?
* How does profit margin vary across zones?

### Profitability

* What is the total profit margin?
* What is the profit margin percentage?
* How is profit contribution distributed across markets?

### Target Analysis

* What is the selected profit target?
* How does actual profit margin compare with the selected target?
* What is the difference between actual performance and the selected target?

---

# 📁 Repository Structure

```text
.
├── README.md
├── Sales Insights of Data Analysis - AtliQ Hardware .pbix
├── sales_insights.sql
├── sales_SQL_analysis.sql
│
├── image/
│   ├── Data Import.png
│   ├── Data Model + Key Measures.png
│   ├── Data model.png
│   ├── Flowchart.png
│   ├── Home.png
│   ├── Import completed.png
│   ├── Key Insights.png
│   ├── overall Dashbboard.mp4
│   ├── Performance Insights.png
│   └── Profit Analysis.png
│
└── logo/
    ├── Atliq logo.png
    ├── Customers.png
    ├── home.png
    ├── images.png
    ├── key insights.png
    ├── Markets.png
    ├── Performance insights.png
    ├── profit analysis.png
    ├── profit.png
    ├── Revenue.png
    └── sales qty.png
```

---

# ▶️ How to Run the Project

### 1. Open the Power BI File

Clone or download the repository and open:

```text
Sales Insights of Data Analysis - AtliQ Hardware .pbix
```

using **Power BI Desktop**.

### 2. Review the Data Model

Open **Model View** to inspect the relationships between the fact and dimension tables.

### 3. Review Power Query

Open:

**Home → Transform Data**

to inspect the data-cleaning and currency-normalization steps.

### 4. Review DAX Measures

Review the measures used for:

* Revenue
* Sales Quantity
* Profit Margin
* Contribution Analysis
* Year-over-Year Analysis
* Growth Analysis
* Vs Last Year
* Profit Target
* Target Difference

### 5. Review SQL

Open the following files using **MySQL Workbench**:

```text
sales_insights.sql
sales_SQL_analysis.sql
```

### 6. Explore the Dashboard

Navigate through:

```text
Home
   ↓
Key Insights
   ↓
Profit Analysis
   ↓
Performance Insights

---

# 🙌 Learning Outcomes

This project provided practical experience across the end-to-end data analytics workflow.


### End-to-End Workflow

```text
Business Problem
       ↓
Data Exploration
       ↓
SQL Analysis
       ↓
Data Cleaning
       ↓
Data Modeling
       ↓
DAX Measures
       ↓
Power BI Dashboard
       ↓
Business Analysis
```

This project helped me practice how raw transactional data can be transformed into an interactive business intelligence report using **SQL, Power Query, DAX and Power BI**.
