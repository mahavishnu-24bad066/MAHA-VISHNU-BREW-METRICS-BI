
# BrewMetrics BI

## Business Intelligence Solution for BrewMetrics Coffee Co.

BrewMetrics BI is a Power BI-based Business Intelligence solution developed to analyze sales performance for BrewMetrics Coffee Co. The project uses a version-controlled development workflow with Power BI, GitHub, and GitHub Copilot.

The main objective is to create an interactive dashboard that helps management understand sales trends, city-level performance, store-format performance, and the seasonal pattern of Cold Brew sales.

---

## Project Objectives

- Analyze transaction-level coffee shop sales data.
- Build a proper star-schema data model.
- Create DAX measures for business analysis.
- Analyze the seasonal Cold Brew sales pattern.
- Compare sales performance across cities.
- Compare different store formats.
- Provide interactive filtering and drill-down analysis.
- Maintain the complete development history using Git and GitHub.
- Document the use of GitHub Copilot during DAX development.

---

## Dataset

The project uses the provided `brewmetrics_sales.csv` dataset.

The dataset contains transaction-level sales information across four cities and three store formats, covering the April–June period.

### Main Fields

| Field            | Description                  |
| ---------------- | ---------------------------- |
| `sale_id`      | Unique identifier for a sale |
| `date`         | Date of the transaction      |
| `city`         | City where the sale occurred |
| `store_format` | Store format                 |
| `category`     | Product category             |
| `item`         | Individual product           |
| `quantity`     | Quantity sold                |
| `unit_price`   | Price per unit               |
| `sales_amount` | Total sales amount           |

The dataset contains approximately 15,500 transactions and includes patterns such as increased Cold Brew sales during April–May and stronger performance from Bengaluru.

---

## Data Model

A star schema was created in Power BI by separating the transaction data into a central fact table and dimension tables.

### Fact Table

#### Fact_Sales

Contains transaction-level information:

- `sale_id`
- `date`
- `city`
- `store_format`
- `category`
- `item`
- `quantity`
- `unit_price`
- `sales_amount`

### Dimension Tables

#### Dim_Date

Contains date-related information:

- `date`
- `Year`
- `Month`
- `Month Number`
- `Quarter`
- `Day`

#### Dim_City

Contains the available cities.

#### Dim_Product

Contains:

- `item`
- `category`

#### Dim_Store

Contains:

- `store_format`

### Model Structure

```text
                 Dim_Date
                    |
                    |
Dim_City ---- Fact_Sales ---- Dim_Product
                    |
                    |
               Dim_Store
```

The dimension tables provide filtering and grouping information for the central `Fact_Sales` table.

---

## DAX Measures

### 1. Total Sales

```DAX
Total Sales =
SUM(Fact_Sales[sales_amount])
```

Calculates the total sales amount.

### 2. Previous Month Sales

```DAX
Previous Month Sales =
CALCULATE(
    [Total Sales],
    DATEADD(Dim_Date[date], -1, MONTH)
)
```

Returns the sales value for the previous month.

### 3. Month-over-Month Growth

```DAX
MoM Growth % =
DIVIDE(
    [Total Sales] - [Previous Month Sales],
    [Previous Month Sales]
)
```

Calculates the percentage change in sales compared with the previous month.

### 4. Running Total Sales

```DAX
Running Total Sales =
CALCULATE(
    [Total Sales],
    FILTER(
        ALLSELECTED(Dim_Date[date]),
        Dim_Date[date] <= MAX(Dim_Date[date])
    )
)
```

Calculates cumulative sales over time while respecting the report selection context.

### 5. City Sales Rank

```DAX
City Sales Rank =
RANKX(
    ALL(Dim_City[city]),
    [Total Sales],
    ,
    DESC,
    DENSE
)
```

Ranks cities according to their total sales.

### 6. Average Order Value

```DAX
Average Order Value =
DIVIDE(
    [Total Sales],
    DISTINCTCOUNT(Fact_Sales[sale_id])
)
```

Calculates the average sales value per transaction.

### 7. Total Transactions

```DAX
Total Transactions =
DISTINCTCOUNT(Fact_Sales[sale_id])
```

Calculates the number of unique transactions.

---

## Dashboard

The Power BI dashboard contains interactive visuals designed to help management explore sales performance.

### Dashboard Components

- Total Sales KPI
- Month-over-Month Growth KPI
- Average Order Value KPI
- Cold Brew Sales Trend
- Sales by City
- Sales by Store Format
- City slicer
- City → Store Format drill-down

### Cold Brew Analysis

The Cold Brew trend visual allows the user to examine sales over time and identify the seasonal pattern built into the dataset.

### City Analysis

The Sales by City visual compares sales performance across the four cities and helps identify the strongest-performing location.

### Store Format Analysis

The Sales by Store Format visual compares Flagship, Kiosk, and Drive-Thru performance.

### Drill-Down

The city visual supports:

```text
City
  ↓
Store Format
```

This allows users to move from overall city performance to individual store-format performance.

---

## Key Business Insights

1. **Cold Brew shows a seasonal pattern**, with increased sales during the April–May period as indicated by the dataset design.
2. **Bengaluru is the strongest-performing city**, consistently outperforming the other cities in the provided dataset.
3. **Store-format performance can be compared interactively**, allowing management to investigate whether Flagship, Kiosk, or Drive-Thru locations contribute more to sales.

---

## Technologies Used

- Power BI Desktop
- Power Query
- DAX
- Git
- GitHub
- GitHub Copilot
- Power BI Project (`.pbip`)

---

## Version Control Workflow

The project was developed incrementally rather than being uploaded as a completed Power BI report.

The development progression is:

```text
Initial Project Setup
        ↓
Star Schema
        ↓
Total Sales Measure
        ↓
MoM Growth Measure
        ↓
Running Total Measure
        ↓
City Sales Rank Measure
        ↓
Average Order Value Measure
        ↓
Dashboard
        ↓
Documentation
```

Each major development step was committed separately to GitHub so that changes to the data model, measures, and report could be traced through the commit history.
