# OCI Data Engineering Learning Plan (SQL → Python → Bash)
**Timeline:** 24 Weeks  
**Time Commitment:** ~6–8 hours/week  
**Goal:** Job-ready Junior Data Engineer (Oracle / OCI focused)

---

## TOOLS (ALL FREE)
- Oracle Live SQL (Oracle SQL practice)
- Oracle Cloud Infrastructure (Always Free Tier)
- Oracle Autonomous Database (ATP / ADW)
- Python 3
- GitHub
- Linux / Bash (local or VM)

---

# PHASE 1 — SQL FOUNDATIONS (Weeks 1–6)
Primary Tool: Oracle Live SQL

---

## Week 1 — SQL Fundamentals
**Topics**
- SELECT, WHERE, ORDER BY
- Data types
- NULL handling

**Free Resources**
- Oracle Live SQL
- SQLBolt (Lessons 1–6)

**Project: Customer Query Pack**
**Instructions**
1. Create tables: customers, orders
2. Insert sample data
3. Write queries:
   - Customers ordered by signup date
   - Orders from last 30 days
   - Customers with NULL phone numbers
4. Save as `week01_sql_basics.sql`
5. Write a README explaining each query

---

## Week 2 — JOINs & Relational Thinking
**Topics**
- INNER / LEFT JOIN
- Primary & foreign keys
- One-to-many relationships

**Free Resources**
- Mode SQL Tutorial (JOIN sections)

**Project: ERP Order Model**
**Instructions**
1. Create tables:
   - customers
   - orders
   - order_lines
   - products
2. Write a JOIN query returning:
   - Order ID
   - Customer name
   - Product name
   - Quantity
   - Line total
3. Validate row counts (no duplication)

---

## Week 3 — Aggregations & Metrics
**Topics**
- GROUP BY
- HAVING
- COUNT vs COUNT DISTINCT

**Free Resources**
- Mode SQL Tutorial (Aggregations)
- Oracle Live SQL examples

**Project: Finance Metrics Report**
**Instructions**
1. Write queries for:
   - Monthly revenue
   - Average order value
   - Revenue by product category
2. Comment each query clearly
3. Assume Finance will reuse this SQL

---

## Week 4 — CTEs & Window Functions
**Topics**
- WITH (CTEs)
- ROW_NUMBER, RANK
- Running totals

**Free Resources**
- Mode SQL (Window Functions)
- LeetCode (SQL problems only)

**Project: Customer Analytics**
**Instructions**
1. Rank customers by lifetime revenue
2. Identify first vs repeat purchases
3. Compute rolling 30-day revenue
4. Use CTEs only (no nested subqueries)

---

## Week 5 — Data Modeling
**Topics**
- OLTP vs Analytics
- Star schema
- Fact & dimension tables

**Free Resources**
- dbt Labs blog (modeling concepts)

**Project: Star Schema Build**
**Instructions**
1. Create:
   - fact_orders
   - dim_customer
   - dim_product
   - dim_date
2. Populate from transactional tables
3. Rewrite Week 3 metrics using star schema

---

## Week 6 — SQL Capstone
**Project: SQL Analytics System**
**Instructions**
1. Raw ERP-style tables
2. Transform into analytics schema
3. Deliver:
   - 10 business-facing queries
   - One README explaining data flow
4. Treat this as a resume project

---

# PHASE 2 — PYTHON FOR DATA ENGINEERING (Weeks 7–16)

---

## Week 7 — Python Fundamentals
**Topics**
- Variables, loops, functions
- Lists, dicts
- File I/O

**Free Resources**
- Python.org official tutorial

**Project: CSV Cleaner**
**Instructions**
1. Load a messy CSV
2. Standardize column names
3. Remove NULL rows
4. Output a cleaned CSV

---

## Week 8 — Python + Oracle Database
**Topics**
- cx_Oracle or SQLAlchemy
- Parameterized queries

**Free Resources**
- Oracle Python documentation

**Project: Python → Oracle Loader**
**Instructions**
1. Read cleaned CSV
2. Insert data into Oracle table
3. Log rows inserted
4. Handle failures gracefully

