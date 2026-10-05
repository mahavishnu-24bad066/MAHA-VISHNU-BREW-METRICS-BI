
# GitHub Copilot DAX Development Notes

## Overview

GitHub Copilot was used as an AI-assisted development tool during the creation of the DAX measures for the BrewMetrics BI project.

Copilot suggestions were treated as starting points. Each measure was reviewed and tested in Power BI before being included in the final report.

---

## 1. Total Sales

### Requirement

Create a measure to calculate total sales.

### Copilot Suggestion

Copilot suggested using the `SUM()` function on the `sales_amount` column.

```DAX
Total Sales =
SUM(Fact_Sales[sales_amount])
```

### My Review

The suggested formula was appropriate because `sales_amount` contains the transaction-level sales values.

### Final Decision

The suggested formula was used without major changes.

---

## 2. Month-over-Month Growth

### Requirement

Create a growth measure comparing the current month with the previous month.

### Copilot Suggestion

Copilot suggested using `CALCULATE()` together with `DATEADD()` to retrieve the previous month's sales.

```DAX
Previous Month Sales =
CALCULATE(
    [Total Sales],
    DATEADD(Dim_Date[date], -1, MONTH)
)
```

It then suggested calculating the percentage difference using `DIVIDE()`.

### My Review and Correction

I separated the previous-month calculation into a separate measure instead of placing the entire calculation into one formula. This made the DAX easier to understand and reuse.

The final growth measure was:

```DAX
MoM Growth % =
DIVIDE(
    [Total Sales] - [Previous Month Sales],
    [Previous Month Sales]
)
```

Using `DIVIDE()` also provides safer division behavior when the denominator is zero or blank.

---

## 3. Running Total Sales

### Requirement

Create a running total of sales over time.

### Copilot Suggestion

Copilot suggested using `CALCULATE()` with a date filter and `ALL()` to calculate cumulative sales.

### My Review and Correction

I changed the filtering approach to use `ALLSELECTED()` so that the running total responds more appropriately to the filters and selections made in the report.

### Final Measure

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

This was an example where the initial AI suggestion needed to be reviewed and adjusted rather than copied directly.

---

## 4. City Sales Rank

### Requirement

Create a ranking measure using `RANKX`.

### Copilot Suggestion

Copilot suggested applying `RANKX()` to the city dimension and ranking cities according to total sales.

### My Review and Correction

I verified that the city filter context needed to be removed when calculating the ranking so that every city would be compared against the complete set of cities.

### Final Measure

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

The `ALL()` function allows the cities to be ranked against each other rather than only within the current city context.

---

## 5. Average Order Value

### Requirement

Create an additional business measure.

### Copilot Suggestion

Copilot suggested calculating average order value by dividing total sales by the number of transactions.

### My Review

I used `DISTINCTCOUNT()` on `sale_id` because each transaction should be counted once.

### Final Measure

```DAX
Average Order Value =
DIVIDE(
    [Total Sales],
    DISTINCTCOUNT(Fact_Sales[sale_id])
)
```

The measure was then used as a KPI in the dashboard.

---

## Summary of Copilot Usage

Copilot was useful for:

- Generating initial DAX structures
- Suggesting appropriate DAX functions
- Explaining functions such as `CALCULATE`, `DATEADD`, `FILTER`, `ALLSELECTED`, and `RANKX`
- Providing alternative approaches to calculations

However, the suggestions were not accepted blindly. The generated formulas were reviewed against the Power BI data model and tested in the report. The running-total and ranking calculations required particular attention to filter context.

The final DAX measures were selected and adjusted based on the actual BrewMetrics data model and dashboard requirements.
