# Interview question bank

> **How to use:** read the answer, cover it, say it out loud in your own words. Mark ✅ (can say it) or 🔶 (shaky). Added date in brackets.

## SQL: window functions

### Top-N per group with ties: ROW_NUMBER vs RANK vs DENSE_RANK? [Oct 13]
**Example:** M1 and M2 tie at 500 for 1st, then M3 400, M4 300, M5 200. How many rows does each return for "<= 3"? Which do you use for "exactly 3 per month"?

**A:** ROW_NUMBER gives 1,2,3,4,5, so "<= 3" returns 3 rows, but which of M1/M2 is 1st is arbitrary unless I add a tie-breaker. RANK gives 1,1,3,4,5, so 3 rows (M1, M2, M3). DENSE_RANK gives 1,1,2,3,4, so 4 rows (M1, M2, M3, M4). For "exactly 3 per month" I use ROW_NUMBER with a tie-breaker. For "top 3 positions, keep ties" I use RANK. For "top 3 distinct values", like unique salaries, I use DENSE_RANK. One-liner: RANK counts people ahead of you; DENSE_RANK counts distinct values ahead of you.

**Status:**

### Why add a tie-breaker to ORDER BY in a window function? [Oct 13]
**A:** If two rows tie on the ORDER BY inside a window, the database can put them in either order, so ROW_NUMBER, LAG or a running total can give different results on different runs. I add a unique column as the last sort key, ideally the timestamp, otherwise the transaction ID, so the output is deterministic. For example, with two transactions on the same day, a "same-day repeat" fraud flag could land on either one without a tie-breaker.

**Status:**

### "Both orders are valid, so what's the problem?" [Oct 13]
**A:** Each order is valid, but my query doesn't choose, so the database chooses arbitrarily. The same data can then give different results: a report changes with no new data, a different transaction gets flagged, tests pass only sometimes. It only matters when the output shows which specific row got which value, but I add a tie-breaker every time because it costs nothing and makes the business decision explicit instead of leaving it to chance.

**Status:**

### Running total: why do two same-date rows show the same value? [Oct 13]
**A:** With ORDER BY and no explicit frame, the default is RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW. RANGE treats rows with the same ORDER BY value as one block, so two same-day transactions both show the end-of-day total. If I need per-transaction totals, like which transaction pushed a card over its limit, I add a tie-breaker. If I want the balance as of the end of each day, the lumping is exactly right.

**Status:**

### LAG gives NULL on the first row: keep it or replace it with 0? [Oct 13]
**A:** It depends on the metric. For "average days between transactions" I keep the NULL, because AVG ignores NULLs; replacing it with 0 would invent a zero-day gap and pull the average down. If the business needs a value for first transactions, I'd handle it explicitly, for example with a separate "first transaction" flag, rather than a fake number.

**Status:**

### Self-join vs window function: which and why? [Oct 13]
**A:** Both work, but the window version is usually better. For consecutive values, a self-join on id+1 and id+2 assumes ids have no gaps; if a row was deleted, it silently misses runs, while LAG follows the actual row order. For running totals, joining each row to all earlier rows creates about n²/2 pairs, so 10,000 rows is roughly 50 million pairs, while SUM() OVER sorts once and makes one pass. The window query is also shorter and easier to review.

**Status:**

### Find users with transactions on 3+ consecutive days. Walk me through it. [Oct 15]
**A:** It's gaps and islands. First I dedupe to one row per user per day, casting timestamps to dates, so two purchases on the same day don't break the pattern. Then ROW_NUMBER per user, ordered by date. Inside a streak, the date and the row number both go up by 1, so date minus row number stays constant, and that becomes the streak label. I group by user and label, count the days, and keep groups with 3 or more. I use DISTINCT at the end because one user can have several qualifying streaks. After the dedupe, dates are unique per user, so ROW_NUMBER can't tie and no tie-breaker is needed.

**Status:**