---

## Week 9 — Pandas Transformations
**Topics**
- DataFrames
- Joins
- Groupby

**Free Resources**
- Kaggle Pandas notebooks

**Project: Python ETL Transform**
**Instructions**
1. Extract data from Oracle DB
2. Transform using Pandas
3. Load analytics tables back to DB

---

## Week 10 — APIs & JSON
**Topics**
- REST APIs
- JSON normalization
- Pagination

**Free Resources**
- Real Python (free API articles)

**Project: API → Oracle Loader**
**Instructions**
1. Pull paginated API data
2. Normalize JSON
3. Store raw JSON for audit
4. Load structured data into Oracle DB

---

## Week 11 — Logging & Error Handling
**Topics**
- logging module
- try/except
- retry logic

**Project: Production ETL Script**
**Instructions**
1. Add structured logging
2. Separate config from code
3. Fail safely with meaningful errors

---

## Week 12 — File Formats & Optimization
**Topics**
- CSV vs Parquet
- Compression
- Storage vs performance tradeoffs

**Project: OCI Object Storage Optimization**
**Instructions**
1. Convert CSV to Parquet
2. Upload both to OCI Object Storage
3. Compare file size & load times
4. Document results

---

## Week 13 — Batch Processing
**Topics**
- Chunked reads
- Memory management

**Project: Large File Loader**
**Instructions**
1. Simulate 1M+ rows
2. Process in chunks
3. Load without memory errors

---

## Week 14 — OCI Autonomous Database Loading
**Topics**
- ATP vs ADW
- Batch commits
- Failure recovery

**Free Resources**
- Oracle Autonomous DB docs

**Project: OCI Batch Load Pattern**
**Instructions**
1. Create Autonomous DB (Always Free)
2. Load data in batches via Python
3. Handle partial failures
4. Validate row counts

---

## Week 15 — OCI Object Storage + IAM
**Topics**
- Buckets
- Pre-authenticated requests
- IAM concepts

**Free Resources**
- OCI Learning Library

**Project: Object Storage → DB Pipeline**
**Instructions**
1. Upload raw files to Object Storage
2. Read files with Python
3. Transform data
4. Load into Autonomous DB
5. Move processed files to `/processed/`

---

## Week 16 — Python Capstone
**Project: End-to-End OCI ETL System**
**Instructions**
- Sources: CSV + API
- Storage: OCI Object Storage
- Transform: Pandas
- Target: Oracle Autonomous DB
- Logging, config files, README

---

# PHASE 3 — BASH & AUTOMATION (Weeks 17–20)

---

## Week 17 — Bash Fundamentals
**Topics**
- ls, cd, pipes
- Environment variables

**Free Resources**
- Linux Foundation shell basics
- ExplainShell

**Project: ETL Runner Script**
**Instructions**
1. Bash script sets env vars
2. Runs Python ETL
3. Captures logs
4. Exits on failure

---

## Week 18 — Scheduling & Automation
**Topics**
- cron
- exit codes

**Project: Nightly OCI Batch Job**
**Instructions**
1. Schedule ETL with cron
2. Log runtime & rows processed
3. Simulate alerting via logs

---

## Week 19 — Data Validation
**Topics**
- Row count checks
- Basic reconciliation

**Project: Post-Load Validator**
**Instructions**
1. Compare source vs target counts
2. Log discrepancies
3. Fail pipeline on mismatch

---

## Week 20 — Bash Capstone
**Project: Production OCI Pipeline**
**Instructions**
1. Bash orchestrates full pipeline
2. Config-driven execution
3. Logging + validation included

---

# PHASE 4 — JOB READINESS (Weeks 21–24)
- Refactor repositories
- Improve READMEs
- Practice SQL interviews
- Practice explaining OCI architecture
- Prepare resume bullets per project

---

## FINAL OUTCOME
- 12–15 real OCI-aligned projects
- Oracle SQL mastery
- Python ETL experience
- OCI Object Storage + Autonomous DB pipelines
- Bash automation
- Recruiter-ready GitHub portfolio
