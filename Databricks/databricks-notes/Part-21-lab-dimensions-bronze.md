# Part 21 — 🧪 LAB 2: Dimensions → Bronze

> **Section goal:** Write your first real pipeline notebook. You'll declare explicit schemas, read CSVs from the volume, add audit columns, and land five Delta tables in the bronze layer — including the four the instructor skips over, reconstructed in full.

Covers transcript `02:36:31` – `02:40:08`.

> 💡 **On completeness:** the video writes the `brands` notebook in detail and then says *"you will repeat the same process for other dimension tables — I'm not going to write that code because it will take too long, but it is essentially the same logic."* This Part writes **all five**, plus a config-driven refactor showing how you'd actually build it in production.

---

## 0. What you'll build

```mermaid
flowchart LR
    subgraph RAW["📦 ecommerce.source_data.raw"]
        R1["brands/brands.csv"]
        R2["categories/categories.csv"]
        R3["products/products.csv"]
        R4["customers/customers.csv"]
        R5["date/calendar.csv"]
    end
    NB["📓 <b>1_dim_bronze</b><br/>explicit schema<br/>+ audit columns<br/>+ write Delta"]
    subgraph BRZ["🥉 ecommerce.bronze"]
        B1["brz_brands"]
        B2["brz_categories"]
        B3["brz_products"]
        B4["brz_customers"]
        B5["brz_calendar"]
    end
    RAW --> NB --> BRZ
    style RAW fill:#e8e8e8,stroke:#666
    style BRZ fill:#cd7f32,stroke:#8b5a2b,color:#fff
```

**Checklist:**

- [ ] Folder `project_ecommerce/medallion_processing_dim` exists
- [ ] Notebook `1_dim_bronze` created
- [ ] `brz_brands`, `brz_categories`, `brz_products`, `brz_customers`, `brz_calendar` all exist
- [ ] Every table has `source_file` and `ingested_at`
- [ ] `brz_products` has ~50,000 rows
- [ ] All tables are `delta` format and `MANAGED`

---

## 1. Set up the notebook

> *"From `raw` now we will transfer data to `bronze`, and we will begin with the **dimension tables**. So let's go to Workspace here, and under `project_ecommerce` let's create a folder. We will call it **`medallion_processing_dim`**. Under that let's create a new notebook — I will name it **`1_dim_bronze`**."*

| # | Action |
|---|--------|
| 1 | **`Workspace`** → `project_ecommerce` → **`Create`** → **`Folder`** → `medallion_processing_dim` |
| 2 | Inside it → **`Create`** → **`Notebook`** |
| 3 | Rename: **`1_dim_bronze`** |
| 4 | Language: **Python** · **`Connect`** → **`Serverless`** |

> 💡 **Why numbered names?** `1_dim_bronze`, `2_dim_silver`, `3_dim_gold` sort correctly in the file browser and make the execution order self-documenting — which matters a lot when you wire these into a job DAG in Part 28.

---

## 2. Imports and configuration

> *"In this notebook, what we will do is: whatever dimension data we have, we will create a table in the bronze layer and we will ingest that data. So let's **import some necessary Python functions** first. Then the **catalog name** is `ecommerce`."*

```python
# ── Cell 1 ────────────────────────────────────────────────────────────
import pyspark.sql.functions as F
import pyspark.sql.types     as T

# ── Cell 2 ────────────────────────────────────────────────────────────
catalog_name  = "ecommerce"
raw_base_path = "/Volumes/ecommerce/source_data/raw"
```

### 🔍 Plain-English deep-dive: why put these in variables

**Analogy:** writing your address once at the top of a form instead of on every line.

| Benefit | Detail |
|---------|--------|
| **Change once** | Point the whole notebook at `dev_ecommerce` by editing one line |
| **Parameterisable** | Becomes `dbutils.widgets.get("catalog")` when this runs as a job task (Part 28) |
| **No typos** | `f"{catalog_name}.bronze.brz_brands"` beats retyping a 3-part name five times |
| **Environment promotion** | The same notebook serves dev and prod with a different parameter |

```python
# The production version — Part 28 passes these in
dbutils.widgets.text("catalog", "ecommerce", "Target catalog")
catalog_name = dbutils.widgets.get("catalog")
```

> ⚠️ **Notice what we are *not* doing: `USE CATALOG`.** Part 20 explained why — session state breaks in job runs. Here we build fully qualified names with an f-string instead.

---

## 3. The `brands` table — worked in full

### 3.1 Declare the schema

