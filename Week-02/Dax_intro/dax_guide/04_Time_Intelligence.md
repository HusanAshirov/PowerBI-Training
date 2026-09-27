# 4. Time Intelligence

**Dataset:** `datasets/04_Sales3.csv`

## Concept
Built-in functions (`TOTALYTD`, `SAMEPERIODLASTYEAR`, `DATEADD`, `DATESYTD`, `PREVIOUSMONTH`, etc.) that shift or accumulate filter context along a date axis. They require a proper **Date table**, marked as a date table, connected via relationship.

## Dataset

| Date | Amount |
|---|---|
| 2023-01-15 | 500 |
| 2023-06-20 | 700 |
| 2024-01-10 | 600 |
| 2024-06-18 | 900 |
| 2024-12-05 | 300 |

## Example

```dax
Total Sales = SUM(Sales3[Amount])

YTD Sales = TOTALYTD([Total Sales], Sales3[Date])

Sales LY = CALCULATE([Total Sales], SAMEPERIODLASTYEAR(Sales3[Date]))

Sales Growth % = DIVIDE([Total Sales] - [Sales LY], [Sales LY])
```

## Practice Questions
1. Why does time intelligence require a dedicated Date table instead of just using the dates in the Sales table?
2. Write a measure for "Sales in the previous month."
3. What would `TOTALYTD([Total Sales], Sales3[Date], "6/30")` change about the YTD calculation?

<details><summary>Answers</summary>

1. Because time intelligence functions need a continuous, unbroken calendar (every date, not just dates with transactions) to correctly compute periods, and best practice is to mark it as an official Date table so DAX's time functions work reliably.
2. `Sales PM = CALCULATE([Total Sales], PREVIOUSMONTH(Sales3[Date]))`
3. It shifts the fiscal year-end to June 30, so "YTD" would accumulate from July 1 instead of January 1.
</details>
