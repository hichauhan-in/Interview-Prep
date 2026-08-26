# Appendix A — Master Glossary

> Every term used in this guide, A–Z, in one line. The **Part** column points to where it's explained properly.

---

## A

| Term | Meaning | Part |
|------|---------|------|
| **ACID** | Atomicity, Consistency, Isolation, Durability — the four transaction guarantees. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Action** | An operation that triggers execution and returns a value or writes data (`count`, `show`, `write`). | [14](Part-14-transformations-actions-lazy-evaluation.md) |
| **Access Connector** | ☁️ An Azure managed identity that lets Databricks reach ADLS Gen2 without any secret. | [29](Part-29-azure-databricks-deep-dive.md) |
| **ADLS Gen2** | ☁️ Azure Data Lake Storage Gen2 = Blob storage **+ hierarchical namespace**. | [29](Part-29-azure-databricks-deep-dive.md) |
| **AQE** | Adaptive Query Execution — re-optimises a plan mid-flight using real runtime statistics. | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| **Asset Bundle** | Databricks jobs and resources defined as YAML in Git, deployable per environment. | [28](Part-28-orchestration-jobs-workflows.md) |
| **Atomicity** | All steps of a transaction succeed, or none do. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Auto Loader** | Incremental file ingestion (`format("cloudFiles")`) that tracks processed files in a checkpoint. | [30](Part-30-miscellaneous-deeper-topics.md) |
| **`abfss://`** | ☁️ The ADLS Gen2 URI scheme: `abfss://container@account.dfs.core.windows.net/path`. | [6](Part-06-tables-volumes-managed-vs-external.md) |

## B

| Term | Meaning | Part |
|------|---------|------|
| **Backfill** | Loading historical data once, typically with `overwrite`. | [28](Part-28-orchestration-jobs-workflows.md) |
| **Broadcast join** | The small side of a join is copied to every executor — **no shuffle**. The biggest join optimisation. | [11](Part-11-joins-in-spark.md) |
| **Bronze** | The medallion layer holding raw, as-received data in Delta, usually all strings. | [17](Part-17-medallion-architecture.md) |
| **Bucketing** | Persisting a hash-partitioned layout in table metadata so future joins can skip the shuffle. | [16](Part-16-partitions-repartition-coalesce.md) |

## C

| Term | Meaning | Part |
|------|---------|------|
| **Catalog** | Top level of the `catalog.schema.object` namespace. | [5](Part-05-unity-catalog-governance.md) |
| **Catalyst** | Spark's query optimiser. Produces a better plan; **does not execute**. | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| **CDF (Change Data Feed)** | Emits row-level inserts/updates/deletes from a Delta table for downstream propagation. | [30](Part-30-miscellaneous-deeper-topics.md) |
| **Checkpoint (Delta)** | A Parquet summary of table state written every ~10 commits so readers don't replay every JSON. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Checkpoint (Streaming)** | Durable progress and state for a streaming query or Auto Loader. | [30](Part-30-miscellaneous-deeper-topics.md) |
| **Cluster** | A driver plus executors acting as one system. | [2](Part-02-distributed-computing-hadoop-spark.md) |
| **Cluster manager** | Owns **node** lifecycle — provisioning, replacement, autoscaling. (The driver owns tasks.) | [13](Part-13-spark-architecture-execution-model.md) |
| **Cluster policy** | An admin template constraining what clusters users may create. Cost guardrail. | [29](Part-29-azure-databricks-deep-dive.md) |
| **`coalesce` (partitions)** | Reduces partition count by merging locally. **Cannot increase.** Narrow. | [16](Part-16-partitions-repartition-coalesce.md) |
| **`coalesce` (function)** | Returns the first non-null argument. | [8](Part-08-dataframe-fundamentals.md) |
| **Column pruning** | Reading only the columns a query needs. Visible as `ReadSchema`. | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| **Composite key** | Two or more columns needed together for uniqueness (e.g. `order_id` + `item_seq`). | [18](Part-18-project-blueprint-data-model.md) |
| **Conditional Access** | ☁️ Entra ID policy (MFA, device compliance, location) applied to Databricks sign-in. | [29](Part-29-azure-databricks-deep-dive.md) |
| **Conformed dimension** | A dimension that means the same thing everywhere it's used. | [17](Part-17-medallion-architecture.md) |
| **Consistency** | Transactions move the database between valid states, honouring constraints. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Control plane** | Databricks-managed: UI, notebook source, job definitions, Unity Catalog metadata. | [3](Part-03-what-databricks-is-lakehouse-editions.md) |
| **Copy-on-write** | Versioning by inserting a new row per change (≡ SCD Type 2). | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Cron** | Schedule expression, e.g. `0 0 23 * * ?` = daily at 23:00. | [28](Part-28-orchestration-jobs-workflows.md) |
| **CTE** | Common Table Expression — a named step defined with `WITH`. Inlined; not a temp table. | [23](Part-23-lab-dimensions-gold.md) |

