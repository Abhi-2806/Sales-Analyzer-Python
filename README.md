# 📊 Sales Analyzer – Core Python Data Analysis Pipeline

*An end-to-end retail sales analytics and ETL pipeline built entirely with native Python data structures—zero external data manipulation libraries required.*

## 📌 Table of Contents

* [Project Overview](#project-overview)

* [Research Questions](#research-questions)

* [Key Performance Indicators (KPIs)](#key-performance-indicators-kpis)

* [Dataset Schema](#dataset-schema)

* [Project Structure](#project-structure)

* [Core Analytical Workflow](#core-analytical-workflow)

* [Detailed Analytical Findings](#detailed-analytical-findings)

* [How to Run This Project](#how-to-run-this-project)

* [Acknowledgment & Inspiration](#Acknowledgments-Inspiration)

* [Future Enhancements](#future-enhancements)

* [Author & Contact](#author--contact)

## 🎯 Project Overview

While libraries like Pandas and NumPy abstract away data manipulation with high-level methods such as `.groupby()` and `.describe()`, understanding how tabular data behaves at the memory and algorithmic level is critical for writing optimized ETL pipelines and SQL queries.

**Sales Analyzer** processes 1,000 retail transactional records from raw CSV logs using only **Python’s Standard Library** (`file I/O`, `nested lists`, `dictionaries`, `string parsing`, and `datetime`). The notebook ingests raw records, parses and type-casts fields, executes multi-dimensional revenue and volume aggregations, evaluates custom date-range queries, and exports partitioned departmental CSV reports.

## ❓ Research Questions

This project investigates four core commercial and operational questions across raw transaction logs:

1. **Category & SKU Revenue Distribution:** How is total revenue distributed across product categories and individual SKUs?

2. **Volume vs. Value Dynamics:** Which products drive the highest unit velocity (quantity sold) versus the highest monetary value?

3. **Customer Concentration:** Who are the top revenue-generating customers driving account concentration across the retail base?

4. **Temporal Velocity & Departmental Isolation:** How do daily sales fluctuate across custom date ranges, and how can specific departmental logs be isolated for downstream teams?

## 📈 Key Performance Indicators (KPIs)

| Metric / KPI | Value / Result | Analytical Context | 
| ----- | ----- | ----- | 
| **Total Revenue** | `₹2,66,62,151` (\~₹2.67 Cr) | Cumulative revenue across all 2023 transactions | 
| **Total Orders Processed** | `1,000` orders | Distinct order transactions (`OrderID` `1001`–`2000`) | 
| **Total Units Sold** | `3,129` items | Total physical item quantity across all categories | 
| **Top Revenue Category** | **Electronics** (`₹2,49,08,611`) | Captures **93.4%** of total business revenue | 
| **Peak Revenue SKU** | **Laptop** (`₹1,60,53,651`) | Drives **60.2%** of overall top-line sales (`349` units) | 
| **Peak Volume SKU** | **Notebook** (`355` units) | Highest unit velocity (`₹35,145` total revenue) | 
| **Top Customer Spend** | **Priya Singh** (`₹53,67,141`) | Highest individual account expenditure (`20.1%` share) | 
| **January 2023 Revenue** | `₹14,54,449` | Extracted via dynamic `datetime` range query (`2023-01-01` to `2023-01-31`) | 

## 🗂️ Dataset Schema

The source dataset (`sales_data.csv`) contains **1,000 transaction records** spanning **January 1, 2023 – December 31, 2023** across 7 attributes:

| Column Index | Field Name | Data Type | Description | 
| ----- | ----- | ----- | ----- | 
| `0` | `OrderID` | String | Unique transaction identifier (`1001` – `2000`) | 
| `1` | `Date` | String / Datetime | Transaction date (`YYYY-MM-DD`) | 
| `2` | `Customer` | String | Full name of the purchasing customer | 
| `3` | `Product` | String | Specific SKU / Item purchased | 
| `4` | `Category` | String | Product department (`Electronics`, `Furniture`, `Stationery`) | 
| `5` | `Quantity` | Integer | Number of units ordered per transaction | 
| `6` | `Price` | Integer | Unit price in INR (`₹`) | 

## 📁 Project Structure

```
Sales-Analyzer-Python/
│
├── README.md                  # Project documentation, research questions & KPI summary
├── SalesAnalyzer.ipynb        # Main Jupyter Notebook containing all ETL & analysis code
│
├── data/
│   └── sales_data.csv         # Raw input dataset (1,000 order records)
│
└── outputs/
    ├── electrical_sales.csv   # Filtered export of all Electronics transactions
    └── furniture_sales.csv    # Filtered export of all Furniture transactions

```

## ⚙️ Core Analytical Workflow

```
[ sales_data.csv ]
       │
       ▼
[ 1. File Ingestion & Parsing ]
   ├── Read raw lines via Python file I/O (`open()`, `.readlines()`)
   ├── Strip newline characters (`.strip()`) & split by delimiter (`.split(",")`)
   └── Skip header row & cast `Quantity` and `Price` to `int`
       │
       ▼
[ 2. In-Memory Aggregation Engine ]
   ├── Linear iteration across nested list structure (`NewData`)
   └── Dynamic hash-map (`dict`) accumulation for Category, Product, Customer & Date KPIs
       │
       ▼
[ 3. Temporal Filtering & ETL Export ]
   ├── String-to-date parsing via `datetime.strptime()` for custom date ranges
   └── Automated filtering & CSV generation for target product categories

```

## 🔍 Detailed Analytical Findings

### 1. Overall Business Baseline (Q1 – Q3)

* **Total Revenue Generated:** `₹2,66,62,151` (\~₹2.67 Crore)

* **Total Orders Processed:** `1,000` transactions

* **Total Units Sold:** `3,129` items

### 2. Category Revenue Breakdown (Q4)

* **Electronics:** `₹2,49,08,611` (**93.4%** of total revenue)

* **Furniture:** `₹17,02,880` (**6.4%** of total revenue)

* **Stationery:** `₹50,660` (**0.2%** of total revenue)

### 3. Product Performance: Revenue vs. Volume (Q5 & Q6)

* **Highest Revenue-Generating Product:** **Laptop** (`₹1,60,53,651` across `349` units — \~60.2% of overall revenue), followed by **Smartphone** (`₹54,87,657`) and **Printer** (`₹28,16,687`).

* **Highest Volume Product Sold:** **Notebook** (`355 units` sold), generating `₹35,145` in revenue.

* *Commercial Takeaway:* High unit velocity in low-ticket stationery items drives order frequency, whereas high-ticket electronics drive bottom-line revenue.

### 4. Customer Expenditure & Concentration (Q7)

* **Top Spending Customer:** **Priya Singh** (`₹53,67,141`)

* **Runner-Up Accounts:** **Neha Verma** (`₹53,46,104`) and **Rahul Mehta** (`₹53,06,609`)

* **Remaining Accounts:** **Anjali Chauhan** (`₹42,00,541`), **Rohit Gupta** (`₹32,84,600`), and **Amit Sharma** (`₹31,57,156`).

### 5. Time-Series & Date-Range Querying (Q8 & Q9)

* Computed daily revenue totals across all active transaction dates.

* Built an interactive date-range filter using `datetime.strptime()`—isolating **January 2023 (`2023-01-01` to `2023-01-31`)** yielded **`₹14,54,449`** in monthly revenue.

### 6. Automated Departmental CSV Exports (Q10)

* Programmatically filtered records by category and generated clean, standalone CSV deliverables (`electrical_sales.csv` and `furniture_sales.csv`) using both `map()` string joining and list comprehensions.

## 🚀 How to Run This Project

1. **Clone the repository:**

   ```
   git clone https://github.com/Abhi-2806/Sales-Analyzer-Python.git
   ```

2. **Ensure Python 3.x is installed:**
   No external `pip` installations are required—this project runs 100% on Python's built-in standard library (`datetime`, file I/O).

3. **Launch Jupyter Notebook:**

   ```
   Notebooks/SalesAnalyzer.ipynb
   
   ```

4. **Execute Cells:**

   * Ensure `sales_data.csv` is in the root working directory.

   * Run all cells sequentially. When prompted in **Q9**, input start and end dates in `YYYY-MM-DD` format (e.g., `2023-01-01` and `2023-01-31`).
  


## 🙏 Acknowledgments & Inspiration

* **Project Guidance & Dataset Concept:** Inspired by the Python data analysis tutorial by **[DataSteroid]** ([Complete Python for Data Analysis🔥(2025)]([https://www.youtube.com/watch?v=S-ThqsqjIb4])).


## 🛠️ Future Enhancements

* **Pandas & Vectorization Benchmark:** Refactor the native `for` loops into Pandas DataFrames (`groupby()`, `resample()`) to benchmark execution speed and memory usage on larger datasets (1M+ rows).

* **Modularization:** Convert notebook cells into reusable Python functions inside a dedicated `src/` module with exception handling (`try-except` blocks) and `with open()` context managers.

* **Visual Reporting:** Add Matplotlib/Seaborn visualizations to chart monthly revenue trends and category Pareto distributions.

## 👤 Author & Contact

**Abhishek Pathak** – *Data Analyst*

* 📧 **Email:** [wayoflife507@gmail.com](mailto:wayoflife507@gmail.com)

* 🔗 **LinkedIn:** [LinkedIn](https://www.linkedin.com/in/abhishekpathak2806/) 

* 💻 **Contact NO.:** (6307017308)
