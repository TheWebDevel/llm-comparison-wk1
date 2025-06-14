### Scenario A – ANSI SQL: Top-3 Items by Revenue per Region in Q1 2025

#### ✅ Strengths

- Uses **CTEs** effectively:
  - `q1_2025_sales` correctly aggregates revenue using `SUM(qty * price)` grouped by `region` and `item_id`.
  - `ranked_items` applies a window function (`RANK()`) to compute rank per region.

- Applies **correct date filtering** for Q1 2025 using:
  ```sql
  s.sale_date >= '2025-01-01' AND s.sale_date < '2025-04-01'
