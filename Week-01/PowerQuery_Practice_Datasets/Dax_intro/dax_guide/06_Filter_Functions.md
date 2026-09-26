# 6. Filter Functions: FILTER, ALL, ALLEXCEPT, ALLSELECTED

**Dataset:** `datasets/06_Products.csv`

## Concept
- `FILTER(table, condition)` returns a filtered table — used inside `CALCULATE` or iterators.
- `ALL(table/column)` removes filters — useful for "% of total" or ignoring slicers.
- `ALLEXCEPT(table, col1, col2...)` removes all filters except the ones listed.
- `ALLSELECTED(...)` removes filters added inside the visual, but keeps external ones (like page-level slicers) — used for "% of what's currently shown."

## Dataset

| ProductID | Category | Price | Stock |
|---|---|---|---|
| 1 | Electronics | 300 | 10 |
| 2 | Electronics | 150 | 25 |
| 3 | Furniture | 500 | 5 |
| 4 | Furniture | 200 | 15 |
| 5 | Toys | 40 | 100 |

## Example

```dax
Total Stock Value = SUMX(Products, Products[Price] * Products[Stock])

Expensive Products Count =
CALCULATE(COUNTROWS(Products), FILTER(Products, Products[Price] > 200))

Stock Value All Categories =
CALCULATE([Total Stock Value], ALL(Products[Category]))

Stock Value Except Category =
CALCULATE([Total Stock Value], ALLEXCEPT(Products, Products[Category]))
```

## Practice Questions
1. What's the difference between `ALL(Products)` and `ALL(Products[Category])`?
2. Write a measure that counts products priced above the overall average price (average computed ignoring any filters).
3. When would you choose `ALLSELECTED` over `ALL`?

<details><summary>Answers</summary>

1. `ALL(Products)` removes every filter on the whole table (all columns); `ALL(Products[Category])` removes filters only on the Category column, leaving filters on Price, Stock, etc. intact.
2. `Above Avg Count = CALCULATE(COUNTROWS(Products), FILTER(Products, Products[Price] > CALCULATE(AVERAGE(Products[Price]), ALL(Products))))`
3. When you want "% of total" to respect page-level or report-level slicers the user applied, but ignore only the local breakdown inside the visual (e.g., a bar chart where each bar is a category — you want each bar's % of the *currently sliced* total, not the grand unfiltered total).
</details>