## D

| Term | Meaning | Part |
|------|---------|------|
| **DAG** | Directed Acyclic Graph — the dependency graph of stages or job tasks. | [28](Part-28-orchestration-jobs-workflows.md) |
| **DataFrame** | A distributed, immutable, lazily-evaluated collection of rows with a schema. | [8](Part-08-dataframe-fundamentals.md) |
| **Data lake** | Cheap object storage accepting any format. No ACID, no schema enforcement. | [3](Part-03-what-databricks-is-lakehouse-editions.md) |
| **Data mesh** | Organisational model where domain teams own their data as products. | [17](Part-17-medallion-architecture.md) |
| **Data plane / compute plane** | Where your code actually runs — your VNet (classic) or Databricks-managed (serverless). | [3](Part-03-what-databricks-is-lakehouse-editions.md) |
| **Data skipping** | Using file statistics to avoid reading files that can't match a predicate. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Data swamp** | A data lake nobody can use — the failure mode Delta and governance prevent. | [3](Part-03-what-databricks-is-lakehouse-editions.md) |
| **Data warehouse** | Structured, schema-on-write analytical database. Fast SQL, poor for ML/unstructured. | [3](Part-03-what-databricks-is-lakehouse-editions.md) |
| **DBFS** | Legacy Databricks File System. **Superseded by Unity Catalog volumes.** | [6](Part-06-tables-volumes-managed-vs-external.md) |
| **DBU** | Databricks Unit — normalised processing capacity per hour. The "kWh" of Databricks. | [3](Part-03-what-databricks-is-lakehouse-editions.md) |
| **DBR** | Databricks Runtime — the bundled Spark + Delta + library stack. **LTS** = long-term support. | [4](Part-04-lab-account-setup-ui-tour.md) |
| **Delta Lake** | Open storage layer = **Parquet + transaction log + metadata**, giving ACID and time travel. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Delta Sharing** | Open protocol for sharing live Delta tables across organisations without copying. | [30](Part-30-miscellaneous-deeper-topics.md) |
| **`_delta_log`** | The directory of numbered JSON commits and checkpoints defining a Delta table's history. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Denormalisation** | Flattening joins into one wide table for query simplicity and speed. | [23](Part-23-lab-dimensions-gold.md) |
| **Dimension table** | Descriptive context you slice measures by. Small, slowly changing. | [18](Part-18-project-blueprint-data-model.md) |
| **DLT** | Delta Live Tables — now **Lakeflow Declarative Pipelines**. Declare tables; the framework infers order. | [30](Part-30-miscellaneous-deeper-topics.md) |
| **Driver** | The coordinating node: builds plans, schedules tasks, collects results, runs your notebook. | [13](Part-13-spark-architecture-execution-model.md) |
| **Durability** | Once committed, a transaction survives crashes. | [7](Part-07-delta-lake-acid-time-travel.md) |

## E

| Term | Meaning | Part |
|------|---------|------|
| **ELT** | Extract, Load, **then** Transform — the modern lakehouse pattern. | [17](Part-17-medallion-architecture.md) |
| **Entra ID** | ☁️ Microsoft's identity service (formerly Azure AD). SSO and groups for Azure Databricks. | [29](Part-29-azure-databricks-deep-dive.md) |
| **ETL** | Extract, Transform, Load — the classic pattern where transformation precedes loading. | [1](Part-01-course-map-story-problem-statement.md) |
| **`Exchange`** | 🔴 A **shuffle** in a physical plan. Count them; fewer is better. | [15](Part-15-narrow-wide-transformations-shuffle.md) |
| **Executor** | The JVM process on a worker node that runs tasks and holds cached data. | [13](Part-13-spark-architecture-execution-model.md) |
| **Expectation** | A DLT data-quality rule: `expect` · `expect_or_drop` · `expect_or_fail`. | [30](Part-30-miscellaneous-deeper-topics.md) |
| **External location** | A storage credential bound to a specific path. What you grant users on. | [6](Part-06-tables-volumes-managed-vs-external.md) |
| **External table** | Metadata in Unity Catalog, data files in your own storage. `DROP` leaves the files. | [6](Part-06-tables-volumes-managed-vs-external.md) |