> *"Then we will **define the schema** for our brand dimension table. We already saw brands has **brand code, name and category code** — these three. See, they all are strings, so they are all `string` type."*

```python
# ── Cell 3 ────────────────────────────────────────────────────────────
brand_schema = T.StructType([
    T.StructField("brand_code",    T.StringType(), True),
    T.StructField("brand_name",    T.StringType(), True),
    T.StructField("category_code", T.StringType(), True),
])
```

> ⭐ **Why every column is `StringType` at bronze** — the principle from Part 17:
>
> Bronze's job is to **land the data, not judge it**. If you declared `weight_in_grams DOUBLE` and the file contains `"150g"`, the load **fails** and you've ingested nothing. As `STRING`, the load succeeds and you fix it in silver where you have room to handle it properly. See Part 9 for why `inferSchema` isn't the answer either.

### 3.2 Point at the file

> *"And also in source we have data at this path — OK, so let's copy this particular path — and we'll create a variable called `raw_data_path` using that path. And in that, whatever CSV you have, you will use it for brands."*

```python
# ── Cell 4 ────────────────────────────────────────────────────────────
raw_data_path = f"{raw_base_path}/brands/brands.csv"
```

> 💡 **Get the path by copying it from the UI.** Catalog → the volume → the file → **`Copy path`**. Typing volume paths by hand is a reliable way to lose ten minutes.

### 3.3 Read it

> *"Then **create a DataFrame** using this. And the way you do it is: you specify `raw_data_path` in this parameter, the **schema** is that schema, right? It is a **comma-separated** file, so that's why you're specifying this parameter. And **`header = True`**, which means that this particular file has this header."*

```python
# ── Cell 5 ────────────────────────────────────────────────────────────
df = (spark.read
        .schema(brand_schema)          # ⚠️ a METHOD, not an option (Part 9)
        .option("sep", ",")
        .option("header", "true")      # first row is column names, not data
        .csv(raw_data_path))
```

| Argument | Why it's there |
|----------|----------------|
| `.schema(brand_schema)` | Deterministic types, no extra inference scan |
| `.option("sep", ",")` | Explicit — self-documenting, and trivially changed for a TSV |
| `.option("header", "true")` | Without it, the header row becomes data and columns become `_c0` |

### 3.4 Add the audit columns

> *"And in that DataFrame we will **add the metadata columns**. So metadata columns are the **source file** and **ingested at**. So whenever there is a problem, when you want to do any **audit**, these columns will be useful."*

```python
# ── Cell 6 ────────────────────────────────────────────────────────────
df = (df
      .withColumn("source_file", F.col("_metadata.file_path"))
      .withColumn("ingested_at", F.current_timestamp()))

display(df)
```

### 🔍 Plain-English deep-dive: why audit columns earn their keep

**Analogy:** a postmark on an envelope. It doesn't change the letter, but it proves where it came from and when it arrived.

| Column | Answers | Real scenario |
|--------|---------|---------------|
| `source_file` | *"Which file did this row come from?"* | Finance reports Tuesday's numbers are wrong → filter to that file → find the bad extract → reprocess only that day |
| `ingested_at` | *"When did we load this?"* | Detect a stalled feed, prove SLA compliance, order duplicate loads |

Extra ones worth adding in a real pipeline:

```python
.withColumn("source_file_modified", F.col("_metadata.file_modification_time"))
.withColumn("ingested_by",          F.lit("1_dim_bronze"))
.withColumn("batch_id",             F.lit(dbutils.widgets.get("run_id")))
```

> ⚠️ **Use `_metadata.file_path`, not `input_file_name()`.** The instructor hits this himself at `02:17:00`: *"`input_file_name` is not supported"* on serverless compute. `_metadata` works everywhere and across formats — Part 6 §8.

**Expected output:**

> *"All right, our DataFrame looks good. See, these are the **three main columns**, these are the **two metadata columns**."*

| brand_code | brand_name | category_code | source_file | ingested_at |
|-----------|-----------|---------------|-------------|-------------|
| NOVA | `Nova Wave ` | ce | `dbfs:/Volumes/…/brands.csv` | 2026-08-24 11:03:22 |

> 👀 **Look closely — the defects from Part 18 are already visible:** the trailing space after `Nova Wave`, the lowercase `ce`. **You do not fix them here.** Silver's job.

### 3.5 Write to Delta

> *"So the idea here is: you load data from CSV into the Spark DataFrame, and then from the Spark DataFrame you will **write to a table** in Databricks — and that's a **Delta table**."*

