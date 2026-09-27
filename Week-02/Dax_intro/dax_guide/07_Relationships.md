# 7. Relationships: RELATED and RELATEDTABLE

**Datasets:** `datasets/07_Customers.csv` (one side), `datasets/07_Orders2.csv` (many side)

## Concept
- `RELATED(column)` pulls a value from the "one" side of a relationship into the "many" side (used in row context, e.g., calculated columns).
- `RELATEDTABLE(table)` returns related rows from the "many" side when you're on the "one" side — commonly wrapped in `COUNTROWS` or an iterator.
- Relationships must exist in the model (typically 1-to-many) for these to work; DAX does not auto-join unrelated tables.

## Datasets

`Customers`

| CustomerID | Name | City |
|---|---|---|
| 1 | Acme Co | Tashkent |
| 2 | Globex | Samarkand |

`Orders2`

| OrderID | CustomerID | Amount |
|---|---|---|
| 101 | 1 | 300 |
| 102 | 1 | 150 |
| 103 | 2 | 500 |

## Example

```dax
-- Calculated column on Orders2 (many side)
Customer City = RELATED(Customers[City])

-- Measure evaluated in row context of Customers (one side)
Order Count = COUNTROWS(RELATEDTABLE(Orders2))
```

## Practice Questions
1. Why does `RELATED` work on the Orders2 side but not the Customers side?
2. Write a calculated column on `Customers` (using `RELATEDTABLE`) that sums each customer's total order amount.
3. What happens if you call `RELATED(Customers[City])` on a row in Orders2 whose CustomerID has no match in Customers?

<details><summary>Answers</summary>

1. Because `RELATED` looks "up" from the many side to the one side, where each row has exactly one related parent. From the one side, there could be many related child rows, so a single value isn't well-defined — that's what `RELATEDTABLE` is for instead.
2. `Total Orders = SUMX(RELATEDTABLE(Orders2), Orders2[Amount])`
3. It returns `BLANK()` (or errors depending on relationship enforcement), since there's no matching row to pull from.
</details>
