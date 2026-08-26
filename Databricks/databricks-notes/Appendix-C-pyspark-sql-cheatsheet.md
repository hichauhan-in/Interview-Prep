# Appendix C — PySpark & SQL Cheat Sheet

> Every command used across this guide, grouped by task, copy-paste ready. Keep this open while you work.

```python
# The standard header for every notebook in this guide
import pyspark.sql.functions as F
import pyspark.sql.types     as T
from pyspark.sql.window import Window
from pyspark.sql.functions import broadcast
```

> ⚠️ **Never `from pyspark.sql.functions import *`** — it shadows Python builtins (`sum`, `max`, `min`, `round`, `abs`, `filter`) and produces baffling bugs.

---

## 1. Session and context

```python
spark                                   # pre-created SparkSession in Databricks
spark.catalog.currentCatalog()          # 'ecommerce'
spark.catalog.currentDatabase()         # 'gold'
spark.catalog.listTables()
spark.conf.get("spark.sql.shuffle.partitions")
spark.conf.set("spark.sql.adaptive.enabled", "true")

# Outside Databricks
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("app").getOrCreate()
```

```sql
USE CATALOG ecommerce;      -- ⚠️ session state — fully qualify names in jobs
USE SCHEMA  gold;
SHOW CATALOGS;  SHOW SCHEMAS IN ecommerce;  SHOW TABLES IN ecommerce.gold;
```

---

## 2. Creating DataFrames

```python
df = spark.table("catalog.schema.table")
df = spark.sql("SELECT * FROM catalog.schema.table WHERE x > 1")

# From Python data — how you build test fixtures
df = spark.createDataFrame([(1, "a"), (2, "b")], "id INT, name STRING")
df = spark.createDataFrame(data, T.StructType([...]))
df = spark.createDataFrame(pandas_df)

df = spark.range(0, 1000)                # a quick synthetic DataFrame
```

---

## 3. Reading files

```python
# CSV — the production shape
df = (spark.read
        .schema(my_schema)                     # ⚠️ a METHOD, matches BY POSITION
        .option("header", "true")
        .option("sep", ",")
        .option("dateFormat", "yyyy-MM-dd")
        .option("mode", "PERMISSIVE")
        .option("columnNameOfCorruptRecord", "_corrupt_record")
        .csv("/Volumes/cat/schema/vol/file.csv"))

# Exploration only
df = spark.read.option("header","true").option("inferSchema","true").csv(path)

# Other formats
spark.read.parquet(path)
spark.read.json(path)
spark.read.format("delta").load(path)

# Many files
spark.read.csv("/Volumes/.../landing/")                  # whole directory
spark.read.csv("/Volumes/.../orders_2025-08-*.csv")      # glob
spark.read.option("recursiveFileLookup","true").csv(dir) # recurse

# Provenance (serverless-safe)
df = df.withColumn("source_file", F.col("_metadata.file_path"))
```

### Key CSV options

| Option | Default | Purpose |
|--------|---------|---------|
| `header` | `false` | Row 1 is column names |
| `inferSchema` | `false` | Guess types (extra scan) |
| `sep` | `,` | Delimiter |
| `multiLine` | `false` | Newlines inside quoted fields |
| `nullValue` | — | String meaning null |
| `dateFormat` | `yyyy-MM-dd` | Date pattern |
| `encoding` | `UTF-8` | Character set |
| `mode` | `PERMISSIVE` | `PERMISSIVE` / `DROPMALFORMED` / `FAILFAST` |

### Schemas

```python
# StructType
schema = T.StructType([
    T.StructField("id",   T.IntegerType(), True),
    T.StructField("name", T.StringType(),  True),
    T.StructField("amt",  T.DoubleType(),  True),
])

# DDL string — shorter, equally valid
schema = "id INT, name STRING, amt DOUBLE"

# Infer once, then freeze into code
print(df.schema)
print(df._jdf.schema().toDDL())
```

| Type | SQL | Use for |
|------|-----|---------|
| `StringType()` | `STRING` | Text, **codes with leading zeros** |
| `IntegerType()` | `INT` | Counts |
| `LongType()` | `BIGINT` | Large IDs |
| `DoubleType()` | `DOUBLE` | Measurements |
| `DecimalType(18,2)` | `DECIMAL` | ⭐ **Money** |
| `BooleanType()` | `BOOLEAN` | Flags |
| `DateType()` | `DATE` | Dates |
| `TimestampType()` | `TIMESTAMP` | Date + time |

