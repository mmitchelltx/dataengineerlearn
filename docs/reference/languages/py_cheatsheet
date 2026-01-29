Ultimate Python Cheat Sheet (Basics + Data Engineering)

Generated: January 29, 2026

A practical reference for everyday Python and the patterns you’ll use in data engineering: files, formats, APIs, databases, Spark, concurrency, orchestration concepts, and production hygiene.

Tip: Keep this open while you code. Search for keywords like parquet, sqlalchemy, spark, async, or idempotency.

PYTHON BASICS

Install / run

python --version
python -m venv .venv
source .venv/bin/activate # mac/linux
..venv\Scripts\activate # windows
pip install -U pip
python script.py
python -m module_name

Syntax essentials

x = 10
pi = 3.14
name = "Marcus"
is_ok = True
nothing = None

Collections

lst = [1, 2, 3]
tup = (1, 2)
st = {1, 2, 3}
dct = {"a": 1, "b": 2}

Indexing / slicing

lst[0] # first
lst[-1] # last
lst[1:3] # slice
lst[:2] # up to 2
lst[::2] # step

Conditionals

if x > 10:
...
elif x == 10:
...
else:
...

msg = "big" if x > 10 else "small" # ternary

Loops

for item in lst:
...

for i in range(5): # 0..4
...

while x > 0:
x -= 1

for i, val in enumerate(lst):
...

for a, b in zip([1, 2], [3, 4]):
...

Comprehensions

squares = [n*n for n in range(10)]
evens = [n for n in range(20) if n % 2 == 0]
lookup = {k: len(k) for k in ["a", "bb"]}
unique = {n for n in [1, 1, 2, 3]}

Functions

def add(a: int, b: int = 0) -> int:
return a + b

def f(*args, **kwargs):
...

Exceptions

try:
risky()
except ValueError as e:
print(e)
else:
print("no error")
finally:
cleanup()

Classes (minimal)

from dataclasses import dataclass

@dataclass
class Job:
name: str
retries: int = 3

Handy built-ins

len(x), sum(x), min(x), max(x)
sorted(x), reversed(x)
any(cond for ...), all(cond for ...)
map(fn, it), filter(fn, it)

STRINGS & DATES

Strings

s = "hello"
s.upper()
s.replace("h", "H")
" - ".join(["a", "b"])
"abc".startswith("a")

f-strings

x = 12.3456
f"value={x:.2f}" # 12.35
f"{name=}" # name='Marcus'

datetime / timezone

from datetime import datetime, timezone
now_utc = datetime.now(timezone.utc)

from zoneinfo import ZoneInfo
now_chi = datetime.now(ZoneInfo("America/Chicago"))

FILES, PATHS, AND CONFIG

Paths (pathlib)

from pathlib import Path

p = Path("data") / "file.csv"
p.exists()
p.parent.mkdir(parents=True, exist_ok=True)

Read/write text

from pathlib import Path

text = Path("notes.txt").read_text(encoding="utf-8")
Path("out.txt").write_text("hi\n", encoding="utf-8")

JSON

import json

obj = json.loads('{"a": 1}')
s = json.dumps(obj, indent=2, sort_keys=True)

Environment variables

import os

token = os.getenv("API_TOKEN")

Dotenv (.env)

pip install python-dotenv

from dotenv import load_dotenv
load_dotenv()

DATA STRUCTURES FOR DATA ENGINEERING

defaultdict / Counter / deque

from collections import defaultdict, Counter, deque

g = defaultdict(list)
g["a"].append(1)

cnt = Counter(["a", "b", "a"]) # {'a': 2, 'b': 1}

q = deque([1, 2, 3])
q.appendleft(0)
q.pop()

itertools (streaming-friendly)

import itertools as it

first10 = list(it.islice(range(100), 10))

TYPE HINTS (worth it)

Core patterns

from typing import Iterator, Optional

def read_lines(path: str) -> Iterator[str]:
...

x: Optional[int] = None

Type checking

pip install mypy
mypy your_code.py

LOGGING (production style)

Structured logging

import logging

logging.basicConfig(
level=logging.INFO,
format="%(asctime)s %(levelname)s %(name)s - %(message)s"
)

log = logging.getLogger("pipeline")
log.info("started")
log.exception("failed") # inside except

TESTING (pytest)

Run tests

pip install pytest
pytest -q

Example

def test_add():
assert 2 + 2 == 4

DATA ENGINEERING I/O + FORMATS

CSV (standard library)

import csv

with open("in.csv", newline="", encoding="utf-8") as f:
reader = csv.DictReader(f)
for row in reader:
...

