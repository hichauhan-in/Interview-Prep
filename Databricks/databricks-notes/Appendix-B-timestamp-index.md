# Appendix B — Transcript Timestamp Index

> Jump from any topic to the exact moment in the source video, and to the Part of this guide that expands it. Useful when you want to *see* something demonstrated rather than read about it.

**Source:** codebasics Databricks micro-course · total runtime **≈ 3 h 35 m**.

---

## 📍 Master index — chronological

| Timestamp | Topic | Guide Part |
|-----------|-------|-----------|
| `00:00:00` | Course intro — what you'll build | [1](Part-01-course-map-story-problem-statement.md) |
| `00:00:24` | Databricks Free Edition introduced | [3](Part-03-what-databricks-is-lakehouse-editions.md), [4](Part-04-lab-account-setup-ui-tour.md) |
| `00:00:34` | What Databricks is · Mercedes-Benz customer example | [3](Part-03-what-databricks-is-lakehouse-editions.md) |
| `00:01:07` | **The A2Z problem statement begins** | [1](Part-01-course-map-story-problem-statement.md) |
| `00:01:44` | Tony Sharma: *"a scalability crisis"* | [1](Part-01-course-map-story-problem-statement.md) |
| `00:01:50` | Bruce (COO): delayed reports, frozen dashboards | [1](Part-01-course-map-story-problem-statement.md) |
| `00:02:15` | Databricks proposed · **pilot, not migration** | [1](Part-01-course-map-story-problem-statement.md) |
| `00:02:38` | Peter Pandey assigned the pilot | [1](Part-01-course-map-story-problem-statement.md) |
| `00:03:09` | ⭐ **The three acceptance criteria** | [1](Part-01-course-map-story-problem-statement.md) |
| `00:03:55` | Roadmap: **scalable · agile · unified** | [1](Part-01-course-map-story-problem-statement.md) |
| `00:04:47` | The two learning paths | [1](Part-01-course-map-story-problem-statement.md) |
| `00:05:22` | 🧪 **Free Edition signup** | [4](Part-04-lab-account-setup-ui-tour.md) |
| `00:06:08` | UI tour · marketplace · catalog | [4](Part-04-lab-account-setup-ui-tour.md) |
| `00:06:21` | ⭐ **catalog → schema → table** hierarchy | [4](Part-04-lab-account-setup-ui-tour.md), [5](Part-05-unity-catalog-governance.md) |
| `00:06:40` | 🧪 Upload `movies.csv` | [4](Part-04-lab-account-setup-ui-tour.md) |
| `00:07:26` | ⚠️ **Fixing the `imdb_rating` type** to double | [4](Part-04-lab-account-setup-ui-tour.md) |
| `00:07:49` | Create table · explore its tabs | [4](Part-04-lab-account-setup-ui-tour.md) |
| `00:08:08` | **Details tab** — managed table, delta format | [6](Part-06-tables-volumes-managed-vs-external.md) |
| `00:08:19` | 🧪 First SQL query · attaching compute | [4](Part-04-lab-account-setup-ui-tour.md) |
| `00:08:45` | **Compute** — serverless only in Free Edition | [4](Part-04-lab-account-setup-ui-tour.md) |
| `00:09:20` | Serverless explained (*"like AWS Lambda"*) | [4](Part-04-lab-account-setup-ui-tour.md) |
| `00:09:36` | 🧪 **First Genie question** | [26](Part-26-genie-natural-language-analytics.md) |
| `00:10:17` | Data ingestion connectors | [4](Part-04-lab-account-setup-ui-tour.md) |
| `00:10:39` | AI section · Jobs & Pipelines · Workspace | [4](Part-04-lab-account-setup-ui-tour.md) |
| `00:11:00` | 🧪 **DataFrame basics begin** | [8](Part-08-dataframe-fundamentals.md) |
| `00:11:16` | pandas vs Spark DataFrames · PySpark | [8](Part-08-dataframe-fundamentals.md) |
| `00:11:37` | The `spark` SparkSession object | [8](Part-08-dataframe-fundamentals.md) |
| `00:12:15` | `spark.table()` → a DataFrame | [8](Part-08-dataframe-fundamentals.md) |
| `00:12:48` | `display()` vs `.show()` | [8](Part-08-dataframe-fundamentals.md) |
| `00:13:04` | `printSchema` · `count` · `columns` | [8](Part-08-dataframe-fundamentals.md) |
| `00:13:44` | Cross-checking `count()` in SQL — **37 rows** | [8](Part-08-dataframe-fundamentals.md) |
| `00:14:08` | `describe()` · `summary()` — avg 7.8, min 1.9, max 9.3 | [8](Part-08-dataframe-fundamentals.md) |
| `00:14:53` | Column filtering with `select` | [8](Part-08-dataframe-fundamentals.md) |
| `00:15:26` | ⭐ **DataFrame immutability** | [8](Part-08-dataframe-fundamentals.md) |
| `00:16:05` | Row filtering · **`Ctrl+I` AI assist** | [8](Part-08-dataframe-fundamentals.md) |
| `00:16:44` | Three filter syntaxes · `between` | [8](Part-08-dataframe-fundamentals.md) |
| `00:18:03` | Marvel filter · `distinct` on industry & language | [8](Part-08-dataframe-fundamentals.md) |
| `00:19:09` | `withColumn` — creating `profit` | [8](Part-08-dataframe-fundamentals.md) |
| `00:20:28` | `withColumnRenamed` | [8](Part-08-dataframe-fundamentals.md) |
| `00:21:08` | 🧪 **Read/write CSV begins** · `orders.csv` | [9](Part-09-reading-writing-data.md) |
| `00:21:52` | 🧪 **Creating a volume** · uploading files | [6](Part-06-tables-volumes-managed-vs-external.md), [9](Part-09-reading-writing-data.md) |
| `00:23:03` | `spark.read.csv` — header appears as data | [9](Part-09-reading-writing-data.md) |
| `00:23:38` | `.option("header","true")` | [9](Part-09-reading-writing-data.md) |
| `00:23:52` | Everything is a string → `inferSchema` | [9](Part-09-reading-writing-data.md) |
| `00:25:06` | ⭐ **Explicit `StructType` schema** · `.schema()` is a *method* | [9](Part-09-reading-writing-data.md) |
| `00:25:59` | `dateFormat` option | [9](Part-09-reading-writing-data.md) |
| `00:26:42` | `spark.read.format(...).load(...)` | [9](Part-09-reading-writing-data.md) |
| `00:27:05` | Writing Parquet · `mode("overwrite")` | [9](Part-09-reading-writing-data.md) |
| `00:27:57` | 🎓 **DISTRIBUTED COMPUTING begins** | [2](Part-02-distributed-computing-hadoop-spark.md) |
| `00:28:24` | ⭐ **The 2,000-puris wedding analogy** | [2](Part-02-distributed-computing-hadoop-spark.md) |
| `00:29:02` | The ticker/EPS/P-E example · 40M records | [2](Part-02-distributed-computing-hadoop-spark.md) |
| `00:29:37` | ⭐ **Map and reduce** | [2](Part-02-distributed-computing-hadoop-spark.md) |
| `00:29:45` | Driver + worker = **cluster** | [2](Part-02-distributed-computing-hadoop-spark.md), [13](Part-13-spark-architecture-execution-model.md) |
| `00:30:13` | **Fault tolerance** | [2](Part-02-distributed-computing-hadoop-spark.md) |
| `00:30:41` | ⭐ **The caterer analogy** = Spark abstraction | [2](Part-02-distributed-computing-hadoop-spark.md) |
| `00:31:07` | Formal definition of distributed computing | [2](Part-02-distributed-computing-hadoop-spark.md) |
| `00:31:21` | **Hadoop** — disk-heavy and slow | [2](Part-02-distributed-computing-hadoop-spark.md) |
| `00:31:38` | ⭐ **Spark's origin** — Matei Zaharia, UC Berkeley AMPLab | [2](Part-02-distributed-computing-hadoop-spark.md) |
| `00:32:11` | PySpark · what Spark hides from you | [2](Part-02-distributed-computing-hadoop-spark.md) |
| `00:33:07` | *"abstracts the complexity of parallelism"* | [2](Part-02-distributed-computing-hadoop-spark.md) |
| `00:33:27` | ⭐ **Self-hosted vs managed** · the event-planner analogy | [2](Part-02-distributed-computing-hadoop-spark.md), [3](Part-03-what-databricks-is-lakehouse-editions.md) |
| `00:34:53` | 🧪 **SQL in Spark begins** | [10](Part-10-sql-in-spark.md) |
| `00:35:11` | `spark.sql()` — the Python API | [10](Part-10-sql-in-spark.md) |
| `00:36:02` | ⚠️ The quoting error, live | [10](Part-10-sql-in-spark.md) |
| `00:36:29` | `%sql` magic · `_sqlDF` | [10](Part-10-sql-in-spark.md) |
| `00:37:17` | 🧪 `createDataFrame` — the weather data | [10](Part-10-sql-in-spark.md) |
| `00:37:53` | **`createOrReplaceTempView`** | [10](Part-10-sql-in-spark.md) |
| `00:38:53` | ⭐ **Global temp view** · session vs cluster scope | [10](Part-10-sql-in-spark.md) |
| `00:40:04` | 🧪 **JOINS begin** | [11](Part-11-joins-in-spark.md) |
| `00:40:44` | ⭐ Fixture data with **deliberate inconsistencies** | [11](Part-11-joins-in-spark.md) |
| `00:41:19` | Inner join — **6 rows** | [11](Part-11-joins-in-spark.md) |
| `00:42:06` | ⭐ **Nulls don't match nulls** | [11](Part-11-joins-in-spark.md) |
| `00:42:37` | Left join — **8 rows** | [11](Part-11-joins-in-spark.md) |
| `00:43:23` | **Aliases** · disambiguating `country` | [11](Part-11-joins-in-spark.md) |
| `00:46:15` | Full outer join — **10 rows** | [11](Part-11-joins-in-spark.md) |
| `00:47:44` | **Left semi** — 6 rows | [11](Part-11-joins-in-spark.md) |
| `00:48:31` | **Left anti** — 2 rows | [11](Part-11-joins-in-spark.md) |
| `00:49:05` | **Composite key join** — 6 rows | [11](Part-11-joins-in-spark.md) |
| `00:49:49` | 🎓 **SPARK INTERNALS begin** — *"very important for interviews"* | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `00:50:23` | `.explain()` introduced | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `00:51:29` | `explain("extended")` — all four plans | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `00:52:12` | ⭐ **The full plan pipeline** explained | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `00:54:18` | **AQE** introduced | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `00:54:36` | **Catalyst optimizer** | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `00:54:57` | **Photon** — vectorised C++ engine | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `00:55:29` | ⭐ **The Google Maps analogy** | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `00:55:53` | Reading the parsed & analyzed plans · *"project means select"* | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `00:57:04` | ❓ **The quiz: why four columns, not three?** | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `00:57:46` | Optimized plan · filter pushdown · added `isnotnull` | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `00:58:50` | Physical plan · **read bottom-to-top** | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `01:00:00` | 💡 *"Talk to ChatGPT if you don't understand the plan"* | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `01:00:18` | `explain("formatted")` | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `01:01:07` | **`ColumnarToRow`** explained | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| `01:02:41` | 🎓 **SPARK ARCHITECTURE** | [13](Part-13-spark-architecture-execution-model.md) |
| `01:03:25` | The **driver** node | [13](Part-13-spark-architecture-execution-model.md) |
| `01:03:55` | Partitions — 100 rows → 40/40/20 | [13](Part-13-spark-architecture-execution-model.md) |
| `01:04:23` | Executors · tasks · the 15+10+3 = **28** example | [13](Part-13-spark-architecture-execution-model.md) |
| `01:06:29` | ⭐ **Cluster manager vs driver** responsibilities | [13](Part-13-spark-architecture-execution-model.md) |
| `01:07:36` | 💡 *"A very commonly asked interview question — draw it on a whiteboard"* | [13](Part-13-spark-architecture-execution-model.md) |
| `01:07:47` | 🎓 **TRANSFORMATIONS vs ACTIONS** | [14](Part-14-transformations-actions-lazy-evaluation.md) |
| `01:08:25` | **Lazy evaluation** explained | [14](Part-14-transformations-actions-lazy-evaluation.md) |
| `01:09:19` | Transformation and action examples | [14](Part-14-transformations-actions-lazy-evaluation.md) |
| `01:09:58` | ⭐ **The recipe vs cooking analogy** | [14](Part-14-transformations-actions-lazy-evaluation.md) |
| `01:10:54` | Benefit 1 — query optimisation | [14](Part-14-transformations-actions-lazy-evaluation.md) |
| `01:12:24` | Benefit 2 — avoid unnecessary work | [14](Part-14-transformations-actions-lazy-evaluation.md) |
| `01:12:55` | Benefit 3 — memory efficiency (1.7 GB vs a pipeline) | [14](Part-14-transformations-actions-lazy-evaluation.md) |
| `01:13:38` | Benefit 4 — fault tolerance · **the half-cooked biryani** | [14](Part-14-transformations-actions-lazy-evaluation.md) |
| `01:16:10` | 🎓 **NARROW vs WIDE transformations** | [15](Part-15-narrow-wide-transformations-shuffle.md) |
| `01:17:17` | Wide — `groupBy` needs data from all partitions | [15](Part-15-narrow-wide-transformations-shuffle.md) |
| `01:18:36` | Formal definitions · shuffle & exchange | [15](Part-15-narrow-wide-transformations-shuffle.md) |
| `01:19:20` | ⚠️ The serverless caveat about forcing a shuffle | [15](Part-15-narrow-wide-transformations-shuffle.md) |
| `01:20:04` | 🎓 **PARTITIONS & PARALLELISM** | [16](Part-16-partitions-repartition-coalesce.md) |
| `01:20:46` | ⭐ **The 2,000-chefs problem** | [16](Part-16-partitions-repartition-coalesce.md) |
| `01:22:21` | ❓ *"Where is my DataFrame?"* | [16](Part-16-partitions-repartition-coalesce.md) |
| `01:23:56` | Data movement cost during `groupBy` | [16](Part-16-partitions-repartition-coalesce.md) |
| `01:25:39` | **`repartition(6, "studio")`** — hash partitioning | [16](Part-16-partitions-repartition-coalesce.md) |
| `01:28:47` | ⭐ *"This is where the skill of data engineering comes in"* | [16](Part-16-partitions-repartition-coalesce.md) |
| `01:29:45` | 🧪 The repartition lab · 22 studios, 37 rows | [16](Part-16-partitions-repartition-coalesce.md) |
| `01:31:43` | Round-robin vs hash partitioning in the plan | [16](Part-16-partitions-repartition-coalesce.md) |
| `01:32:21` | 🧪 **Proving it — 6 partitions → 6 Parquet files** | [16](Part-16-partitions-repartition-coalesce.md) |
| `01:34:04` | 🎓 **`coalesce`** | [16](Part-16-partitions-repartition-coalesce.md) |
| `01:35:04` | 7 → 3 · 7 → 2 · 7 → 6 · **7 → 10 does nothing** | [16](Part-16-partitions-repartition-coalesce.md) |
| `01:37:53` | Use cases — task overhead · file output · data movement | [16](Part-16-partitions-repartition-coalesce.md) |
| `01:39:45` | 🎓 **MANAGED vs EXTERNAL tables** | [6](Part-06-tables-volumes-managed-vs-external.md) |
| `01:40:15` | ⭐ **Data + metadata** · `DESCRIBE EXTENDED` | [6](Part-06-tables-volumes-managed-vs-external.md) |
| `01:41:12` | External table — data in S3, metadata in Databricks | [6](Part-06-tables-volumes-managed-vs-external.md) |
| `01:42:50` | ⭐ When to use each | [6](Part-06-tables-volumes-managed-vs-external.md) |
| `01:43:34` | 🎓 **MEDALLION ARCHITECTURE** | [17](Part-17-medallion-architecture.md) |
| `01:43:52` | ⭐ *"Lakehouse = data lake + data warehouse"* | [3](Part-03-what-databricks-is-lakehouse-editions.md), [17](Part-17-medallion-architecture.md) |
| `01:44:40` | The bronze/silver/gold architecture diagram | [17](Part-17-medallion-architecture.md) |
| `01:45:38` | 🧪 **The website-analytics worked example** | [17](Part-17-medallion-architecture.md) |
| `01:46:54` | Gold — the average-duration KPI | [17](Part-17-medallion-architecture.md) |
| `01:48:33` | 💡 **Bloomberg's "product 0…n" layers** | [17](Part-17-medallion-architecture.md) |
| `01:49:26` | 🎓 **UNITY CATALOG** | [5](Part-05-unity-catalog-governance.md) |
| `01:50:03` | Groups · role-based privileging | [5](Part-05-unity-catalog-governance.md) |
| `01:51:51` | Catalog naming strategies | [5](Part-05-unity-catalog-governance.md) |
| `01:53:02` | ⭐ **The silos problem** before Unity Catalog | [5](Part-05-unity-catalog-governance.md) |
| `01:54:36` | 🧪 Creating groups · granting permissions | [5](Part-05-unity-catalog-governance.md) |
| `01:57:04` | The three headline benefits | [5](Part-05-unity-catalog-governance.md) |
| `01:57:13` | ⭐ **Data lineage — the "spy" story** | [5](Part-05-unity-catalog-governance.md) |
| `01:59:01` | Data discovery · tags · search | [5](Part-05-unity-catalog-governance.md) |
| `01:59:31` | 🎓 **ACID** | [7](Part-07-delta-lake-acid-time-travel.md) |
| `01:59:44` | ⭐ **The bank-transfer example** | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:00:42` | **Atomicity** · rollback | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:01:17` | **Consistency** · constraints | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:02:16` | **Isolation** · the joint-account example | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:03:15` | **Durability** | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:04:10` | 🎓 **TIME TRAVEL** | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:04:24` | ⭐ SEBI / SEC / FINRA / GDPR — the regulatory driver | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:05:55` | **Copy-on-write** versioning | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:06:53` | **Transaction-log** versioning · the Git analogy | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:07:39` | Benefits — audit · error recovery · historical analysis | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:08:26` | `VERSION AS OF` · `TIMESTAMP AS OF` · `RESTORE` | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:09:15` | 🧪 **AWS account setup** *(optional)* | [6](Part-06-tables-volumes-managed-vs-external.md) |
| `02:11:49` | 🎓 **DELTA LAKE / DELTA FORMAT** | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:12:00` | 🧪 Creating an S3 bucket · globally unique names | [6](Part-06-tables-volumes-managed-vs-external.md) |
| `02:13:20` | 🧪 **External location via AWS Quickstart** | [6](Part-06-tables-volumes-managed-vs-external.md) |
| `02:14:37` | `SELECT * FROM csv.\`s3://...\`` | [6](Part-06-tables-volumes-managed-vs-external.md) |
| `02:14:47` | Delta = a storage layer **on top of** the data lake | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:15:50` | 🧪 `CREATE TABLE … USING DELTA LOCATION` | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:17:00` | ⚠️ **`input_file_name()` unsupported on serverless** | [6](Part-06-tables-volumes-managed-vs-external.md) |
| `02:18:09` | 🧪 `UPDATE` the Delta table · **the CSV is untouched** | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:19:36` | 🧪 History · `VERSION AS OF 0` vs `1` | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:20:26` | ⚠️ *"`VERSION AS OF` goes before the `WHERE`"* | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:20:46` | 🧪 **`_delta_log` and Parquet files on disk** | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:21:01` | ⭐ The data lake's three problems | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:22:50` | ⭐ **Delta = Parquet + transaction logs + metadata** | [7](Part-07-delta-lake-acid-time-travel.md) |
| `02:23:43` | The raw → bronze → silver → gold architecture | [17](Part-17-medallion-architecture.md) |
| `02:25:19` | 🏗️ **THE PROJECT BEGINS** | [19](Part-19-legacy-vs-databricks-architecture.md) |
| `02:25:33` | The legacy architecture diagram | [19](Part-19-legacy-vs-databricks-architecture.md) |
| `02:26:53` | ⭐ **The three problems** with the legacy stack | [19](Part-19-legacy-vs-databricks-architecture.md) |
| `02:27:55` | The new Databricks architecture | [19](Part-19-legacy-vs-databricks-architecture.md) |
| `02:28:34` | *"This is a data lakehouse architecture"* | [19](Part-19-legacy-vs-databricks-architecture.md) |
| `02:29:07` | 🧪 **LAB 1 — creating the catalog** | [20](Part-20-lab-environment-setup.md) |
| `02:29:26` | Workspace folders · Git folders | [20](Part-20-lab-environment-setup.md) |
| `02:30:13` | `CREATE CATALOG IF NOT EXISTS ecommerce` | [20](Part-20-lab-environment-setup.md) |
| `02:31:00` | `USE CATALOG` · bronze/silver/gold schemas | [20](Part-20-lab-environment-setup.md) |
| `02:31:47` | ☠️ `DROP CATALOG … CASCADE` | [20](Part-20-lab-environment-setup.md) |
| `02:32:02` | 🧪 **Uploading the source data** | [20](Part-20-lab-environment-setup.md) |
| `02:32:33` | ⭐ **The six source datasets, field by field** | [18](Part-18-project-blueprint-data-model.md) |
| `02:34:16` | `order_items/landing/` — ~92 daily files | [18](Part-18-project-blueprint-data-model.md) |
| `02:34:37` | 🧪 Creating `source_data` schema and the `raw` volume | [20](Part-20-lab-environment-setup.md) |
| `02:36:00` | ⭐ **Why `raw` and not straight to bronze** | [17](Part-17-medallion-architecture.md), [20](Part-20-lab-environment-setup.md) |
| `02:36:31` | 🧪 **LAB 2 — dimensions to bronze** | [21](Part-21-lab-dimensions-bronze.md) |
| `02:37:10` | Imports · catalog name · brand schema | [21](Part-21-lab-dimensions-bronze.md) |
| `02:38:12` | **Audit columns** — `source_file`, `ingested_at` | [21](Part-21-lab-dimensions-bronze.md) |
| `02:38:39` | `write.format("delta")` · `mergeSchema` · `saveAsTable` | [21](Part-21-lab-dimensions-bronze.md) |
| `02:39:29` | *"Repeat for the other dimensions"* | [21](Part-21-lab-dimensions-bronze.md) |
| `02:39:55` | ⭐ **Why `date` stays a string in bronze** | [21](Part-21-lab-dimensions-bronze.md) |
| `02:40:08` | 🧪 **LAB 3 — dimensions to silver** | [22](Part-22-lab-dimensions-silver.md) |
| `02:40:53` | `trim` — the trailing space in `brand_name` | [22](Part-22-lab-dimensions-silver.md) |
| `02:41:49` | ⭐ **Regex** · `[^A-Za-z0-9]` · regex101.com | [22](Part-22-lab-dimensions-silver.md) |
| `02:42:58` | ⭐ **`distinct` finds `Books` vs `BKS`** | [22](Part-22-lab-dimensions-silver.md) |
| `02:43:37` | `df.replace(anomalies, subset=...)` | [22](Part-22-lab-dimensions-silver.md) |
| `02:44:07` | ⚠️ The `.show()`-returns-`None` bug, live | [22](Part-22-lab-dimensions-silver.md) |
| `02:44:45` | **Categories** — finding and dropping duplicates | [22](Part-22-lab-dimensions-silver.md) |
| `02:45:31` | `F.upper` on category codes | [22](Part-22-lab-dimensions-silver.md) |
| `02:45:56` | **Products** — 50,000 rows, 14 columns | [22](Part-22-lab-dimensions-silver.md) |
| `02:46:16` | `weight_in_grams` — stripping the `g` | [22](Part-22-lab-dimensions-silver.md) |
| `02:46:47` | `length` — the comma decimal separator | [22](Part-22-lab-dimensions-silver.md) |
| `02:47:07` | Uppercasing the foreign keys | [22](Part-22-lab-dimensions-silver.md) |
| `02:47:21` | **Material spelling fixes** with `F.when` | [22](Part-22-lab-dimensions-silver.md) |
| `02:48:03` | `rating_count` — `abs` and null → 0 | [22](Part-22-lab-dimensions-silver.md) |
| `02:48:48` | **Customers** — 300 null `customer_id` | [22](Part-22-lab-dimensions-silver.md) |
| `02:49:36` | ⭐ **Null phone → "Not Available"** | [22](Part-22-lab-dimensions-silver.md) |
| `02:49:57` | **Calendar** — `to_date` with `dd-MM-yyyy` | [22](Part-22-lab-dimensions-silver.md) |
| `02:50:22` | Duplicate dates · `initcap` day names · `abs` weeks | [22](Part-22-lab-dimensions-silver.md) |
| `02:51:19` | **Quarter → `Q3 2025`** enrichment | [22](Part-22-lab-dimensions-silver.md) |
| `02:52:20` | 🧪 **LAB 4 — dimensions to gold** | [23](Part-23-lab-dimensions-gold.md) |
| `02:52:37` | ⭐ The denormalisation decision | [23](Part-23-lab-dimensions-gold.md) |
| `02:53:42` | **Temp views** for the join | [23](Part-23-lab-dimensions-gold.md) |
| `02:54:23` | ⭐ **The CTE join** — brands + categories → products | [23](Part-23-lab-dimensions-gold.md) |
| `02:55:53` | **Customers** — the region requirement | [23](Part-23-lab-dimensions-gold.md) |
| `02:56:56` | The nested `country_state_map` dictionary | [23](Part-23-lab-dimensions-gold.md) |
| `02:57:18` | Flat list → `createDataFrame` → join | [23](Part-23-lab-dimensions-gold.md) |
| `02:58:01` | Unmatched → `"Other"` region | [23](Part-23-lab-dimensions-gold.md) |
| `02:58:53` | **Date dimension** — `is_weekend` | [23](Part-23-lab-dimensions-gold.md) |
| `02:59:28` | `date_id` · `month_name` | [23](Part-23-lab-dimensions-gold.md) |
| `03:00:07` | Column reordering | [23](Part-23-lab-dimensions-gold.md) |
| `03:01:01` | 🧪 **LAB 5 — fact table to bronze** | [24](Part-24-lab-fact-bronze-silver.md) |
| `03:02:08` | ⭐ **All-string schema** — *"there is some data quality issue"* | [24](Part-24-lab-fact-bronze-silver.md) |
| `03:02:44` | **183,000 records** from ~92 CSVs | [24](Part-24-lab-fact-bronze-silver.md) |
| `03:03:31` | ⚠️ **The data-quality issues spotted** | [24](Part-24-lab-fact-bronze-silver.md) |
| `03:04:19` | Preview of gold — gross, discount, sales amount | [25](Part-25-lab-fact-gold-reporting-view.md) |
| `03:06:08` | `web` → `Website`, `app` → `Mobile App` | [24](Part-24-lab-fact-bronze-silver.md) |
| `03:06:35` | 🧪 **Fact to silver** | [24](Part-24-lab-fact-bronze-silver.md) |
| `03:06:49` | `"two"` → `2` with `when/otherwise` | [24](Part-24-lab-fact-bronze-silver.md) |
| `03:07:03` | Stripping `$` from `unit_price` | [24](Part-24-lab-fact-bronze-silver.md) |
| `03:07:29` | Stripping `%` from the discount | [24](Part-24-lab-fact-bronze-silver.md) |
| `03:08:44` | `to_date` · `to_timestamp` · `item_seq` cast | [24](Part-24-lab-fact-bronze-silver.md) |
| `03:09:36` | `processed_at` · verify the schema · write to silver | [24](Part-24-lab-fact-bronze-silver.md) |
| `03:10:17` | 🧪 **LAB 6 — fact to gold** | [25](Part-25-lab-fact-gold-reporting-view.md) |
| `03:10:44` | ⭐ **Two cans of beans** — `gross_amount` | [25](Part-25-lab-fact-gold-reporting-view.md) |
| `03:11:09` | `discount_amount` · `sales_amount` · *"don't forget government"* | [25](Part-25-lab-fact-gold-reporting-view.md) |
| `03:11:26` | `date_id` · `coupon_flag` | [25](Part-25-lab-fact-gold-reporting-view.md) |
| `03:11:52` | ⭐ **Currency normalisation** · USD = 88.29 | [25](Part-25-lab-fact-gold-reporting-view.md) |
| `03:12:29` | 💡 *"In a real project you'd use a currency API"* | [25](Part-25-lab-fact-gold-reporting-view.md) |
| `03:13:26` | Renaming for the business | [25](Part-25-lab-fact-gold-reporting-view.md) |
| `03:14:15` | Spot check — **183,000 rows** | [25](Part-25-lab-fact-gold-reporting-view.md) |
| `03:14:38` | 🧞 **GENIE** | [26](Part-26-genie-natural-language-analytics.md) |
| `03:14:53` | Catalog-level AI suggestions | [26](Part-26-genie-natural-language-analytics.md) |
| `03:15:20` | 🧪 Creating a Genie space · ⭐ **"always use gold"** | [26](Part-26-genie-natural-language-analytics.md) |
| `03:15:50` | Instructions to the LLM · settings | [26](Part-26-genie-natural-language-analytics.md) |
| `03:16:14` | Q1 — transactions in USD · verifying the SQL | [26](Part-26-genie-natural-language-analytics.md) |
| `03:16:48` | Q2 — monthly revenue trend · changing the chart type | [26](Part-26-genie-natural-language-analytics.md) |
| `03:17:26` | Q3/Q4 — average revenue per region, refined | [26](Part-26-genie-natural-language-analytics.md) |
| `03:18:16` | 🎓 **Analysing the dashboard's needs** | [25](Part-25-lab-fact-gold-reporting-view.md) |
| `03:18:58` | ⭐ **The One Big Table pattern** | [25](Part-25-lab-fact-gold-reporting-view.md) |
| `03:19:23` | 🧪 `CREATE OR REPLACE VIEW` | [25](Part-25-lab-fact-gold-reporting-view.md) |
| `03:20:33` | ⚠️ *"I don't think we have region — we didn't join customers"* | [25](Part-25-lab-fact-gold-reporting-view.md) |
| `03:21:19` | 📊 **DASHBOARDS** | [27](Part-27-ai-bi-dashboards.md) |
| `03:21:31` | Pages · filters | [27](Part-27-ai-bi-dashboards.md) |
| `03:21:54` | Adding the title text box | [27](Part-27-ai-bi-dashboards.md) |
| `03:22:25` | Adding the data source | [27](Part-27-ai-bi-dashboards.md) |
| `03:22:51` | AI-suggested monthly sales trend | [27](Part-27-ai-bi-dashboards.md) |
| `03:23:31` | Building the same chart manually | [27](Part-27-ai-bi-dashboards.md) |
| `03:24:04` | ⚠️⚠️ **The alphabetical-month trap, live** | [27](Part-27-ai-bi-dashboards.md) |
| `03:24:29` | The fix — transaction date with monthly granularity | [27](Part-27-ai-bi-dashboards.md) |
| `03:24:43` | Titles · axis labels · annotations | [27](Part-27-ai-bi-dashboards.md) |
| `03:25:34` | Net amount by category — Electronics 1.38 B | [27](Part-27-ai-bi-dashboards.md) |
| `03:26:15` | 🔥 **The hour × day heatmap** | [27](Part-27-ai-bi-dashboards.md) |
| `03:27:26` | **Filters** — Electronics vs Books | [27](Part-27-ai-bi-dashboards.md) |
| `03:28:29` | ⏰ **ORCHESTRATION** | [28](Part-28-orchestration-jobs-workflows.md) |
| `03:28:32` | ⭐ **Historical backfill vs daily incremental** | [28](Part-28-orchestration-jobs-workflows.md) |
| `03:29:29` | Creating a job · notebook task · compute | [28](Part-28-orchestration-jobs-workflows.md) |
| `03:30:44` | Chaining tasks · dependencies · branching | [28](Part-28-orchestration-jobs-workflows.md) |
| `03:31:40` | ⭐ **The parent-job pattern** | [28](Part-28-orchestration-jobs-workflows.md) |
| `03:31:57` | Schedules & triggers · file arrival · cron | [28](Part-28-orchestration-jobs-workflows.md) |
| `03:32:34` | 💡 **The Bloomberg market-close buffer** | [28](Part-28-orchestration-jobs-workflows.md) |
| `03:33:35` | Manual `Run now` vs a trigger | [28](Part-28-orchestration-jobs-workflows.md) |
| `03:33:41` | ⭐ **Peter's closing report to Tony** | [1](Part-01-course-map-story-problem-statement.md), [32](Part-32-behavioral-and-closing.md) |
| `03:35:03` | Course wrap-up · certification | [32](Part-32-behavioral-and-closing.md) |

