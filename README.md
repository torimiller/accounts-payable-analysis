# Accounts Payable Analysis

A data analysis portfolio project exploring accounts payable operations through data cleaning, exploratory analysis, SQL querying, and interactive dashboards.

---

## Table of Contents


- [Overview](#overview)
- [Business Problem](#business-problem)
- [Key Features](#key-features)
- [Dataset](#dataset)
- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Data Cleaning & Preparation](#data-cleaning--preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Insights](#insights)
- [Dashboard](#dashboard)
- [How to Run This Project](#how-to-run-this-project)
- [Final Recommendations](#author--contact")


## Overview

This project analyzes accounts payable data to uncover spending patterns, vendor behavior, payment performance, and cash flow trends. The goal is to give finance and operations teams a clearer picture of where money is going, how reliably invoices are paid, and where process improvements may be warranted.

## Business Problem

Accounts payable teams often lack visibility into their own data — which vendors are driving the most spend, whether invoices are being paid on time, and how cash is flowing across projects. This project addresses those gaps by consolidating invoice, payment, vendor, and project data into a structured analytical framework.

## Key Features

- End-to-end data cleaning and type standardization across five related datasets
- SQL-based analysis with reusable views built for Power BI connectivity
- Six analytical focus areas: AP spend overview, vendor spend, project costs, invoice charge breakdown, payment performance, and cash flow trends
- Outlier detection using the IQR method to flag high-spend vendors
- Visualizations built in Matplotlib for exploratory analysis

## Dataset

The project uses five CSV files representing a relational AP dataset:

| File                  | Description |
|-----------------------|-------------|
| `invoices.csv`        | Invoice records including dates, amounts, and approval status |
| `invoice_charges.csv` | Line-level charge detail linked to invoices and projects |
| `payments.csv`        | Payment records linked to invoices |
| `projects.csv`        | Project metadata including type, status, manager, and budget |
| `vendors.csv`         | Vendor information including type, payment terms, and active status |

## Technologies Used

- **Python** — pandas, NumPy, Matplotlib
- **SQL** — MySQL via SQLAlchemy and PyMySQL
- **Jupyter Notebooks** — data cleaning and EDA
- **Power BI** — interactive dashboard (connected to MySQL database)
- **dotenv** — environment variable management for database credentials

## Project Structure

```
├── Dashboard/
│   └── ap_dashboard.pbix         # Power BI dashboard file
├── Data/
│   ├── Cleaned/                  # Cleaned CSVs and pickle files
│   │   ├── invoice_charges_cleaned.csv
│   │   ├── invoice_charges_cleaned.pkl
│   │   ├── invoices_cleaned.csv
│   │   ├── invoices_cleaned.pkl
│   │   ├── payments_cleaned.csv
│   │   ├── payments_cleaned.pkl
│   │   ├── projects_cleaned.csv
│   │   ├── projects_cleaned.pkl
│   │   ├── vendors_cleaned.csv
│   │   └── vendors_cleaned.pkl
│   └── Original/                 # Raw CSV source files
│       ├── invoice_charges.csv
│       ├── invoices.csv
│       ├── payments.csv
│       ├── projects.csv
│       └── vendors.csv
├── Notebooks/
│   ├── data_cleaning.ipynb
│   └── exploratory_data_analysis.ipynb
├── Scripts/
├── .env                          # MySQL credentials (not committed)
├── .env.example                  # Example env file for setup reference
├── .gitignore
└── README.md
```

## Data Cleaning & Preparation

The `data_cleaning.ipynb` notebook standardizes all five datasets before analysis. Steps applied to each table:

- **Row counts and shape** verified for each dataset
- **Duplicate checks** performed across all tables (none found)
- **Missing value audits** — expected nulls (e.g., `approved_date` for pending invoices, `end_date` for active projects) were documented rather than removed
- **Data type corrections** applied consistently:
  - Date columns converted using `pd.to_datetime` with `format='mixed'`
  - ID and name columns cast to `string`
  - Categorical fields (e.g., `approval_status`, `vendor_type`, `payment_terms`) cast to `category`
  - Financial fields cast to `float64`

Cleaned data is exported as both `.pkl` files (to preserve pandas dtypes between notebooks) and `.csv` files (for repository transparency).

## Exploratory Data Analysis

The `exploratory_data_analysis.ipynb` notebook loads the cleaned data into a local MySQL database and performs analysis across six areas:

### 1. AP Spend Overview
High-level summary of total invoice spend, total paid vs. unpaid amounts, percent paid, vendor count, project count, and projects with AP charges. A `vw_ap_spend_detail` view is created for Power BI consumption.

### 2. Vendor Spend Analysis
Ranks vendors by total invoice amount, invoice count, and average invoice size. Uses the IQR method to flag spend outliers, and a scatter plot to distinguish whether high total spend is driven by invoice volume or invoice size. A `vw_vendor_detail` view is created for Power BI.

### 3. Project Cost Analysis
Identifies which projects generate the highest vendor costs, whether spend is concentrated in a few projects, and which project types drive the most AP activity. Includes a horizontal bar chart of the top 10 projects by vendor cost.

### 4. Invoice Charge Breakdown
Examines charge-level detail within the costliest projects — which vendors contribute to high-cost projects, how charges are distributed (many small vs. few large), and which specific charges stand out.

### 5. Payment Performance
Analyzes how long it takes to pay invoices, how often payments are late, and which vendors are most impacted by late payments. Metrics include average days to pay, late payment rate, and late paid invoice amount by vendor and vendor type.

### 6. Cash Flow Trends
Tracks monthly AP cash outflows to identify seasonal patterns, peak payment months, and whether outflows are driven more by certain vendor types. Visualized as a monthly line chart.

## Insights

*To be updated after dashboard completion.*

## Dashboard

Built in Power BI, connected directly to MySQL tables (dim_vendors, dim_projects, fact_invoices, fact_invoice_charges, dim_date). Metrics are calculated using DAX measures in a dedicated `_Measures` table, including:

- **Invoice Count** — total number of invoices
- **Payment Status** — categorization of invoices by paid, partially paid, or unpaid
- **Percent Paid** — share of total invoice spend that has been paid
- **Total AP Spend** — sum of all invoice amounts
- **Total Outstanding** — remaining unpaid balance across all invoices
- **Total Paid** — total amount paid to date
- **Vendors Involved** — count of distinct vendors with AP activity

##  How to Run This Project

1. Clone the repository
2. Add a `.env` file to the project root with your MySQL credentials:
   ```
   MYSQL_USER=your_user
   MYSQL_PASSWORD=your_password
   MYSQL_HOST=localhost
   MYSQL_PORT=3306
   MYSQL_DATABASE=your_database_name
   ```
3. Place the original CSV files in `Data/Original/`
4. Run `data_cleaning.ipynb` to generate the cleaned pickle and CSV files
5. Run `exploratory_data_analysis.ipynb` to build the MySQL database, load tables, create views, and generate charts

> **Note:** Pickle files are excluded from the repository. You must run the cleaning notebook before the EDA notebook.

## Final Recommendations

*To be updated after analysis is complete.*