```python
# ── Cell 7 ────────────────────────────────────────────────────────────
(df.write
   .format("delta")
   .mode("overwrite")
   .option("mergeSchema", "true")
   .saveAsTable(f"{catalog_name}.bronze.brz_brands"))
```

> *"So you will say `df.write`, format is **delta**, and you will say mode **overwrite** — let's say if there is an existing table you want to override it. **`mergeSchema` is true** — so let's say tomorrow if you get two new columns, then it will merge those columns. This is for **schema evolution**. And you will call your table **`brz_brands`**."*

Every argument, unpacked:

| Argument | Meaning | Why here |
|----------|---------|----------|
| `.format("delta")` | Write as a Delta table | ACID, time travel, schema enforcement (Part 7). It's the default, but being explicit is good practice |
| `.mode("overwrite")` | Replace the table if it exists | **Idempotent** — re-running gives the same result, no duplicates. Correct for a full-refresh backfill |
| `.option("mergeSchema","true")` | Allow new columns on write | **Schema evolution** — bronze should absorb what the source sends (Part 7 §6) |
| `.saveAsTable(name)` | Register in Unity Catalog | vs `.save(path)`, which writes files with no governance (Part 9 §9) |

> ⚠️ **`overwrite` is correct *here* and wrong later.** This is a historical backfill, so replacing the whole table each run is right. For the daily incremental case in Part 28, `overwrite` would destroy history and plain `append` would duplicate on retry — you'd want `MERGE` or overwrite-by-partition. Being able to explain *why the right answer changes with the load pattern* is what an interviewer is listening for.

### 3.6 Verify

> *"So let's execute this. This is done. Now let me go here, **refresh**, and under bronze now I see `brz_brands` — and if you look at the sample data, you have to select a compute of course. See, my table is created successfully."*

```python
# ── Cell 8 ────────────────────────────────────────────────────────────
chk = spark.table(f"{catalog_name}.bronze.brz_brands")
chk.printSchema()
print("rows:", chk.count())
display(chk.limit(10))
```

```sql
%sql
DESCRIBE EXTENDED ecommerce.bronze.brz_brands;
-- Type: MANAGED   |   Provider: delta
```

**✅ Checkpoint:** `brz_brands` exists, is `MANAGED` and `delta`, and has 5 columns (3 source + 2 audit).

---

## 4. The other four dimensions

> *"Now you will repeat the same process for other dimension tables. So I'm not going to write that code because it will take too long, but **it is essentially the same logic** — you go to your category dimension table, your products and so on. See, for category you define the schema — exactly same process."*

Here they are, written out.

> ⚠️ **Verify the column names against your actual files first.** Run the profiling code from Part 20 §8. These schemas are reconstructed from what the video shows and from the transformations applied in Part 22 — the *names* may differ slightly in your download.

### 4.1 `brz_categories`

```python
# ── categories ────────────────────────────────────────────────────────
category_schema = T.StructType([
    T.StructField("category_code", T.StringType(), True),
    T.StructField("category_name", T.StringType(), True),
])

df_cat = (spark.read
          .schema(category_schema)
          .option("sep", ",").option("header", "true")
          .csv(f"{raw_base_path}/categories/categories.csv")
          .withColumn("source_file", F.col("_metadata.file_path"))
          .withColumn("ingested_at", F.current_timestamp()))

(df_cat.write.format("delta").mode("overwrite")
       .option("mergeSchema", "true")
       .saveAsTable(f"{catalog_name}.bronze.brz_categories"))

display(df_cat)
```

### 4.2 `brz_products` — the big one

> *"You go to your products and so on."*

~50,000 rows, 14 columns.

```python
# ── products ──────────────────────────────────────────────────────────
product_schema = T.StructType([
    T.StructField("product_id",      T.StringType(), True),
    T.StructField("sku",             T.StringType(), True),
    T.StructField("product_name",    T.StringType(), True),
    T.StructField("category_code",   T.StringType(), True),
    T.StructField("brand_code",      T.StringType(), True),
    T.StructField("color",           T.StringType(), True),
    T.StructField("size",            T.StringType(), True),
    T.StructField("material",        T.StringType(), True),
    T.StructField("weight_in_grams", T.StringType(), True),   # ⚠️ contains "150g"
    T.StructField("length",          T.StringType(), True),   # ⚠️ contains "1,5"
    T.StructField("rating",          T.StringType(), True),
    T.StructField("rating_count",    T.StringType(), True),   # ⚠️ negative values
    T.StructField("created_at",      T.StringType(), True),
    T.StructField("updated_at",      T.StringType(), True),
])

df_prod = (spark.read
           .schema(product_schema)
           .option("sep", ",").option("header", "true")
           .csv(f"{raw_base_path}/products/products.csv")
           .withColumn("source_file", F.col("_metadata.file_path"))
           .withColumn("ingested_at", F.current_timestamp()))

(df_prod.write.format("delta").mode("overwrite")
        .option("mergeSchema", "true")
        .saveAsTable(f"{catalog_name}.bronze.brz_products"))

print("product rows:", df_prod.count())   # ~50,000
display(df_prod.limit(10))
```

