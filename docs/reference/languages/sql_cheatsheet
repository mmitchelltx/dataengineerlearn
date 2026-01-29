Ultimate SQL Cheat Sheet (Basics + Data Engineering)

A practical reference for querying, pipelines, warehouses, and data quality. Syntax varies slightly by database; examples reflect widely used patterns.

SQL FUNDAMENTALS

Query skeleton (order)

SELECT ...
FROM ...
JOIN ...
WHERE ...
GROUP BY ...
HAVING ...
ORDER BY ...
LIMIT ...

SELECT essentials

SELECT DISTINCT col
SELECT col AS alias
SELECT col1, col2
SELECT *

WHERE filters

WHERE col = 10
WHERE col <> 10
WHERE col IN (1,2,3)
WHERE col BETWEEN 10 AND 20
WHERE col LIKE 'abc%' -- starts with
WHERE col ILIKE 'abc%' -- case-insensitive (varies)
WHERE col IS NULL
WHERE col IS NOT NULL

Boolean logic gotchas

WHERE a = 1 AND (b = 2 OR b = 3)

-- NULL comparisons:
WHERE col = NULL -- WRONG
WHERE col IS NULL -- RIGHT

JOINS

Join types

-- Inner
SELECT ...
FROM a
JOIN b ON a.id = b.id;

-- Left
SELECT ...
FROM a
LEFT JOIN b ON a.id = b.id;

-- Full (if supported)
SELECT ...
FROM a
FULL OUTER JOIN b ON a.id = b.id;

-- Anti-join (rows in a not in b)
SELECT ...
FROM a
LEFT JOIN b ON a.id = b.id
WHERE b.id IS NULL;

-- Semi-join (rows in a with match in b)
SELECT ...
FROM a
WHERE EXISTS (SELECT 1 FROM b WHERE b.id = a.id);

Join pitfall: duplicate explosion

If a.id or b.id isn’t unique, joins can multiply rows. Deduplicate or aggregate before joining when appropriate.

AGGREGATION

GROUP BY

SELECT customer_id,
COUNT(*) AS n_orders,
SUM(amount) AS revenue
FROM orders
GROUP BY customer_id;

HAVING (filters after aggregation)

HAVING SUM(amount) > 1000

Common aggregates

COUNT(*)
COUNT(DISTINCT col)
SUM(col)
AVG(col)
MIN(col)
MAX(col)

SORTING + LIMITING

ORDER BY + LIMIT

ORDER BY col DESC, col2 ASC
LIMIT 100
FETCH FIRST 100 ROWS ONLY -- ANSI alternative (varies)

Top-N per group

SELECT *
FROM (
SELECT t.*,
ROW_NUMBER() OVER (PARTITION BY key ORDER BY updated_at DESC) AS rn
FROM t
) x
WHERE rn = 1;

WINDOW FUNCTIONS (DATA ENGINEERING WORKHORSES)

Partition + window aggregates

SUM(amount) OVER (PARTITION BY customer_id) AS customer_total
AVG(amount) OVER () AS global_avg

Ranking

ROW_NUMBER() OVER (PARTITION BY key ORDER BY ts DESC)
RANK() OVER (ORDER BY score DESC)
DENSE_RANK() OVER (ORDER BY score DESC)

Lag/Lead (time-series)

LAG(value) OVER (PARTITION BY id ORDER BY ts)
LEAD(value) OVER (PARTITION BY id ORDER BY ts)

Rolling windows

SUM(amount) OVER (
PARTITION BY id
ORDER BY ts
ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
) AS rolling_7

CTES, CASE, AND NULL HANDLING

CTE (WITH)

WITH filtered AS (
SELECT * FROM t WHERE status = 'active'
)
SELECT * FROM filtered;

Multiple CTEs

WITH a AS (...),
b AS (...)
SELECT ...
FROM a JOIN b USING (id);

CASE / COALESCE / NULLIF

CASE
WHEN amount > 100 THEN 'big'
WHEN amount > 0 THEN 'small'
ELSE 'zero_or_null'
END AS bucket

COALESCE(col, 0)
NULLIF(a, b)
amount / NULLIF(denom, 0)

SET OPERATIONS

UNION / INTERSECT / EXCEPT

SELECT ... UNION ALL SELECT ... -- keeps duplicates (faster)
SELECT ... UNION SELECT ... -- removes duplicates
SELECT ... INTERSECT SELECT ... -- common rows (if supported)
SELECT ... EXCEPT SELECT ... -- in first not second (if supported)

DATE/TIME PATTERNS

Time range filtering (best practice)

WHERE ts >= '2026-01-01' AND ts < '2026-02-01'

Truncation / bucketing (varies)

DATE_TRUNC('day', ts)
DATE_TRUNC('month', ts)

Timezone note

Prefer storing event timestamps in UTC; convert at the reporting edge.

DATA ENGINEERING SQL PATTERNS

Dedup to latest record

WITH ranked AS (
SELECT t.*,
ROW_NUMBER() OVER (PARTITION BY natural_key ORDER BY updated_at DESC) AS rn
FROM raw_table t
)
SELECT * FROM ranked WHERE rn = 1;

Incremental loads (high watermark)

SELECT *
FROM source
WHERE updated_at > (
SELECT COALESCE(MAX(updated_at), '1900-01-01') FROM target
);

Upserts / MERGE (warehouse pattern)

MERGE INTO target t
USING staging s
ON t.id = s.id
WHEN MATCHED THEN UPDATE SET col = s.col, updated_at = s.updated_at
WHEN NOT MATCHED THEN INSERT (id, col, updated_at)
VALUES (s.id, s.col, s.updated_at);

Surrogate keys (dim modeling)

-- Some engines: sequence/identity
-- Otherwise: stable hash
MD5(CONCAT(natural_key_1, '|', natural_key_2)) AS surrogate_key

DATA QUALITY CHECKS (SQL YOU RUN DAILY)

Uniqueness

SELECT key, COUNT() AS c
FROM t
GROUP BY key
HAVING COUNT() > 1;

Not-null

SELECT COUNT(*) AS nulls
FROM t
WHERE important_col IS NULL;

Referential integrity

SELECT f.*
FROM fact f
LEFT JOIN dim d ON f.dim_id = d.id
WHERE d.id IS NULL;

Freshness

SELECT MAX(loaded_at) AS last_load
FROM t;

Volume anomalies (simple)

SELECT CAST(ts AS DATE) AS d, COUNT(*) AS n
FROM t
GROUP BY 1
ORDER BY 1 DESC;

PERFORMANCE BASICS

Practical tips

• Select only needed columns (avoid SELECT * on wide tables).
• Push filters before joins when possible.
• Join on matching data types; avoid implicit casts.
• Pre-aggregate large tables before joining when appropriate.
• Prefer UNION ALL over UNION unless you need dedup.
• Use cross joins intentionally; otherwise avoid.

Partition pruning tip

Avoid wrapping partition columns in functions in WHERE clauses; use range predicates for better pruning.

COMMON PATTERNS

Latest row per key (QUALIFY if supported)

QUALIFY ROW_NUMBER() OVER (PARTITION BY key ORDER BY ts DESC) = 1;

Find missing IDs

SELECT a.id
FROM a
LEFT JOIN b ON a.id = b.id
WHERE b.id IS NULL;

Percent of total

SELECT
category,
SUM(amount) AS amt,
SUM(amount) / NULLIF(SUM(SUM(amount)) OVER (), 0) AS pct_total
FROM t
GROUP BY category;