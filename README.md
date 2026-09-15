# 📊 Sales Insights Data Analysis – AtliQ Hardware

> A Power BI Sales Analytics project focused on transforming transactional sales data into meaningful business insights using **MySQL, Power Query, DAX and Power BI**.

This project is based on the **AtliQ Hardware Sales Insights learning case study**. I used the learning material to understand the business problem, dataset and overall analytics workflow, and independently implemented the **data exploration, SQL analysis, data cleaning, data modeling, DAX measures, dashboard design and presentation**.

The project follows an end-to-end analytics workflow:

**Business Problem → Planning → Data Discovery → SQL Analysis → Data Cleaning & ETL → Data Modeling → DAX → Power BI Dashboard → Business Insights**

---

## 📌 Table of Contents

* [Business Problem](#-business-problem)
* [Project Objectives](#-project-objectives)
* [Project Planning](#-project-planning)
* [Data Discovery](#-data-discovery)
* [SQL Analysis](#-sql-analysis)
* [Data Cleaning & ETL](#-data-cleaning--etl)
* [Data Modeling](#-data-modeling)
* [DAX Measures](#-dax-measures)
* [Power BI Dashboard](#-power-bi-dashboard)
* [Business Questions](#-business-questions)
* [Tools & Technologies](#-tools--technologies)
* [Repository Structure](#-repository-structure)
* [How to Run the Project](#-how-to-run-the-project)
* [Limitations](#-limitations)
* [Learning Outcomes](#-learning-outcomes)

---

# ❗ Business Problem

AtliQ Hardware is presented in the learning case study as an India-based company that supplies computer hardware and peripherals across different markets.

As the business grows, the Sales Director needs a better way to monitor sales performance. Sales information is spread across different markets, reporting involves manual work, and it can be difficult to compare sales performance from one place.

The main challenges addressed in this project are:

* Sales information is fragmented across different markets.
* Comparing market and customer performance is difficult.
* Product-level sales performance is not easily visible.
* Reporting requires manual data gathering and consolidation.
* Revenue, sales quantity and profitability need to be analyzed together.
* Profit-margin performance needs better visibility.
* Data contains quality issues such as blank markets and zero/negative sales values.
* Transactions contain both INR and USD currency values.
* Year-over-year performance comparison is required.
* A centralized interactive report is needed for easier analysis.

### Business Need

The project aims to provide an interactive Power BI report that helps users:

* Monitor overall sales performance.
* Compare different markets.
* Analyze customer and product performance.
* Understand revenue and profitability trends.
* Compare current performance with the previous year.
* Analyze performance against a selected profit target.
* Understand zone-level performance.
* Reduce manual reporting effort.
* Support data-driven decision-making.

---

# 🎯 Project Objectives

The main objectives of the project are:

1. Explore and understand the available sales data.
2. Analyze customers, products, markets, transactions, currencies and dates using MySQL.
3. Identify data-quality issues in the source data.
4. Clean and transform the data using Power Query.
5. Convert USD transactions into INR using the case-study conversion assumption.
6. Build a structured, star-schema-oriented data model.
7. Create reusable DAX measures for sales, revenue, profitability and contribution analysis.
8. Perform year-over-year analysis.
9. Create profit-target analysis.
10. Develop an interactive four-page Power BI report.
11. Present the analysis in a simple and business-friendly format.

---

# 🧭 Project Planning

The project planning follows an AIMS Grid approach.

### Purpose

To create a centralized sales analysis report that makes important sales and profitability information easier to understand and explore.

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
* Year-over-year comparison
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
Business Questions & Insights
```

### Project Execution Flow

![Project Execution Flow](./image/Flowchart.png)

---

# 🔎 Data Discovery

The dataset contains transaction-level sales information along with supporting customer, product, market and date tables.

The initial data exploration focused on understanding the available tables and identifying data-quality issues before creating the Power BI report.

## Available Tables

| Table                | Type      | Purpose                             |
| -------------------- | --------- | ----------------------------------- |
| `sales.transactions` | Fact      | Transaction-level sales information |
| `sales.customers`    | Dimension | Customer information                |
| `sales.products`     | Dimension | Product information                 |
| `sales.markets`      | Dimension | Market and geographic information   |
| `sales.date`         | Dimension | Date, month and year information    |

### Data Import

The database was imported into **MySQL Workbench** for initial exploration and SQL analysis.

![Data Import](./image/Data%20Import.png)

![Import Completed](./image/Import%20completed.png)

---

# 🧮 SQL Analysis

**MySQL** was used to explore the data, validate records and investigate data-quality issues before the Power BI transformation stage.

The SQL analysis covered:

### Customer Analysis

* Viewing customer records.
* Counting total customers.
* Understanding customer-level data.

### Market Analysis

* Checking market records.
* Finding transactions for specific markets.
* Comparing market-level sales.
* Investigating blank or invalid market records.

### Product Analysis

* Finding products sold in specific markets.
* Checking distinct product codes.
* Exploring product-level transaction data.

### Currency Analysis

* Identifying USD transactions.
* Checking INR and USD variations.
* Investigating inconsistent currency values.

### Time-Based Analysis

* Analyzing transactions for 2019 and 2020.
* Calculating yearly revenue.
* Calculating monthly revenue.
* Comparing revenue between different periods.

### Market Revenue Analysis

Revenue was also analyzed for individual markets such as:

* Chennai
* Mumbai
* Other available markets

The SQL analysis helped identify the data issues that were later handled during the Power Query transformation stage.

## SQL Files

The repository contains the SQL analysis files:

* [`sales_insights.sql`](./sales_insights.sql)
* [`sales_SQL_analysis.sql`](./sales_SQL_analysis.sql)

---

# 🧹 Data Cleaning & ETL

After the initial SQL analysis, the data was connected to **Power BI Desktop** and transformed using **Power Query**.

## ETL Workflow

```text
MySQL
  ↓
Power BI Connection
  ↓
Power Query
  ↓
Data Cleaning
  ↓
Currency Normalization
  ↓
Transformed Data
  ↓
Close & Apply
  ↓
Data Model
```

## Key Transformation Steps

### 1. Connect MySQL with Power BI

The MySQL database was connected to Power BI Desktop and the required tables were imported.

### 2. Clean Market Data

Blank and invalid market records were identified during the data-quality investigation.

Unwanted blank market records were filtered during the Power Query transformation.

### 3. Clean Transaction Data

The transaction table was checked for:

* Zero sales values.
* Negative sales values.
* Invalid records.
* Currency inconsistencies.

Unwanted zero and negative sales records were removed from the analysis.

### 4. Currency Normalization

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

This keeps INR values unchanged and converts USD values into INR.

### 5. Currency Standardization

Different representations were found in the source data, including:

```text
INR
INR\r
USD
USD\r
```

These variations were investigated during SQL analysis and unwanted variations were excluded during the cleaning process.

### 6. Load Transformed Data

After completing the required transformations, **Close & Apply** was used to load the cleaned data into Power BI.

The transformed data was then used for:

* Data modeling
* DAX measures
* KPI creation
* Dashboard development
* Business analysis

---

# 🧩 Data Modeling

The cleaned dataset was organized into a **star-schema-oriented model**.

The transaction table acts as the central fact table, while customers, products, markets and dates act as dimension tables.

## Fact Table

* `sales.transactions`

## Dimension Tables

* `sales.customers`
* `sales.products`
* `sales.markets`
* `sales.date`

## Supporting Tables

* `Key Measures`
* `Profit Target`

### Data Model

![Data Model](./image/Data%20model.png)

### Data Model & Key Measures

![Data Model and Key Measures](./image/Data%20Model%20%2B%20Key%20Measures.png)

This model supports filtering and analysis across:

* Markets
* Customers
* Products
* Zones
* Dates

It also provides a structured foundation for reusable DAX measures.

---

# 📐 DAX Measures

DAX was used to create reusable measures for KPIs, profitability, contribution analysis, year-over-year comparison and target analysis.

## Core Sales Measures

### Revenue

```DAX
Revenue =
SUM('sales transactions'[sales_amount])
```

Calculates total revenue.

### Sales Quantity

```DAX
Sales Qty =
SUM('sales transactions'[sales_qty])
```

Calculates total sales quantity.

### Total Profit Margin

```DAX
Total Profit Margin =
SUM('Sales transactions'[Profit_Margin])
```

Calculates the total profit-margin value.

### Profit Margin %

```DAX
Profit Margin % =
DIVIDE([Total Profit Margin], [Revenue], 0)
```

Calculates profit margin as a percentage of revenue.

---

## Contribution Analysis

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

Shows the revenue contribution of the current analytical context.

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

Shows the contribution of the current analytical context to total profit margin.

---

## Year-over-Year Analysis

### Revenue LY

```DAX
Revenue LY =
CALCULATE(
    [Revenue],
    SAMEPERIODLASTYEAR('sales date'[date])
)
```

Returns revenue for the corresponding period in the previous year.

The report also uses additional helper measures for the **Vs Last Year** KPI:

* `Revenue Growth Display`
* `Revenue Growth Color`
* `Sales Growth Display`
* `Sales Growth Color`

These measures help display current performance compared with the previous year directly in the KPI cards.

---

## Profit Target Analysis

### Profit Target

```DAX
Profit Target1 =
GENERATESERIES(-0.05, 0.15, 0.01)
```

Creates the selectable profit-target range.

### Profit Target Value

```DAX
Profit Target Value =
SELECTEDVALUE('Profit Target1'[Profit Target])
```

Returns the selected profit target.

### Target Difference

```DAX
Target Diff =
[Profit Margin %] - 'Profit Target1'[Profit Target Value]
```

Calculates the difference between actual profit margin and the selected target.

---

# 📊 Power BI Dashboard

The final report contains **four interactive pages**:

```text
Home
  ↓
Key Insights
  ↓
Profit Analysis
  ↓
Performance Insights
```

Each page focuses on a different part of the sales analysis.

---

# 🏠 Home

The **Home** page is the landing and navigation page of the report.

It provides navigation to the main analytical pages using the dashboard navigation design.

The page includes:

* Project branding
* Navigation buttons
* Page navigation
* Report layout and design elements

### Home Dashboard

![Home Dashboard](./image/Home.png)

The dashboard assets and navigation icons are available in the [`logo`](./logo/) folder.


---

# 📈 Key Insights

The **Key Insights** page provides an overall view of sales performance.

## KPIs

* Revenue
* Sales Quantity
* Total Customers
* Total Markets
* Vs Last Year

## Main Visuals

* Revenue by Market
* Sales Quantity by Market
* Revenue Trend
* Sales Quantity Trend
* Top 5 Products
* Top 5 Customers
* Year Filter
* Time-period analysis

### Business Focus

This page helps answer:

> **How much are we selling, where are we selling, how are sales changing over time, and which products and customers contribute most to sales?**

### Dashboard

![Key Insights Dashboard](./image/Key%20Insights.png)

---

# 💰 Profit Analysis

The **Profit Analysis** page focuses on revenue contribution and profitability.

## KPIs

* Revenue
* Sales Quantity
* Total Profit Margin
* Vs Last Year

## Main Visuals

* Revenue Contribution % by Market
* Profit Margin % by Market
* Profit Margin Contribution % by Market
* Revenue Trend
* Profitability Analysis
* Customer-level profitability analysis
* Year Filter
* Time-period analysis

### Business Focus

This page helps analyze:

* Revenue contribution by market.
* Profitability across markets.
* Profit margin differences.
* Customer-level revenue and profitability.

### Dashboard

![Profit Analysis Dashboard](./image/Profit%20Analysis.png)

---

# 🎯 Performance Insights

The **Performance Insights** page focuses on comparative and target-based performance analysis.

## KPIs

* Revenue
* Sales Quantity
* Total Profit Margin
* Vs Last Year
* Profit Target

## Main Visuals

* Profit Margin % by Market
* Revenue Trend
* Revenue LY comparison
* Revenue by Zone
* Profit Margin % by Zone
* Customer performance analysis
* Profit Target analysis

### Business Focus

This page helps evaluate:

* Current performance compared with the previous year.
* Actual profit margin against the selected target.
* Market performance.
* Zone-level performance.
* Customer-level performance.

### Dashboard

![Performance Insights Dashboard](./image/Performance%20Insights.png)

---
# 🎬 Overall Dashboard Walkthrough

The overall dashboard walkthrough is presented below.

### Dashboard Preview

![Overall Dashboard Walkthrough](./image/overall%20Dashboard-.gif)

---

# 🔍 Business Questions

The dashboard is designed to answer practical business questions.

## Sales Performance

* What is the overall revenue?
* What is the total sales quantity?
* How are sales changing over time?
* How does current performance compare with last year?

## Market Performance

* Which markets contribute the most revenue?
* Which markets contribute the most sales quantity?
* Which markets have stronger or weaker profit margins?

## Customer Performance

* Which customers contribute the most revenue?
* Which customers contribute most to profitability?
* Which customers show stronger or weaker performance?

## Product Performance

* Which products contribute most to sales?
* Which products appear among the top-performing products?

## Zone Performance

* How is revenue distributed across different zones?
* How does profit margin differ between zones?

## Profitability

* What is the total profit margin?
* What is the profit margin percentage?
* Which markets contribute most to profitability?
* Which customers contribute most to profitability?

## Target Analysis

* What is the selected profit target?
* Is actual profit margin above or below the selected target?
* What is the difference between actual performance and the selected target?

## Year-over-Year Analysis

* How does current revenue compare with the previous year?
* How do KPI values compare with last year?
* How does revenue performance change across years?

> The dashboard can be interacted with directly to explore the detailed values and relationships in the data.

---

# 🛠️ Tools & Technologies

| Tool / Technology    | Purpose                                                  |
| -------------------- | -------------------------------------------------------- |
| **MySQL**            | SQL data exploration and analysis                        |
| **MySQL Workbench**  | Database import and SQL analysis                         |
| **Power BI Desktop** | Data modeling, DAX and dashboard development             |
| **Power Query**      | Data cleaning and transformation                         |
| **DAX**              | KPI, profitability, contribution and target calculations |
| **GitHub**           | Project documentation and portfolio presentation         |

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

## 1. Open the Power BI Report

Download or clone the repository and open:

```text
Sales Insights of Data Analysis - AtliQ Hardware .pbix
```

using **Power BI Desktop**.

## 2. Review the Data Model

Open **Model View** in Power BI Desktop to inspect the relationships between the fact and dimension tables.

## 3. Review Power Query

Open:

**Home → Transform Data**

to inspect the cleaning, filtering and currency-normalization steps.

## 4. Review DAX Measures

Review the measures used for:

* Revenue
* Sales Quantity
* Profit Margin
* Contribution Analysis
* Revenue LY
* Vs Last Year
* Profit Target
* Target Difference

## 5. Review SQL Analysis

The SQL analysis files are available in the repository:

* [`sales_insights.sql`](./sales_insights.sql)
* [`sales_SQL_analysis.sql`](./sales_SQL_analysis.sql)

MySQL and MySQL Workbench can be used to reproduce the SQL-analysis stage.

## 6. Navigate the Dashboard

Use the report navigation to move between:

**Home → Key Insights → Profit Analysis → Performance Insights**

---

# ⚠️ Limitations

* The project uses a learning/case-study dataset rather than a live enterprise data source.
* USD-to-INR conversion uses the fixed assumption **1 USD = 75 INR**.
* The exchange-rate assumption is not a live foreign-exchange rate.
* Source-data quality limitations are part of the learning dataset.
* The report is not an official internal AtliQ Hardware system.
* The project is presented as a portfolio implementation and is not a production enterprise deployment.
* Automated refresh, governance, security and enterprise data-management processes are outside the current project scope.

---

# 🙌 Learning Outcomes

This project provided practical experience across the complete data analytics workflow.

### Technical Skills

* SQL
* MySQL
* MySQL Workbench
* Power Query
* ETL
* Data Cleaning
* Data Modeling
* DAX
* Power BI

### Analytics & BI Skills

* Data Exploration
* Data Visualization
* Business Intelligence
* Business Analysis
* Dashboard Design
* KPI Development
* Year-over-Year Analysis
* Profitability Analysis
* Target Analysis
* Data Storytelling

### End-to-End Workflow

Through this project, I practiced the complete process of:

**Understanding a business problem → Exploring data → Writing SQL queries → Cleaning data → Building a data model → Creating DAX measures → Developing a Power BI dashboard → Presenting business insights**

---


