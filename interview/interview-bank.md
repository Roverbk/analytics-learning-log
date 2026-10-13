# Interview question bank
Format: Q / my answer (own words, 2–3 lines) / status ✅ or 🔶 / date added

## SQL: window functions

### Top-N per group with ties: ROW_NUMBER vs RANK vs DENSE_RANK? (Oct 13, OPEN)
Example: M1 and M2 tie at 500 for 1st, then M3 400, M4 300, M5 200. How many rows
does each return for "<= 3"? Which do you use for "exactly 3 per month"?
My answer:
Status:

### Why add a tie-breaker to ORDER BY in a window? (Oct 13)
Key points: tied rows can come in either order -> result not reproducible;
add a unique column (timestamp, else txn_id) as the LAST sort key.
My answer:
Status:

### "Both orders are valid, so what's the problem?" (Oct 13)
Key points: same data can give different results across runs (audits, reports,
tests break); it only matters when output shows which row got which value;
the tie-breaker makes the business choice explicit.
My answer:
Status:

### Running total: why do two same-date rows show the same value? (Oct 13)
Key points: default frame is RANGE -> rows tied on ORDER BY are lumped.
Per-transaction (which txn crossed the limit?) -> tie-breaker.
End-of-day balance -> the lumping is actually fine.
My answer:
Status:

### LAG gives NULL on the first row: keep it or replace it with 0? (Oct 13)
Key points: depends on the metric. AVG ignores NULL, so for "average days
between txns" keep NULL; COALESCE(..., 0) would pull the average down.
My answer:
Status:

### Self-join vs window function: which and why? (Oct 13)
Key points: #180: self-join on id+1/id+2 breaks with id gaps; LAG follows row order.
#1204: join to all earlier rows grows as n^2; SUM() OVER is one pass.
My answer:
Status:

## SQL: general

### You don't know the interviewer's SQL dialect. What do you write? (Oct 13)
Key points: portable choices: CTE + WHERE (not QUALIFY), a / NULLIF(b, 0)
(not SAFE_DIVIDE), COALESCE (not IFNULL), CASE WHEN (not IF), repeat the
expression in GROUP BY (not the alias).
My answer:
Status:

### What happens if MySQL's ONLY_FULL_GROUP_BY is off? (Oct 13)
Key points: non-grouped columns get an arbitrary value from the group,
silently mixing rows. Never rely on it.
My answer:
Status:

## Excel

### VLOOKUP vs INDEX/MATCH vs XLOOKUP? (Oct 13)
Key points: VLOOKUP can't look left, defaults to approximate, breaks when
columns are inserted. INDEX/MATCH fixes all three and works in old Excel.
XLOOKUP is cleanest, needs 2021/365. Many banks run older Excel.
My answer:
Status:

### A lookup key appears twice in the lookup table. What happens in Excel vs SQL? (Oct 13)
Key points: XLOOKUP/INDEX-MATCH return the first match silently; a SQL JOIN
returns one row per match, duplicating rows. Check key uniqueness in both.
My answer:
Status:
