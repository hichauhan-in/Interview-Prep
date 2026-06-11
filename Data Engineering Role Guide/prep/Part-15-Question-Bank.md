# Part 15 — Interview Question Bank

> Section goal: 100+ questions across the whole curriculum to test recall and build confidence. Mix: ~20% Basic, ~20% Intermediate, ~60% Advanced, plus Behavioral and Closing. Each has a concise answer/hint and a cross-reference to the Part that explains it.

Covers index items **15** (comprehensive self-assessment across all modules).

---

## How to Use This Bank
1. Cover the answers; say your response **aloud** (interviews are verbal).
2. Mark each question in the **self-quiz tracker** at the bottom: ✅ confident / 🔁 review / ❌ relearn.
3. Revisit ❌ and 🔁 questions until all are ✅.

---

## 🟢 Basic (Questions 1–22)

**1. What is a database?** → Organized collection of data for fast retrieval/management. *(Part 1)*

**2. DBMS vs RDBMS?** → DBMS manages data; RDBMS stores it in related tables with keys + ACID. *(Part 1)*

**3. What are ACID properties?** → Atomicity, Consistency, Isolation, Durability. *(Part 1)*

**4. SQL vs NoSQL?** → SQL: fixed schema, strong consistency; NoSQL: flexible, horizontal scale, eventual consistency. *(Part 1)*

**5. Primary key vs foreign key?** → PK uniquely identifies a row (unique+not null); FK references another table's PK. *(Parts 1–2)*

**6. What are DDL, DML, DQL, DCL?** → Define / Manipulate / Query / Control-access. *(Part 2)*

**7. DELETE vs TRUNCATE vs DROP?** → Rows w/ WHERE (rollback) / all rows (fast, reset, no rollback) / entire table. *(Part 2)*

**8. What is a constraint? Name some.** → A rule enforcing valid data: PK, FK, NOT NULL, UNIQUE, CHECK, DEFAULT. *(Part 2)*

**9. What does AUTO_INCREMENT do?** → Auto-generates sequential unique IDs. *(Part 2)*

**10. What does SELECT do? Basic syntax?** → Reads data: SELECT cols FROM table WHERE ... *(Part 3)*

**11. WHERE vs HAVING?** → WHERE filters rows pre-grouping; HAVING filters groups post-aggregation. *(Parts 3–4)*

**12. `%` vs `_` in LIKE?** → `%` = any number of chars; `_` = exactly one. *(Part 3)*

**13. What is an aggregate function? Examples?** → Summarizes many rows: COUNT, SUM, AVG, MIN, MAX. *(Part 3)*

**14. What is an alias?** → A temporary name for a column/table via AS. *(Part 3)*

**15. What is a JOIN?** → Combines rows from multiple tables on a related column. *(Part 4)*

**16. INNER vs LEFT JOIN?** → INNER: matches only; LEFT: all left + matches, NULLs otherwise. *(Part 4)*

**17. What is the 5 V's of Big Data?** → Volume, Velocity, Variety, Veracity, Value. *(Part 6)*

**18. Structured vs unstructured data?** → Tables/CSV vs images/video/free text. *(Part 6)*

**19. What is Hadoop?** → Framework for distributed storage + processing on commodity clusters. *(Part 7)*

**20. What is HDFS?** → Hadoop's distributed file system splitting files into replicated blocks. *(Part 7)*

**21. What is Kafka?** → Distributed event-streaming platform (append-only log) for data in motion. *(Part 10)*

**22. What is a Kafka topic?** → A named feed of messages, split into partitions. *(Part 10)*

---

## 🟡 Intermediate (Questions 23–44)

**23. Explain the logical order of SQL execution.** → FROM→WHERE→GROUP BY→HAVING→SELECT→ORDER BY→LIMIT. *(Part 3)*

**24. How do you paginate results?** → LIMIT n OFFSET m. *(Part 3)*

**25. Difference between COUNT(*) and COUNT(col)?** → COUNT(*) counts all rows; COUNT(col) skips NULLs. *(Part 3)*

