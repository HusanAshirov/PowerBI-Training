# Power Query Practice Pack — 20 Tasks (Basic → Advanced)

## Files (put them all in one folder, e.g. `PowerQuery_Practice_Datasets\`)

| File | Purpose | What's messy about it |
|---|---|---|
| `customers_dirty.csv` | Customer master data | Extra spaces in headers/values, mixed casing, inconsistent Status values, mixed date formats, mixed phone formats, missing values, exact duplicate rows |
| `sales_jan.csv` | January orders | Relatively clean — your baseline |
| `sales_feb.csv` | February orders | **Different column names** than Jan/Mar (`Order_Id` vs `OrderID`, etc.) |
| `sales_mar.csv` | March orders | Blank row, missing `UnitPrice` values, negative `Quantity` (returns), extra `Notes` column not present in other files |
| `products_wide.csv` | Product catalog with monthly sales | Wide/pivoted format — one column per month, needs unpivoting |
| `returns.csv` | Return records | Some `OrderID`s don't exist in the sales data (data quality issue) |

---

## 🟢 Basic (1–6)

**1. Import and inspect**
Load `customers_dirty.csv` into Power Query. Promote headers. Notice the column headers themselves have leading/trailing spaces — trim the *header names*, not just the data (Transform → Rename, or edit the header row text directly).

**2. Trim and clean all text columns at once**
Instead of trimming column by column, write one step that dynamically finds every text column in the customers table and applies `Text.Trim` to all of them (hint: `Table.TransformColumns` + a dynamically built list of column names).

**3. Fix inconsistent data types**
Set correct types: `CustomerID` as whole number, `SignupDate` as date, everything else sensibly as text. Don't rely on Power Query's auto-detect — set each type deliberately and check what breaks.

**4. Standardize the Status column**
`Status` appears as `Active`, `active`, `ACTIVE`, `Inactive`, `inactive`. Normalize all of them to just two consistent values: `Active` / `Inactive`.

**5. Remove exact duplicate rows**
Two customer rows are byte-for-byte duplicates. Remove them using `Remove Duplicates` (or `Table.Distinct`) — but check first whether you should dedupe on the whole row or just on `CustomerID`.

**6. Handle missing values**
`Email` and `Phone` have some blanks. Decide on a consistent approach — either fill with a placeholder like `"Unknown"`, or leave as `null` — and apply it consistently across both columns.

---

## 🟡 Intermediate (7–13)

**7. Standardize the messy dates**
`SignupDate` appears in at least 4 different formats (`01/15/2024`, `2024-02-20`, `20/03/2024`, `2024/06/18`, `19-07-2024`...). Build one custom column that parses all of them into a single `date` type. (You've already built similar logic for the RWA and invoice-date exercises — reuse and adapt it.)

**8. Split Full Name into First/Last Name**
Split the `Full Name` column into `First Name` and `Last Name`, then apply `Text.Proper` so casing is consistent (fixes both `john smith` and `MARY JONES`).

**9. Combine the 3 monthly sales files from a folder**
Use `Get Data → Folder` on the folder containing `sales_jan.csv`, `sales_feb.csv`, and `sales_mar.csv`. Since Feb uses different column names (`Order_Id`, `Customer_Id`, `Qty`, `Unit_Price`, `Order_Date`) than Jan/Mar, your combine logic needs to **rename columns to a consistent schema inside the per-file transform step** before appending — a straight `Combine & Transform` will misalign columns if you don't handle this.

**10. Clean up the appended sales table**
After combining, deal with: the fully blank row (from March), missing `UnitPrice` values (decide: fill with 0, or look up the product's known price from `products_wide.csv`), and the extra `Notes` column that only exists in March's file (should now be `null` for Jan/Feb rows — is that OK, or should it be filled?).

**11. Add a calculated TotalAmount column**
Add `TotalAmount = Quantity × UnitPrice`. Watch what happens with the negative-quantity return rows — decide whether returns should show as negative revenue (probably yes) and confirm your formula handles it correctly.

**12. Merge sales with customers**
Left-join the combined sales table to `customers_dirty` on `CustomerID`, bringing in `Full Name` and `Status`. Notice: some `CustomerID`s in Feb's file (e.g. `1011`) refer to customers with blank/missing names — trace where that data quality issue originated.

**13. Unpivot the wide product table**
Load `products_wide.csv` and unpivot the `Jan_Sales`, `Feb_Sales`, `Mar_Sales`, `Apr_Sales` columns into two columns: `Month` and `MonthlySales`. Then clean the `Month` values (e.g. turn `"Jan_Sales"` into just `"Jan"`).

---

## 🔴 Advanced (14–20)

**14. Merge returns with sales and flag data quality issues**
Merge `returns.csv` with your combined sales table on `OrderID` (left join from returns → sales). Some returns (`5099`, `5150`) don't match any real order — add a flag column `HasMatchingOrder` (true/false) so these orphaned return records are easy to isolate and report as data errors.

**15. Group, summarize, then re-pivot into a report**
From your merged sales+customers table, group by `CustomerID` and `Month` (derived from `OrderDate`), summing `TotalAmount`. Then pivot `Month` back into columns, so you end up with one row per customer and one column per month — a classic "long → wide" reporting shape.

**16. Flag each customer's first order**
Add a calculated column `IsFirstOrder` that's `1` for each customer's earliest order (by `OrderDate`) and `0` for the rest — same pattern as the "first row per group" logic you built earlier, but now driven by date order instead of row position.

**17. Build a custom function for phone number standardization**
`Phone` values appear as `555-0101`, `(555) 0102`, `555.0103`, `555 0106`. Write a **custom M function** (`(phone as text) as text => ...`) that strips all non-digit characters and reformats consistently as `555-0101`, then invoke it as a new custom column across the whole customers table.

**18. Add a Query Parameter to drive a dynamic filter**
Create a Parameter called `MinOrderAmount` (Decimal, default `20`). Build a query that filters your merged sales table to only orders where `TotalAmount >= MinOrderAmount`. Change the parameter value and confirm the query result updates.

**19. Segment customers with nested conditional logic**
Using each customer's total spend (from your grouped sales data), add a `CustomerSegment` column: `"High"` if total spend ≥ 200, `"Medium"` if ≥ 75, otherwise `"Low"`. Do this with a single `Table.AddColumn` using nested `if...then...else if...else`, not multiple separate steps.

**20. Build an error-catching reconciliation query**
Wrap your riskiest type-conversion steps (e.g. the date parsing in Task 7, the price fill logic in Task 10) in `try ... otherwise` so a bad row doesn't break the whole query. Then build a **separate query** that filters for rows where an error occurred (`try` returns a record with `[HasError]`), producing a clean "error log" table listing exactly which rows failed and why — something you'd hand to a data owner to fix at the source.

---

## Suggested order of attack

Do 1–8 first as isolated warm-ups directly on `customers_dirty.csv`. Then 9–13 shift to combining and reshaping the sales/product data. Save 14–20 for once you're comfortable — they all build on the combined table from Task 9–12, so getting that foundation right matters more than speed.