### ROW_NUMBER or LAG: how do you choose? [Oct 15]
**A:** ROW_NUMBER gives each row its position; LAG puts the previous row's value on the current row. If the question compares values, like time since the last payment or change in amount, position doesn't help: row number 2 tells me nothing about minutes. Gaps and islands works with ROW_NUMBER only because consecutive calendar days line up with consecutive positions. A rule like "within 10 minutes" needs the actual previous timestamp, so LAG.

**Status:**

## SQL: general

### You don't know the interviewer's SQL dialect. What do you write? [Oct 13]
**A:** I write portable SQL: a CTE plus WHERE instead of QUALIFY, a / NULLIF(b, 0) instead of SAFE_DIVIDE, COALESCE instead of IFNULL, CASE WHEN instead of IF, and I repeat the expression in GROUP BY instead of using an alias. Date functions are the main difference between dialects, so I'd ask which one they use, or mention the equivalent, e.g. DATE_TRUNC in BigQuery vs DATE_FORMAT in MySQL.

**Status:**

### What happens if MySQL's ONLY_FULL_GROUP_BY is off? [Oct 13]
**A:** With it on, which is the MySQL 8 default, selecting a column that's neither grouped nor aggregated throws an error. With it off, MySQL silently returns a value from an arbitrary row in the group, so one result row can mix values from different rows, e.g. a card's max amount next to the date of a different transaction. I never rely on it: every column is a group key or inside an aggregate, and if I need values from one specific row, I use a window function to pick that row.

**Status:**

### No rows match your filter. What do SUM and COUNT return? [Oct 15]
**A:** COUNT returns 0; SUM returns NULL. For a metric like "duplicate charges today", the dashboard should show 0, not a blank, so I use COUNT, or wrap SUM in COALESCE(..., 0).

**Status:**

### Why cast timestamps to dates before deduping? [Oct 15]
**A:** Purchases at 10:15 and 18:40 on the same day are different timestamps, so DISTINCT keeps both, and ROW_NUMBER numbers them separately, which breaks a consecutive-day count. Casting to a date makes them identical, so they collapse into one row. Portable: CAST(x AS DATE). Postgres also allows x::date, and BigQuery DATE(x).

**Status:**

## Payments / fraud

### How would you detect duplicate charges? [Oct 15]
**A:** I partition by card, merchant and amount, order by timestamp with the transaction ID as a tie-breaker, and use LAG to bring the previous identical payment's timestamp onto each row. Then I flag rows where the gap is within the window, say 10 minutes, and count them. The gap syntax differs by dialect: in Postgres, timestamp minus timestamp is an interval, compared to INTERVAL '10 minutes'; MySQL uses TIMESTAMPDIFF(MINUTE, prev, ts); BigQuery uses TIMESTAMP_DIFF(ts, prev, MINUTE). The business point: the window size is a trade-off. Too wide flags genuine repeat purchases, like two coffees in an hour, and too narrow misses slow retries. I'd agree the window with the fraud or ops team and check how many flags each option produces.

**Status:**

## Excel

### VLOOKUP vs INDEX/MATCH vs XLOOKUP? [Oct 13]
**A:** VLOOKUP has three problems: it can only look to the right, it defaults to approximate match so a missing FALSE gives silent wrong answers, and its hard-coded column number breaks when someone inserts a column. INDEX/MATCH fixes all three and works in every Excel version. XLOOKUP is the cleanest, with a built-in not-found value and search in either direction, but it needs Excel 2021 or 365, and many bank machines run older versions. So I'm comfortable with both.

**Status:**

### A lookup key appears twice in the lookup table. Excel vs SQL? [Oct 13]
**A:** In Excel, XLOOKUP and INDEX/MATCH return the first match and silently ignore the rest. In SQL, a join returns one row per match, so a duplicate key multiplies rows, and any SUM afterwards is inflated. For example, a merchant listed twice in the merchant table would double its spend. In both tools I check the key is unique first; in SQL, GROUP BY key HAVING COUNT(*) > 1.

**Status:**
