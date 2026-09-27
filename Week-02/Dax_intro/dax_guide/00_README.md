# DAX Practice Guide — File Index

10 topics, each in its own file, with a matching CSV dataset in `datasets/`.

| # | Topic | File | Dataset(s) |
|---|---|---|---|
| 1 | Calculated Columns vs Measures | 01_Calculated_Columns_vs_Measures.md | datasets/01_Sales.csv |
| 2 | Row Context vs Filter Context | 02_Row_Context_vs_Filter_Context.md | datasets/02_Employees.csv |
| 3 | CALCULATE Function | 03_CALCULATE_Function.md | datasets/03_Sales2.csv |
| 4 | Time Intelligence | 04_Time_Intelligence.md | datasets/04_Sales3.csv |
| 5 | Iterators (SUMX, AVERAGEX...) | 05_Iterators.md | datasets/05_Orders.csv |
| 6 | Filter Functions (FILTER, ALL, ALLEXCEPT) | 06_Filter_Functions.md | datasets/06_Products.csv |
| 7 | Relationships (RELATED, RELATEDTABLE) | 07_Relationships.md | datasets/07_Customers.csv, datasets/07_Orders2.csv |
| 8 | Variables (VAR/RETURN) | 08_Variables.md | datasets/08_Sales4.csv |
| 9 | Ranking & Top-N (RANKX, TOPN) | 09_Ranking_TopN.md | datasets/09_ProductSales.csv |
| 10 | Practical Patterns (SWITCH, DIVIDE) | 10_Practical_Patterns.md | datasets/10_Students.csv |

## How to use
1. Import each CSV into Power BI (Get Data > Text/CSV) or Excel Power Pivot.
2. Read the topic's `.md` file for the concept and worked example.
3. Write the DAX yourself before checking the answers hidden under "Answers."

Suggested order: 1 → 2 → 3 → 5 → 6 → 7 → 4 → 9 → 10, weaving in 8 (VAR/RETURN) throughout.