> 💡 **Look at the three commented columns.** `weight_in_grams` as `"150g"`, `length` as `"1,5"`, `rating_count` negative — declaring them `STRING` is precisely why the load succeeds. A typed schema here would fail on row one.

### 4.3 `brz_customers`

```python
# ── customers ─────────────────────────────────────────────────────────
customer_schema = T.StructType([
    T.StructField("customer_id",  T.StringType(), True),   # ⚠️ ~300 nulls
    T.StructField("phone",        T.StringType(), True),   # ⚠️ many nulls
    T.StructField("country_code", T.StringType(), True),
    T.StructField("state",        T.StringType(), True),
])

df_cust = (spark.read
           .schema(customer_schema)
           .option("sep", ",").option("header", "true")
           .csv(f"{raw_base_path}/customers/customers.csv")
           .withColumn("source_file", F.col("_metadata.file_path"))
           .withColumn("ingested_at", F.current_timestamp()))

(df_cust.write.format("delta").mode("overwrite")
        .option("mergeSchema", "true")
        .saveAsTable(f"{catalog_name}.bronze.brz_customers"))

print("customer rows:", df_cust.count())
print("null customer_id:", df_cust.filter(F.col("customer_id").isNull()).count())   # ~300
display(df_cust.limit(10))
```

### 4.4 `brz_calendar` — and why the date stays a string

> *"Now in date, you might be like: OK, **why is date a string type?** Well, we want to **store the data in the same format as a string**, and when we do the silver processing, at that time we will create the date data type."*

```python
# ── calendar / date dimension ─────────────────────────────────────────
calendar_schema = T.StructType([
    T.StructField("date",         T.StringType(), True),   # ⚠️ "14-08-2025" — dd-MM-yyyy
    T.StructField("day_name",     T.StringType(), True),   # ⚠️ inconsistent casing
    T.StructField("month",        T.StringType(), True),
    T.StructField("year",         T.StringType(), True),
    T.StructField("quarter",      T.StringType(), True),   # ⚠️ bare "3"
    T.StructField("week_of_year", T.StringType(), True),   # ⚠️ negative values
])

df_cal = (spark.read
          .schema(calendar_schema)
          .option("sep", ",").option("header", "true")
          .csv(f"{raw_base_path}/date/calendar.csv")
          .withColumn("source_file", F.col("_metadata.file_path"))
          .withColumn("ingested_at", F.current_timestamp()))

(df_cal.write.format("delta").mode("overwrite")
       .option("mergeSchema", "true")
       .saveAsTable(f"{catalog_name}.bronze.brz_calendar"))

display(df_cal.limit(10))
```

> ⭐ **This is a genuinely good interview point.** The dates in the file are `dd-MM-yyyy`, which is *not* Spark's default `yyyy-MM-dd`. If you declared `DateType` here, either the parse fails or — worse, on a legacy parser policy — every date silently becomes null. Reading as string means the file always lands; Part 22 converts it with an explicit format. **Bronze lands, silver interprets.**

---

## 5. The DRY refactor — how you'd really write this

Five near-identical blocks is fine for teaching and poor for maintenance. Here's the config-driven version. **Showing this in an interview signals you think about maintainability, not just correctness.**