with open("out.csv", "w", newline="", encoding="utf-8") as f:
writer = csv.DictWriter(f, fieldnames=["a", "b"])
writer.writeheader()
writer.writerow({"a": 1, "b": 2})

Pandas (quick transforms)

pip install pandas

import pandas as pd

df = pd.read_csv("in.csv")
df["total"] = df["a"] + df["b"]
df = df.dropna()
df.to_parquet("out.parquet", index=False)

Parquet (recommended)

pip install pyarrow

Pandas can write Parquet via pyarrow
df.to_parquet("out.parquet", index=False)

Avro (common in streaming)

pip install fastavro

fastavro: read/write Avro efficiently

APIs and HTTP

requests (simple)

pip install requests

import requests

r = requests.get("https://api.example.com/items
", timeout=30)
r.raise_for_status()
data = r.json()

Retries (tenacity)

pip install tenacity

from tenacity import retry, wait_exponential, stop_after_attempt

@retry(wait=wait_exponential(min=1, max=30), stop=stop_after_attempt(5))
def fetch():
...

DATABASES (core patterns)

SQLAlchemy + Postgres

pip install sqlalchemy psycopg2-binary

from sqlalchemy import create_engine, text

engine = create_engine("postgresql+psycopg2://user:pass@host:5432/db")

with engine.begin() as conn:
rows = conn.execute(
text("select * from my_table where id=:id"),
{"id": 1},
).all()

Bulk loads (high level)

Postgres: COPY from CSV
Snowflake: stage files + COPY INTO
BigQuery: load jobs from GCS
Redshift: COPY from S3

SPARK (PySpark essentials)

Read, transform, write

pip install pyspark

from pyspark.sql import SparkSession, functions as F

spark = SparkSession.builder.appName("job").getOrCreate()

df = spark.read.parquet("s3://bucket/path/")
df2 = (
df.filter(F.col("status") == "ACTIVE")
.groupBy("account_id")
.agg(F.count("*").alias("n"))
)

df2.write.mode("overwrite").parquet("s3://bucket/out/")

Useful tips

Prefer built-in Spark SQL functions over Python UDFs
Watch shuffles: groupBy, join, distinct
Cache only when reused: df.cache()

CONCURRENCY (ETL + APIs)

ThreadPoolExecutor (I/O bound)

from concurrent.futures import ThreadPoolExecutor

def work(x):
...

with ThreadPoolExecutor(max_workers=20) as ex:
results = list(ex.map(work, items))

multiprocessing (CPU bound)

from multiprocessing import Pool

def work(x):
...

with Pool() as p:
results = p.map(work, items)

async + aiohttp (lots of HTTP)

pip install aiohttp

import asyncio, aiohttp

async def fetch(session, url):
async with session.get(url) as r:
r.raise_for_status()
return await r.json()

async def main(urls):
async with aiohttp.ClientSession() as s:
return await asyncio.gather(*(fetch(s, u) for u in urls))

data = asyncio.run(main(urls))

DATA PIPELINE PATTERNS

Extract -> Transform -> Load skeleton

def extract(...) -> list[dict]:
...

def transform(rows: list[dict]) -> list[dict]:
...

def load(rows: list[dict]) -> None:
...

def run():
rows = extract()
rows = transform(rows)
load(rows)

Idempotency (must-have)

Use deterministic output paths: .../dt=YYYY-MM-DD/
Use upserts/merge keys in warehouses
Track watermarks (max timestamp processed)

Partitioning (files)

Example: s3://bucket/table/dt=2026-01-28/part-*.parquet

ORCHESTRATION & SCHEDULING CONCEPTS

Key ideas

Tools: Airflow / Dagster / Prefect
Backfills: rerun historical partitions safely
SLAs & alerts: notify on late/failed runs
Data quality checks: row counts, null thresholds, uniqueness

PACKAGING & PROJECT LAYOUT

Suggested layout

project/
src/myproj/
init.py
main.py
extract.py
transform.py
load.py
tests/
pyproject.toml
README.md

Editable install

pip install -e .

CLI ARGS (for jobs)

argparse

import argparse

p = argparse.ArgumentParser()
p.add_argument("--date", required=True)
args = p.parse_args()

QUICK DATA ENGINEERING DEBUG KIT

Snippets

inspect types

type(x), isinstance(x, dict)

safe dict access

x.get("key", "default")

pretty print

from pprint import pprint
pprint(obj)

measure time

import time
t0 = time.time()
...
print(time.time() - t0)