**26. What is a self join?** → A table joined to itself (e.g., employee→manager). *(Part 4)*

**27. What is a correlated subquery?** → A subquery referencing the outer query, re-run per outer row. *(Part 4)*

**28. NOT IN vs NOT EXISTS — which is NULL-safe?** → NOT EXISTS; NOT IN returns nothing if the list has a NULL. *(Part 4)*

**29. What does GROUP_CONCAT do?** → Concatenates group values into one string. *(Part 4)*

**30. What does CASE WHEN do? A use case?** → SQL if/else; pay bands or pivoting. *(Part 4)*

**31. What does ROLLUP add?** → Subtotal + grand-total super-aggregate rows. *(Part 4)*

**32. Vertical vs horizontal scaling?** → Bigger machine (ceiling) vs more machines (big-data choice). *(Part 6)*

**33. Distributed storage vs computation?** → Split files across nodes (HDFS) vs process pieces in parallel (MapReduce). *(Part 6)*

**34. Why are columnar formats faster for analytics?** → Read only needed columns + better compression. *(Parts 6, 8)*

**35. Roles of NameNode and DataNode?** → Metadata master vs data-block workers. *(Part 7)*

**36. Default HDFS block size and replication?** → 128 MB, 3×. *(Part 7)*

**37. Explain MapReduce phases.** → Map (emit k,v) → Shuffle/Sort (group) → Reduce (aggregate). *(Part 7)*

**38. What is the Hive metastore?** → RDBMS storing table schemas + HDFS data locations. *(Part 8)*

**39. Internal vs external Hive table?** → DROP deletes data (internal) vs keeps data (external). *(Part 8)*

**40. What is schema-on-read?** → Schema applied at query time, not load time. *(Part 8)*

**41. What is a SerDe?** → Serializer/Deserializer translating file bytes ↔ Hive rows. *(Part 8)*

**42. What is a consumer group in Kafka?** → Consumers splitting a topic's partitions; one partition per consumer per group. *(Part 10)*

**43. What is a Kafka offset?** → A message's position in a partition; committed to track progress. *(Part 10)*

**44. What is ETL vs ELT?** → Transform before vs after loading into the warehouse. *(Part 12)*

---

## 🔴 Advanced (Questions 45–104)

**45. GROUP BY vs window function?** → GROUP BY collapses rows; window keeps all rows, adds computed column. *(Part 5)*

**46. ROW_NUMBER vs RANK vs DENSE_RANK?** → Unique / ties+gaps / ties+no gaps. *(Part 5)*

**47. Find the 2nd highest salary per department.** → DENSE_RANK() OVER (PARTITION BY dept ORDER BY salary DESC), filter rank=2. *(Part 5)*

**48. What is the frame clause? Example?** → Defines rows around current; ROWS BETWEEN 2 PRECEDING AND CURRENT ROW = 3-row moving avg. *(Part 5)*

**49. What do LAG and LEAD do?** → Fetch previous/next row's value (month-over-month). *(Part 5)*

**50. What is a CTE? Advantage over subquery?** → Named temp result (WITH); readable, reusable, recursive. *(Part 5)*

**51. Explain a recursive CTE.** → Anchor + recursive member via UNION ALL; traverses hierarchies. *(Part 5)*

**52. What does COALESCE do?** → Returns first non-null of its arguments. *(Part 5)*

**53. How do you compute a running total in SQL?** → SUM(x) OVER (ORDER BY ...). *(Part 5)*

**54. How do you pivot rows to columns?** → SUM(CASE WHEN col='X' THEN val ELSE 0 END). *(Part 4)*

**55. How do you find rows in A with no match in B?** → LEFT JOIN ... WHERE B.key IS NULL, or NOT EXISTS. *(Part 4)*

**56. What is data locality?** → Move computation to data, not data to computation. *(Parts 6–7)*

**57. What problem does YARN solve? Components?** → Cluster resource management; ResourceManager, NodeManager, ApplicationMaster, containers. *(Part 7)*

**58. What happens when a DataNode fails?** → NameNode detects under-replication and re-replicates from surviving copies. *(Part 7)*