---

## 🔍 Reverse index — find a topic fast

| I want to see… | Timestamp |
|----------------|-----------|
| The problem statement | `00:01:07` |
| Signing up | `00:05:22` |
| Uploading a CSV and fixing a type | `00:06:40` |
| First SQL query | `00:08:19` |
| Genie for the first time | `00:09:36` |
| DataFrame basics | `00:11:00` |
| Reading CSVs with options | `00:23:03` |
| Explicit `StructType` schemas | `00:25:06` |
| **The puris analogy** | `00:28:24` |
| Map-reduce | `00:29:37` |
| Spark's origin | `00:31:38` |
| Self-hosted vs managed | `00:33:27` |
| `%sql` and temp views | `00:36:29` |
| All the join types | `00:40:04` |
| **`.explain()` and query plans** | `00:50:23` |
| **Catalyst vs Photon (Google Maps)** | `00:55:29` |
| **Spark architecture (whiteboard)** | `01:02:41` |
| Lazy evaluation | `01:08:25` |
| Narrow vs wide | `01:16:10` |
| **`repartition`** | `01:25:39` |
| **`coalesce`** | `01:34:04` |
| Managed vs external tables | `01:39:45` |
| **Medallion architecture** | `01:43:34` |
| Unity Catalog | `01:49:26` |
| **ACID (bank transfer)** | `01:59:44` |
| **Time travel** | `02:04:10` |
| Delta Lake defined | `02:22:50` |
| **The project architecture** | `02:25:19` |
| Creating the catalog | `02:29:07` |
| The source datasets | `02:32:33` |
| Bronze ingestion | `02:36:31` |
| **Silver cleaning** | `02:40:08` |
| **The CTE join** | `02:54:23` |
| Region mapping | `02:55:53` |
| Fact ingestion (183k rows) | `03:01:01` |
| Fact cleaning | `03:06:35` |
| **Business measures** | `03:10:44` |
| Currency normalisation | `03:11:52` |
| **The OBT view** | `03:18:58` |
| Building a dashboard | `03:21:19` |
| **The month-order bug** | `03:24:04` |
| The heatmap | `03:26:15` |
| **Jobs and scheduling** | `03:28:29` |
| The closing report | `03:33:41` |

---

*Back to:* **[Master index](../Databricks%20-%20Study%20Guide.md)** · *Next:* **[Appendix C — PySpark & SQL Cheat Sheet](Appendix-C-pyspark-sql-cheatsheet.md)**
