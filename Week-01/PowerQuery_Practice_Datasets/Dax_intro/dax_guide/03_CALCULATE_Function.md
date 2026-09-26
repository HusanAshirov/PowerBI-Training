# 3. CALCULATE — the most important function in DAX

**Dataset:** `datasets/03_Sales2.csv`

## Concept
`CALCULATE(<expression>, <filter1>, <filter2>, ...)` evaluates an expression in a **modified filter context**. Filters you pass in either **add** a new filter, or **replace** an existing filter on the same column. This is the engine behind almost every advanced DAX pattern (time intelligence, %-of-total, comparisons, etc.).

## Dataset

| Date | Region | Product | Amount |
|---|---|---|---|
| 2024-01-05 | East | A | 100 |
| 2024-01-06 | West | B | 150 |
| 2024-02-10 | East | A | 200 |
| 2024-02-15 | West | A | 120 |
| 2024-03-01 | East | B | 90 |

## Example

```dax
Total Amount = SUM(Sales2[Amount])

East Sales =
CALCULATE([Total Amount], Sales2[Region] = "East")

Sales Excluding Region Filter =
CALCULATE([Total Amount], ALL(Sales2[Region]))
```

## Practice Questions
1. Write a measure `Product A Sales` that always returns total sales for Product A, regardless of any Product slicer.
2. What's the difference between `CALCULATE([Total Amount], Sales2[Region]="East")` and just filtering the visual by East?
3. Write `% of Total Sales` = current filtered amount ÷ total amount ignoring all filters.

<details><summary>Answers</summary>

1. `Product A Sales = CALCULATE([Total Amount], Sales2[Product] = "A")` — this REPLACES any existing filter on Product with "A".
2. Functionally similar in result, but `CALCULATE` is explicit and reusable inside a measure regardless of what's on the report; a visual-level filter is external and only applies to that visual.
3. `% of Total Sales = DIVIDE([Total Amount], CALCULATE([Total Amount], ALL(Sales2)))`
</details>