**59. Why is the NameNode a single point of failure? Mitigation?** → Holds all metadata; mitigate with Standby NameNode + HA/ZooKeeper. *(Part 7)*

**60. Static vs dynamic partitioning in Hive?** → You specify vs Hive infers partitions from data. *(Part 9)*

**61. Partitioning vs bucketing?** → Folders by value (low cardinality) vs fixed N hash files (high cardinality). *(Part 9)*

**62. What is partition pruning?** → Reading only partitions matching the query filter. *(Part 9)*

**63. What is over-partitioning and why bad?** → Too many tiny folders/files from high-cardinality keys; overwhelms NameNode. *(Part 9)*

**64. Explain map-side (broadcast) join.** → Load small table into each mapper's memory; no shuffle. *(Part 9)*

**65. What is a sort-merge-bucket (SMB) join?** → Both tables bucketed+sorted on key; merge sorted streams. *(Part 9)*

**66. What is data skew? How handle a skew join?** → One key dominates; hive.optimize.skewjoin splits the hot key. *(Part 9)*

**67. Which file format for analytics and why?** → ORC/Parquet — columnar, compressed, column projection. *(Parts 6, 8)*

**68. What complex types does Hive support?** → ARRAY, MAP, STRUCT. *(Part 8)*

**69. Does Kafka guarantee ordering? How keep related events ordered?** → Per-partition only; same key → same partition. *(Part 10)*

**70. What if consumers > partitions?** → Extra consumers idle (parallelism capped by partitions). *(Part 10)*

**71. Sync vs async commit in Kafka?** → commitSync waits/retries (reliable, slow); commitAsync fire-and-forget (fast). *(Part 10)*

**72. Explain at-most/at-least/exactly-once delivery.** → Commit before / after processing / idempotent+transactions. *(Part 10)*

**73. How does Kafka avoid data loss?** → RF≥3, acks=all, min.insync.replicas≥2; ISR-only leader election. *(Part 10)*

**74. What are ISR and leader/follower replicas?** → In-sync replicas eligible to become leader; leader serves I/O, followers copy. *(Part 10)*

**75. What do producer acks (0/1/all) mean?** → No wait / leader only / all in-sync replicas. *(Part 10)*

**76. Why use a Schema Registry?** → Central versioned schema contract; small schema-ID messages; enforced compatibility. *(Part 11)*

**77. Avro vs Protobuf vs JSON Schema?** → Compact+evolution / fast cross-language / readable but large. *(Part 11)*

**78. Backward vs forward compatibility?** → New reads old / old reads new. *(Part 11)*

**79. Which schema changes are safe?** → Add optional field with default; never change types/remove required. *(Part 11)*

**80. What is log compaction?** → Retain only latest value per key (changelog/snapshot). *(Part 11)*

**81. What is Kafka Connect / ksqlDB?** → No-code connectors / SQL on streams. *(Part 11)*

**82. Lambda vs Kappa architecture?** → Batch+speed layers merged vs streaming-only with replay. *(Part 12)*

**83. Star vs snowflake schema?** → Denormalized dims (fast) vs normalized dims (less redundancy). *(Part 12)*

**84. Fact vs dimension table?** → Measurable events vs descriptive context. *(Part 12)*

**85. Explain SCD Types 1/2/3.** → Overwrite / new versioned row / extra column. *(Part 12)*

**86. How implement SCD Type 2?** → Expire current row (end_date, is_current=false), insert new current row. *(Part 12)*

**87. What is an index and the trade-off?** → Sorted lookup; faster reads, slower writes + storage. *(Part 13)*

**88. How read EXPLAIN output?** → type ALL=scan (bad), ref/range=index (good); watch rows, filesort, temporary. *(Part 13)*

**89. Why can a function in WHERE hurt performance?** → Blocks index use; rewrite as a range. *(Part 13)*

**90. How optimize a slow Hive query?** → Partition + ORC/Parquet + compression + right join + Tez/Spark + vectorization + stats. *(Part 13)*

**91. Snappy vs Gzip compression?** → Fast/balanced vs small/slow (cold data). *(Part 13)*