```python
# ── Cell A · configuration ────────────────────────────────────────────
import pyspark.sql.functions as F
import pyspark.sql.types     as T

catalog_name  = "ecommerce"
raw_base_path = "/Volumes/ecommerce/source_data/raw"

def all_strings(*cols):
    """Bronze reads everything as StringType — land it, don't judge it."""
    return T.StructType([T.StructField(c, T.StringType(), True) for c in cols])

DIMENSIONS = [
    {
        "name":   "brands",
        "path":   f"{raw_base_path}/brands/brands.csv",
        "target": "brz_brands",
        "schema": all_strings("brand_code", "brand_name", "category_code"),
    },
    {
        "name":   "categories",
        "path":   f"{raw_base_path}/categories/categories.csv",
        "target": "brz_categories",
        "schema": all_strings("category_code", "category_name"),
    },
    {
        "name":   "products",
        "path":   f"{raw_base_path}/products/products.csv",
        "target": "brz_products",
        "schema": all_strings("product_id", "sku", "product_name", "category_code",
                              "brand_code", "color", "size", "material",
                              "weight_in_grams", "length", "rating", "rating_count",
                              "created_at", "updated_at"),
    },
    {
        "name":   "customers",
        "path":   f"{raw_base_path}/customers/customers.csv",
        "target": "brz_customers",
        "schema": all_strings("customer_id", "phone", "country_code", "state"),
    },
    {
        "name":   "calendar",
        "path":   f"{raw_base_path}/date/calendar.csv",
        "target": "brz_calendar",
        "schema": all_strings("date", "day_name", "month", "year",
                              "quarter", "week_of_year"),
    },
]

# ── Cell B · the single reusable function ─────────────────────────────
def ingest_to_bronze(cfg: dict, catalog: str) -> int:
    """Read one source CSV, stamp audit columns, write a bronze Delta table."""
    df = (spark.read
            .schema(cfg["schema"])
            .option("sep", ",")
            .option("header", "true")
            .csv(cfg["path"])
            .withColumn("source_file", F.col("_metadata.file_path"))
            .withColumn("ingested_at", F.current_timestamp()))

    target = f"{catalog}.bronze.{cfg['target']}"
    (df.write
       .format("delta")
       .mode("overwrite")
       .option("mergeSchema", "true")
       .saveAsTable(target))

    return spark.table(target).count()

# ── Cell C · run them all ─────────────────────────────────────────────
results = {}
for cfg in DIMENSIONS:
    n = ingest_to_bronze(cfg, catalog_name)
    results[cfg["target"]] = n
    print(f"✅ {cfg['name']:<12} → {catalog_name}.bronze.{cfg['target']:<16} {n:>7,} rows")

# ── Cell D · assert, don't eyeball ────────────────────────────────────
assert results["brz_products"] > 40_000, "products row count looks wrong"
assert all(n > 0 for n in results.values()), "an empty table was produced"
print("\n🎉 All bronze dimension tables loaded.")
```

| Benefit | Why it matters |
|---------|----------------|
| **One code path** | A bug fix or a new audit column is applied in one place, not five |
| **Adding a dimension is one dict** | Not a copy-paste of 15 lines |
| **Testable** | `ingest_to_bronze` is a pure-ish function you can unit-test |
| **Job-ready** | Loops naturally over a config that could come from a parameter |
| **Assertions built in** | Failures are loud, not discovered three layers later |

> ⚠️ **The trade-off, stated honestly:** the loop version is harder to debug when *one* dimension fails, and it hides per-table specifics. For five similar tables it's clearly right; for five tables needing genuinely different handling, explicit blocks are clearer. Say that in an interview — recognising when DRY is *over*-applied is a senior signal.

---

## 6. Verify everything

```python
# ── Structural verification ───────────────────────────────────────────
expected = ["brz_brands", "brz_categories", "brz_products",
            "brz_customers", "brz_calendar"]

actual = [r.tableName for r in spark.sql(f"SHOW TABLES IN {catalog_name}.bronze").collect()]

for t in expected:
    print(f"{'✅' if t in actual else '❌'} {t}")

# ── Row counts and audit-column presence ──────────────────────────────
for t in expected:
    df = spark.table(f"{catalog_name}.bronze.{t}")
    has_audit = {"source_file", "ingested_at"}.issubset(set(df.columns))
    print(f"{t:<18} {df.count():>7,} rows | audit cols: {'✅' if has_audit else '❌'}")
```

```sql
%sql
SHOW TABLES IN ecommerce.bronze;

SELECT table_name, table_type
FROM   ecommerce.information_schema.tables
WHERE  table_schema = 'bronze'
ORDER  BY table_name;

DESCRIBE EXTENDED ecommerce.bronze.brz_products;
DESCRIBE HISTORY  ecommerce.bronze.brz_brands;      -- ✅ version 0 already exists
```

**Prove idempotency — run the whole notebook twice:**

```python
before = spark.table(f"{catalog_name}.bronze.brz_brands").count()
# … re-run the ingestion cells …
after  = spark.table(f"{catalog_name}.bronze.brz_brands").count()
assert before == after, "❌ not idempotent — overwrite should give identical counts"
print("✅ idempotent")
```