## F

| Term | Meaning | Part |
|------|---------|------|
| **Fact table** | Measurements of events — large, additive, foreign-keyed. | [18](Part-18-project-blueprint-data-model.md) |
| **Fan-out** | A join accidentally multiplying rows because the lookup side has duplicate keys. **Silent.** | [23](Part-23-lab-dimensions-gold.md) |
| **Fault tolerance** | Recomputing a lost partition from its lineage rather than checkpointing every step. | [2](Part-02-distributed-computing-hadoop-spark.md) |
| **File-arrival trigger** | Runs a job when a new file lands, rather than on a clock. | [28](Part-28-orchestration-jobs-workflows.md) |
| **Foreign key** | A column referencing another table's primary key. **Not enforced** in Databricks. | [18](Part-18-project-blueprint-data-model.md) |

## G

| Term | Meaning | Part |
|------|---------|------|
| **Genie** | Natural-language interface: English → LLM (reading metadata) → SQL → results. | [26](Part-26-genie-natural-language-analytics.md) |
| **Gold** | The medallion layer holding business-ready, joined, aggregated, business-named data. | [17](Part-17-medallion-architecture.md) |
| **Grain** | What one row of a table represents. Declare it first. | [18](Part-18-project-blueprint-data-model.md) |
| **Global temp view** | A view visible to any notebook on the same cluster, via `global_temp.name`. | [10](Part-10-sql-in-spark.md) |

## H

| Term | Meaning | Part |
|------|---------|------|
| **Hadoop** | The predecessor ecosystem: HDFS (storage) + MapReduce (compute) + YARN (manager). | [2](Part-02-distributed-computing-hadoop-spark.md) |
| **Hash partitioning** | Assigning rows by `hash(key) % n` so all rows for a key land in one partition. | [16](Part-16-partitions-repartition-coalesce.md) |
| **HDFS** | Hadoop Distributed File System. Largely replaced by cloud object storage. | [2](Part-02-distributed-computing-hadoop-spark.md) |
| **Hierarchical namespace (HNS)** | ☁️ Real directories with atomic rename. **This is what makes ADLS Gen2 Gen2.** | [29](Part-29-azure-databricks-deep-dive.md) |
| **Hive metastore** | The legacy per-workspace, two-level metastore. Superseded by Unity Catalog. | [5](Part-05-unity-catalog-governance.md) |

## I

| Term | Meaning | Part |
|------|---------|------|
| **Idempotency** | Running the same operation twice produces the same result. **The #1 property of scheduled work.** | [28](Part-28-orchestration-jobs-workflows.md) |
| **Immutability** | DataFrames can't be modified; every operation returns a new one. Enables lineage and safe parallelism. | [8](Part-08-dataframe-fundamentals.md) |
| **Incremental load** | Processing only new or changed data, not the full history. | [28](Part-28-orchestration-jobs-workflows.md) |
| **`inferSchema`** | Guessing column types by sampling. Costs an extra scan; non-deterministic. Avoid in production. | [9](Part-09-reading-writing-data.md) |
| **`information_schema`** | ANSI metadata views auto-created per catalog. A free data dictionary. | [20](Part-20-lab-environment-setup.md) |
| **Isolation** | Concurrent transactions don't interfere. Delta uses snapshot isolation + optimistic concurrency. | [7](Part-07-delta-lake-acid-time-travel.md) |

## J–L