**92. How increase Kafka throughput?** → Producer batching+compression, more partitions/consumers, tune fetch sizes. *(Part 13)*

**93. What is predicate pushdown?** → Push filters to storage layer to skip non-matching data. *(Part 13)*

**94. Why did Spark replace MapReduce?** → In-memory processing, faster, friendlier APIs, streaming. *(Part 14)*

**95. What is Airflow / a DAG?** → Orchestrator; tasks + dependencies, no cycles. *(Part 14)*

**96. Data lake vs warehouse vs lakehouse?** → Raw/cheap vs curated/fast vs both (ACID via Delta/Iceberg). *(Part 14)*

**97. What do Delta/Iceberg/Hudi add over Parquet?** → ACID, time travel, schema evolution, upserts. *(Part 14)*

**98. HBase vs Hive?** → Low-latency random access vs batch analytics. *(Part 14)*

**99. What is dbt?** → SQL transformation tool with testing/version control (the T in ELT). *(Part 14)*

**100. What is data lineage and why it matters?** → Origin + transformation trail; debugging, compliance, trust. *(Part 14)*

**101. Design a real-time fraud-detection pipeline.** → Kafka ingest → stream processor (Spark/Flink) scoring → alerts + sink to lake/warehouse. *(Parts 10–12)*

**102. How would you handle late-arriving data in streaming?** → Watermarks + windowing; allow grace period, then update aggregates. *(Parts 12, 14)*

**103. How would you migrate a Hadoop/Hive stack to the cloud?** → Move HDFS→object storage, Hive→Spark SQL + Iceberg/Delta, orchestrate with Airflow, adopt ELT/dbt. *(Part 14)*

**104. How do you ensure data quality in a pipeline?** → Automated checks (nulls/ranges/uniqueness), tests (Great Expectations/dbt), monitoring/alerts, lineage. *(Part 14)*

---

## 🧑‍💼 Behavioral (STAR) — see Part 16 for full prep

**B1. Tell me about a challenging data problem you solved.**
**B2. Describe a time you optimized a slow query or pipeline.**
**B3. Tell me about a time you handled a production data incident.**
**B4. Describe working with a difficult stakeholder on data requirements.**
**B5. Tell me about a time you learned a new technology quickly.**
**B6. Describe a mistake you made with data and how you handled it.**

---

## 🎯 Closing / "Why" Questions — see Part 16

**C1. Why do you want this data engineering role?**
**C2. Why this company?**
**C3. Why should we hire you?**
**C4. Where do you see your data engineering skills growing?**
**C5. Do you have questions for us?** (Always say yes — Part 16 lists strong ones.)

---

## 📊 Self-Quiz Tracker

| Range | Topic | ✅ Confident | 🔁 Review | ❌ Relearn |
|-------|-------|-------------|-----------|------------|
| 1–22 | Basics (DB, SQL, Big Data, Kafka intro) | | | |
| 23–44 | Intermediate (queries, HDFS, Hive, Kafka) | | | |
| 45–55 | Advanced SQL (windows, CTEs, joins) | | | |
| 56–68 | Hadoop & Hive internals | | | |
| 69–81 | Kafka deep + Schema Registry | | | |
| 82–86 | Pipelines & modeling | | | |
| 87–93 | Optimization | | | |
| 94–104 | Modern stack & design | | | |
| B1–B6 | Behavioral | | | |
| C1–C5 | Closing | | | |

> 💡 **Target:** all rows fully ✅ before interview day. Re-test ❌/🔁 every few days.

---

## 🧠 30-Second Memory Hooks
- Drill **aloud**, not silently — interviews are verbal.
- The most-asked advanced trio: **2nd-highest salary (window)**, **internal vs external table**, **Kafka ordering/consumer groups**.
- For "design" questions, narrate the **modern stack**: Kafka → object storage → Spark + Iceberg → warehouse, orchestrated by Airflow.

---

*Next suggested section:* **Part 16 — Behavioral & Closing Prep** (turn technical knowledge into a confident, well-rounded interview performance).
