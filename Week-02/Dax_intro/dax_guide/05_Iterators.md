# 5. Iterators (X-functions): SUMX, AVERAGEX, COUNTX, MAXX, RANKX...

**Dataset:** `datasets/05_Orders.csv`

## Concept
Iterators loop row-by-row over a table (creating row context for each row), evaluate an expression, then aggregate. Use them whenever the calculation isn't a simple column aggregation — e.g., multiplying two columns before summing.

## Dataset

| OrderID | Qty | Price | Discount |
|---|---|---|---|
| 1 | 3 | 50 | 0.1 |
| 2 | 1 | 200 | 0 |
| 3 | 5 | 20 | 0.2 |
| 4 | 2 | 75 | 0.05 |

## Example

```dax
Revenue = SUMX(Orders, Orders[Qty] * Orders[Price] * (1 - Orders[Discount]))

Avg Order Value = AVERAGEX(Orders, Orders[Qty] * Orders[Price])

Max Line Revenue = MAXX(Orders, Orders[Qty] * Orders[Price])
```

## Practice Questions
1. Why won't `SUM(Orders[Qty] * Orders[Price])` work directly (in most tools it errors)?
2. Write a measure counting how many orders have a line revenue (Qty × Price) greater than 100.
3. What's the difference in result between `AVERAGEX(Orders, Orders[Qty]*Orders[Price])` and `AVERAGE(Orders[Qty]) * AVERAGE(Orders[Price])`?

<details><summary>Answers</summary>

1. `SUM` only accepts a single column reference, not an expression across multiple columns — you need an iterator to evaluate the row-by-row expression first.
2. `Big Lines = COUNTX(FILTER(Orders, Orders[Qty]*Orders[Price] > 100), Orders[OrderID])`
3. They usually differ: `AVERAGEX` computes each row's product first then averages those products (correct); the second multiplies two separate averages together, which is mathematically different and usually wrong for this kind of calculation.
</details>
