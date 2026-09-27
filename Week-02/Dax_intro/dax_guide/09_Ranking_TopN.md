# 9. Ranking & Top-N: RANKX, TOPN

**Dataset:** `datasets/09_ProductSales.csv`

## Concept
- `RANKX(table, expression, [value], [order], [ties])` ranks each row/item by an expression evaluated over a table.
- `TOPN(n, table, orderby_expression)` returns the top N rows as a table, usable inside `CALCULATE`/`SUMX`.

## Dataset

| Product | TotalSales |
|---|---|
| A | 500 |
| B | 900 |
| C | 300 |
| D | 900 |
| E | 150 |

## Example

```dax
Sales Rank =
RANKX(ALL(ProductSales), ProductSales[TotalSales], , DESC, Dense)

Top 3 Sales =
CALCULATE(
    SUM(ProductSales[TotalSales]),
    TOPN(3, ProductSales, ProductSales[TotalSales], DESC)
)
```

## Practice Questions
1. Why is `ALL(ProductSales)` important inside `RANKX` here?
2. Products B and D are tied at 900. With `Dense` ranking, what ranks do A, B, C, D, E get?
3. Write a measure that returns TRUE if a product is in the top 2 by sales.

<details><summary>Answers</summary>

1. Without `ALL`, `RANKX` would only rank within the current filter context (e.g., a single row if filtered to one product), producing rank 1 for everything. `ALL` ensures ranking happens across the full, unfiltered product list.
2. With Dense ranking: B=1, D=1, A=2, C=3, E=4 (sorted descending: B 900, D 900, A 500, C 300, E 150 — ties share a rank, and the next rank isn't skipped).
3. ```dax
   Is Top 2 =
   VAR Rnk = RANKX(ALL(ProductSales), ProductSales[TotalSales], , DESC, Dense)
   RETURN Rnk <= 2
   ```
</details>