> 💡 `DESCRIBE HISTORY` will show **two versions** after the second run, both `WRITE` with `mode=Overwrite`. Same data, full audit trail. That's Delta earning its keep (Part 7).

**✅ Checkpoint:** all 5 tables exist, all have audit columns, `brz_products` has ~50,000 rows, and a second run leaves counts unchanged.

---

## 7. What you deliberately did *not* do

Worth stating explicitly, because it's the discipline that makes medallion work.

| Tempting | Why you resisted |
|----------|------------------|
| Trim the trailing space in `brand_name` | ❌ Bronze must match the source. Silver's job |
| Cast `weight_in_grams` to double | ❌ Would fail on `"150g"`. Silver's job |
| Parse `date` to a `DateType` | ❌ `dd-MM-yyyy` isn't Spark's default; risks nulls. Silver's job |
| Drop rows with a null `customer_id` | ❌ Bronze keeps everything. Silver decides |
| Deduplicate `categories` | ❌ Silver's job |
| Uppercase the codes | ❌ Silver's job |

> 🧠 **The bronze rule in one line: *change nothing except adding metadata.***

> ⭐ **Interview:** *"Why not clean the data as you ingest it — wouldn't that be more efficient?"* → *"Because it destroys the faithful record. If cleaning happens during ingest and the logic turns out to be wrong six months later, the original values are gone and most source systems can't reproduce history. Keeping bronze as-received means I can fix the silver logic and reprocess from bronze without going back to the source at all. It also separates concerns — a source schema change is a bronze problem, a bad code value is a silver problem, a redefined metric is a gold problem — so each type of change has exactly one place to fix it. The extra storage is trivial against that."*

---

## 8. 🚑 Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| `Path does not exist` | Wrong volume path or flattened upload | `dbutils.fs.ls(raw_base_path)`; re-upload folders |
| All values null | Schema column **order** doesn't match the CSV | `StructType` maps **by position** — compare with a headerless read |
| Fewer columns than expected | Schema declares fewer fields than the file has | Extra file columns are dropped silently — count them |
| Header appears as a data row | Missing `header` option | `.option("header","true")` |
| `input_file_name is not supported` | Serverless compute | Use `F.col("_metadata.file_path")` |
| `A schema mismatch detected` | Re-running with different columns, no evolution | `.option("mergeSchema","true")` |
| Table exists but is empty | Read matched no files, or file is header-only | Check `df.count()` **before** writing |
| Row count doubles on re-run | Used `append` instead of `overwrite` | `.mode("overwrite")` for backfill |
| `TABLE_OR_VIEW_NOT_FOUND` on verify | Schema not created | Re-run the Part 20 setup notebook |
| Products load is slow | 50,000 rows across small files | Normal; check the Spark UI if it's minutes |

### ⚠️ The positional-schema trap, in detail

`StructType` matches **by position, not by name**, when a schema is supplied.

```python
# File header:  brand_code, brand_name, category_code
# Your schema:  brand_code, category_code, brand_name    ← swapped!
```
No error. Silently, `brand_name` now contains category codes. This is one of the nastiest bugs in Spark ingestion.

**Guard against it:**

```python
# Read with inference first and compare the column ORDER
probe = spark.read.option("header", "true").csv(raw_data_path)
print("file order  :", probe.columns)
print("schema order:", [f.name for f in brand_schema.fields])
assert probe.columns == [f.name for f in brand_schema.fields], "❌ column order mismatch"
```

---

## 9. The complete notebook

