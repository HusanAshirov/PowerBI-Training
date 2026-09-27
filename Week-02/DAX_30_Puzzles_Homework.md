# DAX Practice — 30 Puzzles (Basic → Advanced)

Assumed columns (adjust to your actual dataset if names differ):
- **01_Sales.csv**: OrderDate, ProductID, CustomerID, Region, Quantity, UnitPrice
- **02_Employees.csv**: EmployeeID, Department, HireDate, Salary, ManagerID
- **03_Sales2.csv / 04_Sales3.csv / 08_Sales4.csv**: similar Sales-style tables with slight schema variations (used for practicing merges/relationships)
- **05_Orders.csv / 07_Orders2.csv**: OrderID, OrderDate, CustomerID, Status, Amount
- **06_Products.csv**: ProductID, ProductName, Category, Cost, Price
- **07_Customers.csv**: CustomerID, CustomerName, Country, JoinDate
- **09_ProductSales.csv**: ProductID, Month, UnitsSold, Revenue
- **10_Students.csv**: StudentID, Subject, Score, ExamDate

---

## 1. Calculated Columns vs Measures (`01_Calculated_Columns_vs_Measures.md`)

**1.1 (Basic)** Create a calculated column `LineTotal` on the Sales table = Quantity × UnitPrice.

**1.2 (Intermediate)** Create a measure `Total Revenue` that sums the same value. Explain in a comment why the column approach and the measure approach give different results when placed in a PivotTable with Region on rows.

**1.3 (Advanced)** Create a calculated column `Running Employee Count` that shows, for each Employee row, how many employees were hired on or before that employee's HireDate. Then create a measure that does the same thing dynamically inside a visual. Compare performance implications.

---

## 2. Row Context vs Filter Context (`02_Row_Context_vs_Filter_Context.md`)

**2.1 (Basic)** Write a calculated column `PriceWithTax` = UnitPrice × 1.15. Identify what kind of context is active here.

**2.2 (Intermediate)** Write a measure `Avg Order Value` using `AVERAGEX` over the Orders table. Explain why `AVERAGEX` creates row context even though it's used inside a measure.

**2.3 (Advanced)** Explain (in a comment) and then write a measure `Sales Above Region Avg` that returns the total sales for only those rows where a product's sale is above the average sale for its Region — combining row context transformed into filter context.

---

## 3. CALCULATE Function (`03_CALCULATE_Function.md`)

**3.1 (Basic)** Write a measure `Sales in North Region` using `CALCULATE` and a hardcoded filter `Sales[Region] = "North"`.

**3.2 (Intermediate)** Write a measure `Sales Excluding Returns` using `CALCULATE` with `Orders[Status] <> "Returned"`.

**3.3 (Advanced)** Write a measure `YoY Sales Growth %` using `CALCULATE` combined with `SAMEPERIODLASTYEAR`, handling divide-by-zero safely with `DIVIDE`.

---

## 4. Time Intelligence (`04_Time_Intelligence.md`)

**4.1 (Basic)** Write a measure `Sales MTD` using `TOTALMTD`.

**4.2 (Intermediate)** Write a measure `Sales Last Quarter` using `PREVIOUSQUARTER` inside `CALCULATE`.

**4.3 (Advanced)** Write a measure `Rolling 3-Month Average Sales` using `DATESINPERIOD` combined with `AVERAGEX`, and make it correctly blank for months with no prior data.

---

## 5. Iterators (`05_Iterators.md`)

**5.1 (Basic)** Write a measure `Total Sales (Iterator)` using `SUMX` over Sales, multiplying Quantity × UnitPrice.

**5.2 (Intermediate)** Write a measure `Total Discounted Sales` using `SUMX`, applying a 10% discount only to rows where Quantity > 5 (use an `IF` inside `SUMX`).

**5.3 (Advanced)** Write a measure `Weighted Avg Employee Tenure` using `AVERAGEX`, where tenure is weighted by Salary (i.e., higher-paid employees count more toward the average).

---

## 6. Filter Functions (`06_Filter_Functions.md`)

**6.1 (Basic)** Write a measure `Product Count` using `COUNTROWS(FILTER(...))` to count products with Price > 100.

**6.2 (Intermediate)** Write a measure `Sales Ex High Value` using `ALL` to remove any existing filter on Region, then reapply only a Category filter.

**6.3 (Advanced)** Write a measure `% of Category Total` using `ALLEXCEPT` so it always shows a product's share of its Category's total sales, regardless of what other slicers (Region, Date, Customer) are applied.

---

## 7. Relationships (`07_Relationships.md`)

**7.1 (Basic)** Explain what happens to a measure summing Sales when Products and Sales are joined on ProductID with an inactive relationship, versus an active one.

**7.2 (Intermediate)** Write a measure `Sales via Inactive Relationship` using `USERELATIONSHIP` to force DAX to use a second, inactive date relationship (e.g., ShipDate instead of OrderDate).

**7.3 (Advanced)** You have Orders and Orders2 with overlapping OrderIDs but different schemas. Design (in words, then DAX) a way to build a unified measure `Combined Order Count` that correctly counts distinct orders across both tables without duplicating IDs that appear in both.

---

## 8. Variables (`08_Variables.md`)

**8.1 (Basic)** Rewrite this measure using a `VAR`:
```
Sales Margin % = DIVIDE(SUM(Sales[Revenue]) - SUM(Sales[Cost]), SUM(Sales[Revenue]))
```

**8.2 (Intermediate)** Write a measure `Sales vs Target Flag` that computes actual sales and a target value as two VARs, then returns "Above", "Below", or "On Target" using an `IF`/`SWITCH` on the difference.

**8.3 (Advanced)** Write a measure `Top Customer Contribution %` that uses VARs to calculate (a) the current customer's total sales and (b) the overall total sales, then returns the ratio — but make it resilient (correct output) whether it's used in a table visual per-customer or in a card showing overall context.

---

## 9. Ranking / TopN (`09_Ranking_TopN.md`)

**9.1 (Basic)** Write a measure `Product Rank by Sales` using `RANKX` over all products by total sales.

**9.2 (Intermediate)** Write a measure `Top 5 Products Sales` that returns total sales only for the top 5 products by revenue, using `TOPN`.

**9.3 (Advanced)** Write a measure `Rank Within Region` using `RANKX` with `ALLEXCEPT`, so each product is ranked only against other products in the same Region (not globally).

---

## 10. Practical Patterns (`10_Practical_Patterns.md`)

**10.1 (Basic)** Write a measure `New Customers This Month` that counts customers whose JoinDate falls in the current filter month.

**10.2 (Intermediate)** Write a measure `Customer Retention Rate %` = customers who purchased both this period and last period ÷ customers who purchased last period.

**10.3 (Advanced)** Build a full RFM-style measure set: `Recency (Days)`, `Frequency (Order Count)`, and `Monetary (Total Spend)` per customer, then combine them into a single measure `Customer Segment` using nested `SWITCH`/`IF` logic to label customers as "Champion", "At Risk", or "Lost".

---

## How to use this
1. Work top to bottom — each section builds on the previous one's concepts.
2. Write your DAX in Power BI, test the output against a pivot table before checking correctness.
3. For the "Advanced" puzzle in each section, first write out your logic in plain English comments before coding — this catches context-transition mistakes early.
