# 1. Calculated Columns vs Measures

**Dataset:** `datasets/01_Sales.csv`

## Concept
- A **calculated column** is computed row-by-row, at data-load/refresh time, and stored physically in the table. It has **row context** by default.
- A **measure** is computed on the fly, at query time, based on the current **filter context** (whatever slicers/rows/columns are in play). It is never stored.
- Rule of thumb: if the result should change depending on how the report is sliced, use a measure. If you need to filter, sort, or use it in a relationship, use a column.

## Dataset

| OrderID | Product | Quantity | UnitPrice |
|---|---|---|---|
| 1 | Laptop | 2 | 800 |
| 2 | Mouse | 5 | 20 |
| 3 | Laptop | 1 | 800 |
| 4 | Keyboard | 3 | 50 |
| 5 | Mouse | 10 | 20 |

## Example

Calculated column (row context — computes per row):
```dax
LineTotal = Sales[Quantity] * Sales[UnitPrice]
```

Measure (filter context — aggregates over whatever is visible):
```dax
Total Sales = SUMX(Sales, Sales[Quantity] * Sales[UnitPrice])
```

## Practice Questions
1. Why can't `LineTotal` (as written above) react to a slicer on `Product` the way a measure would?
2. Write a calculated column `IsBigOrder` that returns TRUE if `Quantity * UnitPrice > 500`.
3. Write a measure `Total Quantity` that sums `Quantity` across the current filter context.

<details><summary>Answers</summary>

1. Because it's calculated once at refresh and stored as a static value per row — it doesn't re-evaluate when filters change. A measure recalculates every time the filter context changes.
2. `IsBigOrder = Sales[Quantity] * Sales[UnitPrice] > 500`
3. `Total Quantity = SUM(Sales[Quantity])`
</details>