```python
# Databricks notebook source
# MAGIC %md
# MAGIC # 1 · Dimensions → Bronze
# MAGIC Reads the five dimension CSVs from `ecommerce.source_data.raw`,
# MAGIC stamps audit columns, and writes Delta tables into `ecommerce.bronze`.
# MAGIC
# MAGIC **Bronze rule:** change nothing except adding metadata. All types are `STRING`.
# MAGIC **Idempotent:** `mode("overwrite")` — safe to re-run.

# COMMAND ----------
import pyspark.sql.functions as F
import pyspark.sql.types     as T

dbutils.widgets.text("catalog", "ecommerce", "Target catalog")
catalog_name  = dbutils.widgets.get("catalog")
raw_base_path = "/Volumes/ecommerce/source_data/raw"

# COMMAND ----------
def all_strings(*cols):
    return T.StructType([T.StructField(c, T.StringType(), True) for c in cols])

DIMENSIONS = [
    {"name": "brands",     "path": f"{raw_base_path}/brands/brands.csv",
     "target": "brz_brands",
     "schema": all_strings("brand_code", "brand_name", "category_code")},

    {"name": "categories", "path": f"{raw_base_path}/categories/categories.csv",
     "target": "brz_categories",
     "schema": all_strings("category_code", "category_name")},

    {"name": "products",   "path": f"{raw_base_path}/products/products.csv",
     "target": "brz_products",
     "schema": all_strings("product_id", "sku", "product_name", "category_code",
                           "brand_code", "color", "size", "material",
                           "weight_in_grams", "length", "rating", "rating_count",
                           "created_at", "updated_at")},

    {"name": "customers",  "path": f"{raw_base_path}/customers/customers.csv",
     "target": "brz_customers",
     "schema": all_strings("customer_id", "phone", "country_code", "state")},

    {"name": "calendar",   "path": f"{raw_base_path}/date/calendar.csv",
     "target": "brz_calendar",
     "schema": all_strings("date", "day_name", "month", "year",
                           "quarter", "week_of_year")},
]

# COMMAND ----------
def ingest_to_bronze(cfg, catalog):
    df = (spark.read
            .schema(cfg["schema"])
            .option("sep", ",")
            .option("header", "true")
            .csv(cfg["path"])
            .withColumn("source_file", F.col("_metadata.file_path"))
            .withColumn("ingested_at", F.current_timestamp()))

    target = f"{catalog}.bronze.{cfg['target']}"
    (df.write.format("delta").mode("overwrite")
       .option("mergeSchema", "true").saveAsTable(target))
    return spark.table(target).count()

# COMMAND ----------
results = {}
for cfg in DIMENSIONS:
    n = ingest_to_bronze(cfg, catalog_name)
    results[cfg["target"]] = n
    print(f"✅ {cfg['name']:<12} → {cfg['target']:<16} {n:>7,} rows")

# COMMAND ----------
assert all(n > 0 for n in results.values()), "❌ an empty bronze table was produced"
assert results["brz_products"] > 40_000,     "❌ products row count looks wrong"
print("🎉 Bronze dimension load complete.")

# COMMAND ----------
# MAGIC %sql
# MAGIC SHOW TABLES IN ecommerce.bronze;
```

---

## ⭐ Likely Interview Questions for This Section

**Q1. "Why declare an explicit schema instead of using `inferSchema`?"**
> *Model answer:* "Three reasons. It's deterministic — inference depends on the contents of that particular file, so one `N/A` value silently turns an integer column into a string and breaks everything downstream. It avoids an extra full scan, since inference reads the data once just to guess types. And it fails loudly on unexpected structure rather than silently adapting, which is what I want in a scheduled pipeline. I'll use `inferSchema` interactively to *discover* the schema, print `schema.toDDL()`, then freeze that into code. The one caveat is that a supplied schema matches by **position**, not name, so I assert the column order against the file header before trusting it."

**Q2. "Why is everything a `StringType` in bronze?"**
> *Model answer:* "Because bronze's job is to land the data, not judge it. The source files contain `'150g'` in a weight column, `'1,5'` with a comma decimal, and dates in `dd-MM-yyyy`. If bronze declared proper types, the load would fail on the first bad row and we'd have ingested nothing — the worst possible outcome, because now we've neither captured the data nor learned what's wrong with it. As strings, the load always succeeds, we have a faithful record, and silver does the casting with proper handling for the edge cases, including routing unparseable values to a quarantine table."

**Q3. "What audit columns do you add and why?"**
> *Model answer:* "At minimum `source_file` from `_metadata.file_path` and `ingested_at` from `current_timestamp()`. `source_file` means any row is traceable to the exact file it came from, so when someone reports that Tuesday's numbers are wrong I can filter to that file, identify a bad extract, and reprocess just that day rather than the whole history. `ingested_at` detects stalled feeds, proves SLA compliance, and orders duplicate loads. In production I'd add the notebook or job name and a batch or run ID so a row traces to a specific pipeline execution. It's the same reason you keep a postmark on an envelope — it doesn't change the letter but it proves provenance."

**Q4. "Why `mode('overwrite')` here, and would you always use it?"**
> *Model answer:* "No — it's right for *this* load pattern and wrong for others. This is a historical backfill of static dimension files, so replacing the table each run is correct and gives idempotency for free: re-running produces the identical result with no duplicates. For daily incremental loads, `overwrite` would destroy history and plain `append` isn't idempotent because a retried task duplicates rows. There I'd use `MERGE INTO` on the business key, which upserts, or overwrite a single partition, or Auto Loader with a checkpoint that tracks processed files. Recognising that the correct write mode depends on the load pattern rather than being a fixed preference is the actual point."