| Term | Meaning | Part |
|------|---------|------|
| **Job** | A schedulable unit of work containing one or more tasks. | [28](Part-28-orchestration-jobs-workflows.md) |
| **Job (Spark)** | The work triggered by one action. | [13](Part-13-spark-architecture-execution-model.md) |
| **Jobs Compute** | The cheaper compute type for scheduled work. **Never use All-Purpose for jobs.** | [28](Part-28-orchestration-jobs-workflows.md) |
| **Key Vault-backed scope** | ☁️ A Databricks secret scope whose secrets live in Azure Key Vault. | [29](Part-29-azure-databricks-deep-dive.md) |
| **Lakehouse** | Data lake + data warehouse: open storage plus ACID, schema and governance. | [3](Part-03-what-databricks-is-lakehouse-editions.md) |
| **Lakeflow** | The current name for Databricks' declarative pipeline framework (was DLT). | [30](Part-30-miscellaneous-deeper-topics.md) |
| **Lazy evaluation** | Transformations build a plan; nothing runs until an action. | [14](Part-14-transformations-actions-lazy-evaluation.md) |
| **Left anti join** | Rows from the left with **no** match on the right. Your orphaned-key detector. | [11](Part-11-joins-in-spark.md) |
| **Left semi join** | Rows from the left **with** a match, left columns only, no duplication. ≡ `WHERE EXISTS`. | [11](Part-11-joins-in-spark.md) |
| **Lineage** | The auto-captured graph of how data flows between tables, notebooks and dashboards. | [5](Part-05-unity-catalog-governance.md) |
| **Liquid clustering** | Modern data layout; clustering keys changeable with `ALTER TABLE`. Replaces partitioning/Z-ORDER. | [30](Part-30-miscellaneous-deeper-topics.md) |

## M

| Term | Meaning | Part |
|------|---------|------|
| **Managed identity** | ☁️ An Azure identity with no password. What the Access Connector is. | [29](Part-29-azure-databricks-deep-dive.md) |
| **Managed resource group** | ☁️ The auto-created RG holding Databricks' VMs, NSG and DBFS root. **Don't touch it.** | [29](Part-29-azure-databricks-deep-dive.md) |
| **Managed table** | Databricks owns metadata **and** files. `DROP` deletes both. | [6](Part-06-tables-volumes-managed-vs-external.md) |
| **MapReduce** | The map-then-reduce model; also Hadoop's disk-heavy engine that Spark replaced. | [2](Part-02-distributed-computing-hadoop-spark.md) |
| **Materialized view** | A view whose result is stored and incrementally maintained. Fast reads, staleness window. | [10](Part-10-sql-in-spark.md) |
| **Medallion architecture** | Bronze → Silver → Gold progressive refinement. | [17](Part-17-medallion-architecture.md) |
| **`MERGE`** | Upsert on a business key. What makes an incremental pipeline idempotent. | [30](Part-30-miscellaneous-deeper-topics.md) |
| **`mergeSchema`** | Write option permitting new columns. Permissive at bronze, strict at silver/gold. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Metastore** | The top-level Unity Catalog container. One per region per account. | [5](Part-05-unity-catalog-governance.md) |
| **Micro-batch** | How Spark Structured Streaming works — small batches, not event-at-a-time. | [30](Part-30-miscellaneous-deeper-topics.md) |
| **MLflow** | Experiment tracking, model packaging and registry. | [30](Part-30-miscellaneous-deeper-topics.md) |

## N–O

| Term | Meaning | Part |
|------|---------|------|
| **Narrow transformation** | Each output partition depends on one input partition. **No shuffle.** | [15](Part-15-narrow-wide-transformations-shuffle.md) |
| **Node** | One computer (usually a cloud VM) in a cluster. | [2](Part-02-distributed-computing-hadoop-spark.md) |
| **No Public IP (NPIP)** | ☁️ Secure Cluster Connectivity — cluster VMs have no public IP; the tunnel is outbound. | [29](Part-29-azure-databricks-deep-dive.md) |
| **OBT (One Big Table)** | A wide pre-joined table or view so consumers never write joins. | [25](Part-25-lab-fact-gold-reporting-view.md) |
| **OLAP** | Analytical processing — few huge reads across history. | [1](Part-01-course-map-story-problem-statement.md) |
| **OLTP** | Transactional processing — many small writes as events happen. | [1](Part-01-course-map-story-problem-statement.md) |
| **Optimistic concurrency** | Prepare, then attempt an atomic commit; retry on conflict. No locks. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **`OPTIMIZE`** | Compacts small Delta files into fewer, larger ones. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Orphaned key** | A fact row referencing a dimension row that doesn't exist. Produces silent nulls. | [24](Part-24-lab-fact-bronze-silver.md) |

## P–Q