---

## 4. Writing

```python
# Managed Delta table — the default
(df.write.format("delta").mode("overwrite")
   .option("mergeSchema", "true")          # bronze: allow new columns
   .saveAsTable("cat.schema.table"))

# Silver/gold — types change, so replace the schema
(df.write.format("delta").mode("overwrite")
   .option("overwriteSchema", "true")
   .saveAsTable("cat.schema.table"))

# External table
(df.write.format("delta").mode("overwrite")
   .option("path", "abfss://ctr@acct.dfs.core.windows.net/bronze/")
   .saveAsTable("cat.schema.table"))

# Files
df.write.mode("overwrite").parquet(path)
df.coalesce(1).write.mode("overwrite").csv(path, header=True)   # ⚠️ small data only
df.write.mode("overwrite").partitionBy("country").parquet(path)
```

| Mode | Behaviour |
|------|-----------|
| `overwrite` | Replace — idempotent for backfills |
| `append` | Add — ⚠️ **not** idempotent |
| `ignore` | Skip if it exists |
| `errorifexists` | Default — throws |

---

## 5. Inspecting

```python
df.printSchema()          # ⭐ after every read and transform
df.dtypes                 # [('id','int'), …]
df.columns
df.count()
df.isEmpty()
display(df)               # Databricks rich grid
df.show(5, truncate=False)
display(df.describe())    # count/mean/stddev/min/max
display(df.summary())     # + percentiles
df.rdd.getNumPartitions()
df.explain("formatted")
```

```sql
DESCRIBE TABLE     cat.schema.tbl;
DESCRIBE EXTENDED  cat.schema.tbl;    -- Type: MANAGED/EXTERNAL, Location
DESCRIBE DETAIL    cat.schema.tbl;    -- files, size, partitions
DESCRIBE HISTORY   cat.schema.tbl;    -- versions
SHOW CREATE TABLE  cat.schema.tbl;
```

---

## 6. Selecting and filtering

```python
df.select("a", "b")
df.select(F.col("a"), F.col("b").alias("bee"))
df.select("*", F.lit("x").alias("const"))
df.selectExpr("a", "b * 2 AS double_b")
df.drop("a", "b")

# ⚠️ Use & | ~ — never and/or/not — and parenthesise EVERY condition
df.filter((F.col("a") > 1) & (F.col("b") < 10))
df.where(F.col("a") > 1)                        # exact alias of filter
df.filter(F.col("a").between(1, 10))
df.filter(F.col("a").isin("x", "y"))
df.filter(F.col("a").isNull())  ·  df.filter(F.col("a").isNotNull())
df.filter(F.col("a").like("%x%"))  ·  df.filter(F.col("a").rlike("^x"))
df.filter("a > 1 AND b = 'x'")                  # SQL string form
df.limit(10)
```

---

## 7. Columns and transformations

```python
# Add / replace
df.withColumn("new", F.col("a") + F.col("b"))
df.withColumnRenamed("old", "new")

# Cast
F.col("a").cast("int")  ·  F.col("a").cast(T.DoubleType())

# Strings — the silver-layer workhorses
F.trim(c)  ·  F.ltrim(c)  ·  F.rtrim(c)
F.upper(c) ·  F.lower(c)  ·  F.initcap(c)
F.length(c) · F.concat(a, b) · F.concat_ws("-", a, b)
F.substring(c, 1, 3) · F.split(c, ",") · F.regexp_replace(c, "[^0-9.]", "")
F.regexp_extract(c, r"(\d+)", 1) · F.lpad(c, 5, "0")

# Conditional — SQL CASE WHEN
F.when(F.col("x") > 8, "High").when(F.col("x") > 5, "Mid").otherwise("Low")

# Nulls
F.coalesce(F.col("a"), F.lit(0))
F.nullif(F.col("a"), F.lit(""))
df.na.fill({"phone": "Not Available"})
df.na.drop(subset=["customer_id"])
df.na.replace({"Books": "BKS"}, subset=["code"])

# Numbers
F.abs(c) · F.round(c, 2) · F.ceil(c) · F.floor(c) · F.greatest(a,b) · F.least(a,b)

# Dates
F.current_date() · F.current_timestamp()
F.to_date(c, "dd-MM-yyyy") · F.to_timestamp(c, "yyyy-MM-dd HH:mm:ss")
F.date_format(c, "yyyyMMdd") · F.year(c) · F.month(c) · F.dayofmonth(c)
F.dayofweek(c) · F.dayofyear(c) · F.weekofyear(c) · F.hour(c)
F.date_add(c, 7) · F.datediff(a, b) · F.months_between(a, b)

# Constants and IDs
F.lit(1) · F.uuid() · F.monotonically_increasing_id() · F.spark_partition_id()

# Readable multi-line chains
df_clean = (df
    .withColumn("a", F.trim(F.col("a")))
    .withColumn("b", F.col("b").cast("double"))
    .filter(F.col("b") > 0))
```