**Q5. "What does `mergeSchema` do, and is it safe?"**
> *Model answer:* "It permits schema evolution on write — if the DataFrame has columns the table doesn't, they're added rather than the write failing. It's appropriate at bronze because bronze should absorb whatever the source sends, and a new upstream column is usually additive and harmless. It's *not* safe as a blanket setting: a typo in a rename creates a junk column alongside the real one, and it can mask an upstream change that should have triggered a review. So my pattern is permissive at bronze and strict at silver and gold, with explicit column selection there, so no new column leaks into business logic unreviewed. And I'd never use `overwriteSchema`, which can silently drop columns."

**Q6. "You wrote five nearly identical blocks. How would you improve that?"**
> *Model answer:* "Drive it from configuration. A list of dictionaries — source path, target table, schema — and one `ingest_to_bronze` function looped over them. That means a bug fix or a new audit column is applied once instead of five times, adding a dimension is one dictionary entry rather than fifteen copied lines, and the function is testable in isolation. It also makes the notebook naturally parameterisable for a job. The honest trade-off is that a loop is harder to debug when one table fails and it hides genuine per-table differences — so for five near-identical tables it's clearly right, but if each needed materially different handling I'd keep them explicit. Over-applying DRY is its own problem."

**Q7. "The date column is `dd-MM-yyyy` but you left it as a string. Why not parse it in bronze?"**
> *Model answer:* "Because parsing is interpretation and bronze is capture. Spark's default date format is `yyyy-MM-dd`, so a typed read of `dd-MM-yyyy` either errors or, under a legacy parser policy, silently produces nulls for every row — which is far worse because it looks like it worked. Keeping it as a string guarantees the file lands, and silver converts it with an explicit format string where I can also validate the result and quarantine anything that fails. It's the same principle as the numeric columns: bronze lands, silver interprets."

**Q8. "How do you verify a bronze load succeeded?"**
> *Model answer:* "Assertions in code rather than looking at the UI. Confirm every expected table exists by querying `information_schema.tables`. Assert row counts are non-zero and within an expected range — a silently truncated upload producing a 12-row products table should fail the run, not proceed. Assert the audit columns are present. Compare the column order against the file header, since a positional schema mismatch is silent and devastating. Then re-run the whole notebook and assert counts are unchanged, which proves idempotency. And `DESCRIBE HISTORY` should show a clean `WRITE` operation per run, which is both a verification and an audit trail."

---

## 🧠 30-Second Memory Hooks

- **🧠 The bronze rule: *change nothing except adding metadata.***
- **Every column is `StringType()` at bronze** — `"150g"`, `"1,5"` and `dd-MM-yyyy` all land safely.
- **`.schema()` is a METHOD, not an option.** And it matches **by POSITION, not name** — assert the order.
- **Two audit columns: `source_file` (`_metadata.file_path`) + `ingested_at` (`current_timestamp()`).** The postmark on the envelope.
- **⚠️ `input_file_name()` is dead on serverless. Use `_metadata.file_path`.**
- **The write: `.format("delta").mode("overwrite").option("mergeSchema","true").saveAsTable(...)`.**
- **`overwrite` = idempotent for a backfill.** For daily incremental you'd need `MERGE` — the right mode depends on the load pattern.
- **`mergeSchema` permissive at bronze, strict at silver/gold.**
- **`.saveAsTable()` not `.save()`** — you want it governed in Unity Catalog.
- **Naming: `brz_` / `slv_` / `gld_` prefix, layer as the schema.**
- **Config-driven loop beats five copy-pasted blocks** — but say the trade-off out loud.
- **Assert, don't eyeball.** Row counts, audit columns, column order, and re-run for idempotency.
- **Defects are visible in bronze and you leave them there.** Trailing spaces, lowercase codes — all silver's job.

---

*Next suggested section:* **[Part 22 — 🧪 LAB 3: Dimensions → Silver](Part-22-lab-dimensions-silver.md)** — the biggest lab in the guide. Every cleaning technique in the course, in full: trim, regex, anomaly replacement, deduplication, casing, unit stripping, spelling fixes, null strategy, date parsing and quarter enrichment — across all five dimensions.

---

**Navigation** — ⬅️ **[Part 20 — LAB 1: Environment Setup](Part-20-lab-environment-setup.md)** · 🏠 **[Master Index](../Databricks%20-%20Study%20Guide.md)** · ➡️ **[Part 22 — LAB 3: Dimensions → Silver](Part-22-lab-dimensions-silver.md)**
