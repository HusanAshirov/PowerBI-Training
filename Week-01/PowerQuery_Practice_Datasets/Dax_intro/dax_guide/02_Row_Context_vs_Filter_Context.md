# 2. Row Context vs Filter Context

**Dataset:** `datasets/02_Employees.csv`

## Concept
- **Row context**: DAX "knows" which row it's currently on (exists automatically in calculated columns, and inside iterators like `SUMX`).
- **Filter context**: the set of filters currently applied (from slicers, visual rows/columns, or explicit `CALCULATE` filters). Measures always evaluate inside a filter context.
- The trickiest part of DAX is learning when you're in one, the other, or both — and how `CALCULATE` converts row context into filter context (**context transition**).

## Dataset

| EmpID | Name | Department | Salary |
|---|---|---|---|
| 1 | Anna | Sales | 4000 |
| 2 | Bob | Sales | 4500 |
| 3 | Cara | IT | 5000 |
| 4 | Dan | IT | 5500 |
| 5 | Eve | HR | 3800 |

## Example

```dax
-- This measure has NO row context on its own — it needs filter context
Avg Salary = AVERAGE(Employees[Salary])

-- Inside a table visual grouped by Department, filter context = "current department"
-- so Avg Salary automatically returns each department's average.
```

Context transition example:
```dax
SalaryVsDeptAvg =
VAR CurrentDeptAvg =
    CALCULATE(AVERAGE(Employees[Salary]), ALLEXCEPT(Employees, Employees[Department]))
RETURN
    Employees[Salary] - CurrentDeptAvg
```

## Practice Questions
1. If you drop `Avg Salary` into a table visualized by `Department`, what filter context does each row have?
2. What does `ALLEXCEPT(Employees, Employees[Department])` do here?
3. True or false: a calculated column can use `CALCULATE` to turn its row context into filter context.

<details><summary>Answers</summary>

1. Each row's filter context is "only rows where Department = that row's value."
2. It removes all filters on the `Employees` table except the one on `Department` — so you get the department-level average regardless of other filters (like a slicer on Name).
3. True — this is exactly what "context transition" means: `CALCULATE` inside a row context (like a calculated column) turns the current row into an equivalent filter.
</details>