⚠️ **Date format patterns:** `yyyy` year · **`MM` MONTH** · `dd` day · `HH` hour · **`mm` MINUTES** · `ss` seconds · `MMMM` full month · `EEEE` full day.

---

## 8. Deduplication and sorting

```python
df.distinct()
df.dropDuplicates()
df.dropDuplicates(["key"])                       # ⚠️ keeps an ARBITRARY row

# ⭐ Deterministic dedupe
w = Window.partitionBy("key").orderBy(F.col("updated_at").desc())
df.withColumn("_rn", F.row_number().over(w)).filter(F.col("_rn") == 1).drop("_rn")

df.orderBy(F.col("a").desc(), F.col("b").asc())
df.sortWithinPartitions("a")                     # narrow — no global shuffle
df.orderBy(F.col("a").desc()).limit(10)
```

---

## 9. Aggregation

```python
df.groupBy("k").count()

(df.groupBy("a", "b").agg(
    F.count("*").alias("n"),
    F.countDistinct("id").alias("distinct_ids"),
    F.sum("amt").alias("total"),
    F.round(F.avg("amt"), 2).alias("avg"),
    F.min("amt").alias("lo"),
    F.max("amt").alias("hi"),
    F.stddev("amt").alias("sd"),
    F.collect_list("x").alias("all_x"),
    F.collect_set("x").alias("unique_x"),
))

df.groupBy("a").pivot("month").sum("amt")
df.rollup("a", "b").sum("amt")
df.cube("a", "b").sum("amt")
```

---

## 10. Joins

```python
df1.join(df2, on="key", how="inner")             # ⭐ key appears ONCE
df1.join(df2, on=["k1", "k2"], how="left")       # composite key
df1.join(df2, df1.k == df2.k, "left")            # ⚠️ key appears TWICE

# Aliases — disambiguate duplicate names
(df1.alias("a").join(df2.alias("b"), on="key", how="left")
    .select(F.col("a.country").alias("ship_country"),
            F.col("b.country").alias("cust_country")))

df1.join(broadcast(df_small), "key", "left")     # ⭐ no shuffle
df1.join(df2, df1.k.eqNullSafe(df2.k), "inner")  # nulls match nulls
df1.crossJoin(df2)

# Non-equi (e.g. FX rate valid on the transaction date)
(orders.alias("o").join(rates.alias("r"),
    (F.col("o.ccy") == F.col("r.ccy")) &
    (F.col("o.dt")  >= F.col("r.valid_from")) &
    (F.col("o.dt")  <  F.col("r.valid_to")), "left"))
```

| `how=` | Keeps | Right cols? |
|--------|-------|-------------|
| `inner` | Matches only | ✅ |
| `left` | All left | ✅ nulls if no match |
| `right` | All right | ✅ |
| `full` | Everything | ✅ |
| `left_semi` | Left rows **with** a match | ❌ |
| `left_anti` | Left rows **without** a match | ❌ |
| `cross` | Every combination | ✅ |

```python
# ⭐ Referential-integrity check
orphans = fact.join(dim, "key", "left_anti")
assert orphans.count() == 0, f"{orphans.count()} orphaned keys"
```

---

## 11. Window functions

