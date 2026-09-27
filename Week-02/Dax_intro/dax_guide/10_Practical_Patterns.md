# 10. Practical Patterns: SWITCH, DIVIDE, error handling, SWITCH(TRUE())

**Dataset:** `datasets/10_Students.csv`

## Concept
- `DIVIDE(numerator, denominator, [alternate])` safely handles division by zero (better than `/` with `IFERROR`).
- `SWITCH(expression, value1, result1, value2, result2, ..., default)` is a cleaner alternative to nested `IF`.
- `SWITCH(TRUE(), condition1, result1, condition2, result2, ..., default)` handles ranges/multiple conditions, not just exact matches.

## Dataset

| Name | Score |
|---|---|
| Ali | 95 |
| Bek | 72 |
| Cate | 58 |
| Dan | 40 |
| Ella | 88 |

## Example

```dax
Pass Rate =
VAR TotalStudents = COUNTROWS(Students)
VAR Passed = CALCULATE(COUNTROWS(Students), Students[Score] >= 60)
RETURN
    DIVIDE(Passed, TotalStudents)

Grade =
VAR S = SELECTEDVALUE(Students[Score])
RETURN
    SWITCH(
        TRUE(),
        S >= 90, "A",
        S >= 80, "B",
        S >= 70, "C",
        S >= 60, "D",
        "F"
    )
```

## Practice Questions
1. Why use `DIVIDE(Passed, TotalStudents)` instead of `Passed / TotalStudents`?
2. What does `SELECTEDVALUE` do, and why is it needed here instead of just `Students[Score]`?
3. Rewrite `Grade` using nested `IF` instead of `SWITCH(TRUE(), ...)` — which version is easier to read as more grade bands are added?

<details><summary>Answers</summary>

1. `DIVIDE` returns `BLANK()` (or a specified alternate value) instead of throwing a division-by-zero error when `TotalStudents` is 0 — safer for production reports.
2. `SELECTEDVALUE(column)` returns the column's value only if exactly one distinct value is in context (otherwise returns BLANK or a default) — needed because `Students[Score]` alone is ambiguous outside row context; a measure needs a single scalar.
3. ```dax
   Grade =
   VAR S = SELECTEDVALUE(Students[Score])
   RETURN
       IF(S >= 90, "A",
           IF(S >= 80, "B",
               IF(S >= 70, "C",
                   IF(S >= 60, "D", "F"))))
   ```
   `SWITCH(TRUE(), ...)` is easier to read and extend — nested `IF`s get harder to parse as bands increase, while `SWITCH` keeps each condition/result pair flat and scannable.
</details>