| Term | Meaning | Part |
|------|---------|------|
| **Parquet** | Open columnar file format with compression and per-row-group statistics. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Partition (Spark)** | A chunk of data = a unit of parallelism = one task. | [16](Part-16-partitions-repartition-coalesce.md) |
| **Partition (write)** | Physical directories created by `write.partitionBy`, enabling partition pruning. | [16](Part-16-partitions-repartition-coalesce.md) |
| **Photon** | Databricks' vectorised C++ execution engine. Falls back to the JVM for UDFs. | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| **Predicate pushdown** | Pushing filters into the file reader so data is skipped before decompression. | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| **Predictive Optimization** | Databricks running `OPTIMIZE`/`VACUUM`/clustering automatically on managed tables. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Primary key** | The column(s) uniquely identifying a row. Informational only in Databricks. | [18](Part-18-project-blueprint-data-model.md) |
| **Private Link** | ☁️ Private connectivity to a service over the Microsoft backbone. Front-end and back-end. | [29](Part-29-azure-databricks-deep-dive.md) |
| **`Project`** | ⭐ In a query plan, `Project` means **`SELECT`**. | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| **Quarantine** | Routing rejected rows to a `_rejects` table with a reason, instead of deleting them. | [22](Part-22-lab-dimensions-silver.md) |

## R–S

| Term | Meaning | Part |
|------|---------|------|
| **RDD** | Resilient Distributed Dataset — the low-level API. Opaque to Catalyst; prefer DataFrames. | [2](Part-02-distributed-computing-hadoop-spark.md) |
| **`repartition`** | Changes partition count in either direction. Always shuffles. Can partition by key. | [16](Part-16-partitions-repartition-coalesce.md) |
| **`RESTORE`** | Rolls a Delta table back to an earlier version, creating a new version. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Row filter** | A UC function attached to a table restricting which rows a principal sees. | [5](Part-05-unity-catalog-governance.md) |
| **`_rescued_data`** | Auto Loader column capturing values that didn't fit the schema, as JSON. | [30](Part-30-miscellaneous-deeper-topics.md) |
| **Salting** | Adding a random suffix to a hot join key to spread skew across partitions. | [15](Part-15-narrow-wide-transformations-shuffle.md) |
| **`saveAsTable`** | Writes **and** registers in Unity Catalog. Prefer over `.save(path)`. | [9](Part-09-reading-writing-data.md) |
| **SCD** | Slowly Changing Dimension. Type 1 overwrites; **Type 2** keeps history with validity dates. | [30](Part-30-miscellaneous-deeper-topics.md) |
| **Schema (namespace)** | Middle level of `catalog.schema.object`. Same as "database" in Databricks. | [5](Part-05-unity-catalog-governance.md) |
| **Schema (structure)** | Column names and types. Declared with `StructType` or a DDL string. | [9](Part-09-reading-writing-data.md) |
| **Schema enforcement** | Rejecting writes whose schema doesn't match. On by default. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **SCIM** | ☁️ Standard for syncing users and groups from Entra ID into Databricks. | [29](Part-29-azure-databricks-deep-dive.md) |
| **Secret scope** | A named collection of secrets, ideally backed by Azure Key Vault. | [29](Part-29-azure-databricks-deep-dive.md) |
| **Serverless** | Compute you request but never see, size or shut down. Seconds to start; no idle cost. | [4](Part-04-lab-account-setup-ui-tour.md) |
| **Service principal** | A non-human identity for automation. **Run jobs as one, never as a person.** | [5](Part-05-unity-catalog-governance.md) |
| **Shuffle** | Redistributing data across the network so related rows co-locate. The dominant cost. | [15](Part-15-narrow-wide-transformations-shuffle.md) |
| **Silver** | The medallion layer holding cleaned, typed, deduplicated, conformed data at the same grain. | [17](Part-17-medallion-architecture.md) |
| **Skew** | One key holding a disproportionate share of rows, so one task dominates runtime. | [15](Part-15-narrow-wide-transformations-shuffle.md) |
| **Snapshot isolation** | Readers see a consistent table version even while writers commit. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Snowflake schema** | Normalised dimensions split across multiple tables. More joins than a star. | [18](Part-18-project-blueprint-data-model.md) |
| **Spark** | Open-source distributed compute engine. **Compute, not storage.** | [2](Part-02-distributed-computing-hadoop-spark.md) |
| **SparkSession** | Your entry point to Spark. Pre-created as `spark` in Databricks. | [8](Part-08-dataframe-fundamentals.md) |
| **Spill** | Writing intermediate data to local disk when execution memory is exhausted. | [13](Part-13-spark-architecture-execution-model.md) |
| **Stage** | A set of tasks that can run without a shuffle. Boundaries are shuffles. | [13](Part-13-spark-architecture-execution-model.md) |
| **Star schema** | Fact table surrounded by denormalised dimensions. | [18](Part-18-project-blueprint-data-model.md) |
| **Storage credential** | The identity Databricks uses to reach cloud storage (IAM role / Access Connector). | [6](Part-06-tables-volumes-managed-vs-external.md) |
| **`StructType`** | Programmatic schema definition — a list of `StructField(name, type, nullable)`. | [9](Part-09-reading-writing-data.md) |
| **System tables** | Governance and billing metadata as queryable Delta tables (`system.access.audit`). | [5](Part-05-unity-catalog-governance.md) |