```python
w = Window.partitionBy("studio").orderBy(F.col("rating").desc())

df.withColumn("rank",       F.row_number().over(w))
df.withColumn("dense_rank", F.dense_rank().over(w))
df.withColumn("pct",        F.percent_rank().over(w))
df.withColumn("prev",       F.lag("amt", 1).over(w))
df.withColumn("next",       F.lead("amt", 1).over(w))

# Aggregate over a frame
wu = Window.partitionBy("studio")
df.withColumn("studio_avg", F.avg("rating").over(wu))

# Running total
wr = Window.partitionBy("k").orderBy("dt") \
           .rowsBetween(Window.unboundedPreceding, Window.currentRow)
df.withColumn("running_total", F.sum("amt").over(wr))
```

---

## 12. Set operations and partitioning

```python
df1.union(df2)                             # ⚠️ matches BY POSITION
df1.unionByName(df2, allowMissingColumns=True)   # ⭐ by name — safer
df1.intersect(df2)  ·  df1.exceptAll(df2)

df.repartition(6)                # round-robin · wide
df.repartition(6, "key")         # hash by key · wide
df.repartitionByRange(6, "date")
df.coalesce(3)                   # reduce only · narrow · silently ignores increases

df.cache()  ·  df.persist()  ·  df.unpersist()
```

---

## 13. SQL

```python
spark.sql("SELECT * FROM cat.schema.tbl")

# ⭐ Safe parameterisation — values must NOT be f-string interpolated
spark.sql("SELECT * FROM t WHERE studio = :s AND yr >= :y",
          args={"s": user_input, "y": 2010})

df.createOrReplaceTempView("v")
df.createOrReplaceGlobalTempView("gv")     # query as global_temp.gv
spark.catalog.dropTempView("v")
```

```sql
%sql
SELECT a, COUNT(*) AS n FROM t GROUP BY a HAVING COUNT(*) > 1 ORDER BY n DESC;

WITH step1 AS (SELECT * FROM t WHERE x > 1)
SELECT * FROM step1;

SELECT title, ROW_NUMBER() OVER (PARTITION BY studio ORDER BY rating DESC) AS rn FROM t;

SELECT * FROM csv.`/Volumes/cat/sch/vol/f.csv`;
SELECT * FROM read_files('/Volumes/.../f.csv', format => 'csv', header => true);
```

```python
dbutils.widgets.text("catalog", "ecommerce", "Target catalog")
catalog = dbutils.widgets.get("catalog")
```

---

## 14. DDL and governance

```sql
CREATE CATALOG IF NOT EXISTS ecommerce;
CREATE SCHEMA  IF NOT EXISTS ecommerce.bronze COMMENT 'Raw ingested data';
CREATE VOLUME  IF NOT EXISTS ecommerce.source_data.raw;
CREATE EXTERNAL VOLUME ecommerce.source_data.landing
  LOCATION 'abfss://ctr@acct.dfs.core.windows.net/landing/';

CREATE OR REPLACE VIEW ecommerce.gold.vw_obt AS SELECT …;
CREATE OR REPLACE MATERIALIZED VIEW ecommerce.gold.mv_obt AS SELECT …;

DROP TABLE IF EXISTS t;   UNDROP TABLE t;
DROP CATALOG IF EXISTS ecommerce CASCADE;      -- ☠️ deletes everything

ALTER TABLE t ADD COLUMNS (region STRING COMMENT 'sales region');
ALTER TABLE t ADD CONSTRAINT positive_qty CHECK (quantity > 0);
ALTER TABLE t ALTER COLUMN customer_id SET NOT NULL;
ALTER TABLE t SET TAGS ('pii' = 'true');
COMMENT ON TABLE t IS 'Grain: one row per order line.';
ALTER TABLE t ALTER COLUMN amt COMMENT 'Net amount in INR.';
```

```sql
-- ⭐ The three-grant rule
GRANT USE CATALOG ON CATALOG ecommerce            TO `data_analysts`;
GRANT USE SCHEMA  ON SCHEMA  ecommerce.gold       TO `data_analysts`;
GRANT SELECT      ON SCHEMA  ecommerce.gold       TO `data_analysts`;

GRANT MODIFY, CREATE TABLE ON SCHEMA ecommerce.silver TO `data_engineers`;
GRANT READ VOLUME  ON VOLUME ecommerce.source_data.raw TO `data_analysts`;
GRANT READ FILES, WRITE FILES ON EXTERNAL LOCATION `ext-gold` TO `data_engineers`;

SHOW GRANTS ON TABLE ecommerce.gold.t;
REVOKE SELECT ON TABLE ecommerce.gold.t FROM `data_analysts`;
ALTER TABLE ecommerce.gold.t OWNER TO `data_engineers`;
```

