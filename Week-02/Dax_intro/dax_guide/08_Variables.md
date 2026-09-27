# 8. Variables: VAR / RETURN

**Dataset:** `datasets/08_Sales4.csv`

## Concept
`VAR` lets you name intermediate calculations inside a measure, improving readability and performance (a variable is evaluated once, then reused, instead of DAX re-evaluating the same expression multiple times). `RETURN` provides the final result.

## Dataset

| Month | Revenue | Cost |
|---|---|---|
| Jan | 1000 | 700 |
| Feb | 1200 | 800 |
| Mar | 900 | 650 |

## Example

```dax
Profit Margin % =
VAR TotalRevenue = SUM(Sales4[Revenue])
VAR TotalCost = SUM(Sales4[Cost])
VAR Profit = TotalRevenue - TotalCost
RETURN
    DIVIDE(Profit, TotalRevenue)
```

## Practice Questions
1. Rewrite `Profit Margin %` without variables — what problem does this illustrate about repeating `SUM(Sales4[Revenue])`?
2. Write a measure using VAR that returns "Above Average" or "Below Average" per month compared to the overall average revenue.
3. Can a VAR's value change based on filter context after it's defined? Why or why not?

<details><summary>Answers</summary>

1. `Profit Margin % = DIVIDE(SUM(Sales4[Revenue]) - SUM(Sales4[Cost]), SUM(Sales4[Revenue]))` — this recalculates `SUM(Sales4[Revenue])` twice; with VAR it's calculated once and reused, which is both cleaner and can be faster.
2. ```dax
   Revenue Status =
   VAR CurrentRevenue = SUM(Sales4[Revenue])
   VAR AvgRevenue = CALCULATE(AVERAGE(Sales4[Revenue]), ALL(Sales4))
   RETURN
       IF(CurrentRevenue > AvgRevenue, "Above Average", "Below Average")
   ```
3. No — a VAR is evaluated once, at the point it's defined, using the filter context active at that moment. It's effectively a fixed value/table for the rest of the measure, which is exactly why it's useful for avoiding context-transition surprises.
</details>