## T–Z

| Term | Meaning | Part |
|------|---------|------|
| **Task** | One operation on one partition, occupying one core. | [13](Part-13-spark-architecture-execution-model.md) |
| **Temp view** | A session-scoped name for a DataFrame so SQL can query it. | [10](Part-10-sql-in-spark.md) |
| **Time travel** | Querying a Delta table as of an earlier version or timestamp. **Not a backup.** | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Transaction log** | The `_delta_log` commits defining a Delta table's history. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Transformation** | An operation that builds a plan and returns a DataFrame. Doesn't execute. | [14](Part-14-transformations-actions-lazy-evaluation.md) |
| **Trusted asset** | A certified SQL function Genie calls rather than generating SQL for. | [26](Part-26-genie-natural-language-analytics.md) |
| **UDF** | User-Defined Function. Python UDFs are slow and force Photon fallback. Prefer built-ins. | [30](Part-30-miscellaneous-deeper-topics.md) |
| **UniForm** | Delta tables simultaneously exposing Iceberg (and Hudi) metadata. | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Unity Catalog** | Account-level governance for data and AI assets. | [5](Part-05-unity-catalog-governance.md) |
| **`UNDROP`** | Recovers a dropped managed table within the retention window. | [6](Part-06-tables-volumes-managed-vs-external.md) |
| **`VACUUM`** | Deletes unreferenced files past the retention window. ⚠️ **Destroys time travel beyond it.** | [7](Part-07-delta-lake-acid-time-travel.md) |
| **Vectorised execution** | Processing batches of values per instruction rather than row-at-a-time. How Photon is fast. | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| **View** | A saved query. Re-runs each time; stores no data. | [10](Part-10-sql-in-spark.md) |
| **VNet injection** | ☁️ Deploying cluster VMs into your own Azure VNet. **Creation-time only.** | [29](Part-29-azure-databricks-deep-dive.md) |
| **Volume** | Unity Catalog's governed abstraction for files. `/Volumes/catalog/schema/volume/…` | [6](Part-06-tables-volumes-managed-vs-external.md) |
| **Watermark** | How long a streaming query waits for late data before finalising a window. | [30](Part-30-miscellaneous-deeper-topics.md) |
| **Wide transformation** | Each output partition depends on many input partitions. **Requires a shuffle.** | [15](Part-15-narrow-wide-transformations-shuffle.md) |
| **Widget** | A notebook parameter (`dbutils.widgets`) — how a job passes arguments in. | [10](Part-10-sql-in-spark.md) |
| **Worker node** | The machine hosting executors. | [13](Part-13-spark-architecture-execution-model.md) |
| **WholeStageCodegen** | Fusing operators into one generated function. Shown by `*` in a plan. | [12](Part-12-query-plans-catalyst-aqe-photon.md) |
| **YARN** | Hadoop's cluster manager. One of Spark's supported managers. | [2](Part-02-distributed-computing-hadoop-spark.md) |
| **Z-ORDER** | Co-locating similar values into files to enable data skipping. Superseded by liquid clustering. | [7](Part-07-delta-lake-acid-time-travel.md) |

---

*Back to:* **[Master index](../Databricks%20-%20Study%20Guide.md)** · *Next:* **[Appendix B — Timestamp Index](Appendix-B-timestamp-index.md)**