---

## 15. Delta operations

```sql
-- Time travel
SELECT * FROM t VERSION AS OF 5;                       -- ⚠️ BEFORE the WHERE
SELECT * FROM t TIMESTAMP AS OF '2026-08-24 10:00:00';
SELECT * FROM t@v5;
DESCRIBE HISTORY t;
RESTORE TABLE t TO VERSION AS OF 3;

-- Maintenance
OPTIMIZE t;
OPTIMIZE t ZORDER BY (dt, category);
ALTER TABLE t CLUSTER BY (dt, region);                 -- ⭐ liquid clustering
VACUUM t RETAIN 168 HOURS DRY RUN;
VACUUM t;                                              -- ⚠️ destroys time travel
ANALYZE TABLE t COMPUTE STATISTICS FOR COLUMNS a, b;

-- Change Data Feed
ALTER TABLE t SET TBLPROPERTIES (delta.enableChangeDataFeed = true);
SELECT * FROM table_changes('t', 5, 10);

-- MERGE — the idempotent upsert
MERGE INTO target AS t USING source AS s ON t.id = s.id
WHEN MATCHED THEN UPDATE SET *
WHEN NOT MATCHED THEN INSERT *;
```

```python
df1 = spark.read.format("delta").option("versionAsOf", 0).table("t")
from delta.tables import DeltaTable
DeltaTable.forName(spark, "t").merge(src.alias("s"), "t.id = s.id") \
    .whenMatchedUpdateAll().whenNotMatchedInsertAll().execute()
```

---

## 16. `dbutils`

```python
dbutils.fs.ls("/Volumes/cat/schema/vol/")
dbutils.fs.cp(src, dst)  ·  dbutils.fs.mv(src, dst)  ·  dbutils.fs.rm(path, True)
dbutils.fs.mkdirs(path)  ·  dbutils.fs.head(path, 1000)

dbutils.widgets.text("k", "default", "Label")
dbutils.widgets.dropdown("layer", "bronze", ["bronze","silver","gold"])
dbutils.widgets.get("k")  ·  dbutils.widgets.removeAll()

dbutils.secrets.get(scope="kv-scope", key="api-key")
dbutils.secrets.listScopes()

dbutils.notebook.run("./other", 600, {"param": "value"})
dbutils.notebook.exit("done")

dbutils.jobs.taskValues.set(key="rows", value=183000)
dbutils.jobs.taskValues.get(taskKey="upstream", key="rows", debugValue=0)

dbutils.help()
```

---

## 17. Auto Loader and streaming

```python
(spark.readStream.format("cloudFiles")
   .option("cloudFiles.format", "csv")
   .option("cloudFiles.schemaLocation", f"{base}/_schema/t")
   .option("rescuedDataColumn", "_rescued_data")
   .option("header", "true").schema(my_schema)
   .load(landing)
 .writeStream
   .option("checkpointLocation", f"{base}/_ckpt/t")
   .trigger(availableNow=True)                # ⭐ batch-style
   .toTable("cat.bronze.t"))
```

```python
# Windowed streaming aggregation
(spark.readStream.table("cat.bronze.t")
   .withWatermark("ts", "2 hours")
   .groupBy(F.window("ts", "1 hour"), "channel")
   .agg(F.sum("amt").alias("revenue"))
 .writeStream.outputMode("append")
   .option("checkpointLocation", ckpt)
   .toTable("cat.gold.hourly"))
```

---

## 18. System tables

```sql
SELECT * FROM ecommerce.information_schema.tables  WHERE table_schema = 'gold';
SELECT * FROM ecommerce.information_schema.columns WHERE table_name = 'gld_dim_products';

SELECT event_time, user_identity.email, action_name
FROM   system.access.audit
WHERE  event_date >= current_date() - INTERVAL 7 DAYS;

SELECT usage_date, sku_name, SUM(usage_quantity) AS dbus
FROM   system.billing.usage
WHERE  usage_date >= current_date() - INTERVAL 30 DAYS
GROUP  BY 1, 2;

SELECT job_name, period_start_time, result_state
FROM   system.lakeflow.job_run_timeline
WHERE  period_start_time >= current_date() - INTERVAL 7 DAYS;
```

---

## 19. Quality-gate patterns

```python
# Row count preserved (catches fan-out and accidental filters)
assert out.count() == src.count(), f"❌ {src.count():,} → {out.count():,}"

# Uniqueness of the grain
assert df.count() == df.select("id","seq").distinct().count(), "❌ duplicate grain"

# No nulls after parsing (catches silent to_date / cast failures)
assert df.filter(F.col("dt").isNull()).count() == 0, "❌ date parsing failed"

# Domain constraints
assert df.filter(F.col("qty") <= 0).count() == 0
assert df.filter(~F.col("ccy").isin("INR","USD","GBP","AUD")).count() == 0

# Referential integrity
assert fact.join(dim, "key", "left_anti").count() == 0, "❌ orphaned keys"

# Recomputation test — catches logic bugs a null check never would
assert df.filter(F.abs(F.col("gross") - F.col("qty")*F.col("price")) > 0.01).count() == 0

# Column order matches the file header (positional schema guard)
probe = spark.read.option("header","true").csv(path)
assert probe.columns == [f.name for f in my_schema.fields], "❌ column order mismatch"
```

---

## 20. Keyboard shortcuts & magics

| Shortcut | Action |
|----------|--------|
| `Ctrl+Enter` | Run cell |
| `Shift+Enter` | Run and move to next |
| `Ctrl+Alt+P` / `N` | Insert cell above / below |
| `Ctrl+Shift+Enter` | Run all |
| `Esc` `D` `D` | Delete cell |
| **`Ctrl+I`** | 🤖 AI assistant |
| `Ctrl+/` | Comment |
| `Ctrl+Shift+F` | Format |

| Magic | Effect |
|-------|--------|
| `%sql` | SQL cell (result → `_sqlDF`) |
| `%md` | Markdown |
| `%python` / `%scala` / `%r` | Language |
| `%sh` | Shell on the driver |
| `%pip install pkg` | Install for this session |
| `%run ./nb` | Execute another notebook inline |

---

## 21. ⚠️ The traps, in one place

| Trap | Correct form |
|------|--------------|
| `df.filter(a > 1 and b < 2)` | `df.filter((a > 1) & (b < 2))` — `&` `\|` `~`, parenthesise everything |
| `from pyspark.sql.functions import *` | `import pyspark.sql.functions as F` |
| `df.select(...)` result discarded | `df = df.select(...)` — DataFrames are **immutable** |
| `df.replace(...).show()` assigned | `.show()` returns `None` — assign first |
| `"dd-mm-yyyy"` | `"dd-MM-yyyy"` — **`MM` month, `mm` minutes** |
| `"\$"` in a regex | `"\\$"` or `r"\$"` |
| `.option("schema", s)` | `.schema(s)` — a **method** |
| Schema in the wrong column order | `StructType` matches **by position** — assert against the header |
| `df.union(df2)` with different column order | `df.unionByName(df2)` |
| `dropDuplicates(["k"])` in production | `row_number()` window + `filter(rn == 1)` |
| `coalesce(20)` on 6 partitions | Silently does nothing — use `repartition` |
| `VERSION AS OF` after `WHERE` | `FROM t VERSION AS OF 5 WHERE …` |
| `collect()` / `toPandas()` on big data | `show`, `display`, or aggregate first |
| `append` in an incremental job | `MERGE` on the business key |
| `f"… WHERE x = '{user_input}'"` | `args={"x": user_input}` — 🚨 SQL injection |
| `DoubleType` for money | `DecimalType(18, 2)` |
| Scheduled job on All-Purpose Compute | **Jobs Compute** or serverless |
| `input_file_name()` on serverless | `F.col("_metadata.file_path")` |
| Azure `Contributor` on storage | **`Storage Blob Data Contributor`** |
| `wasbs://` | `abfss://` |

---

*Back to:* **[Master index](../Databricks%20-%20Study%20Guide.md)** · **[Appendix A — Glossary](Appendix-A-glossary.md)** · **[Appendix B — Timestamp Index](Appendix-B-timestamp-index.md)**
