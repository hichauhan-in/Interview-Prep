# Data Engineering - Complete Study Guide

**Your Background:** Beginner to intermediate to advanced  
**Mode:** Interview Prep (includes behavioral + practice questions)  
**Last Updated:** 2026-06-11

---

## 📋 Master Index & Progress Tracker

Follow the Parts in order. Each builds on the previous one.

| # | Part | Topics | Status |
|---|------|--------|--------|
| **FOUNDATIONS** | | | |
| 1 | [Database Fundamentals](prep/Part-1-Database-Fundamentals.md) | What is a database? DBMS vs RDBMS. Relational vs NoSQL. Transaction concepts. | ✅ Done |
| 2 | [SQL Essentials: DDL & DML](prep/Part-2-SQL-DDL-DML.md) | CREATE, ALTER, INSERT, UPDATE, DELETE. Integrity constraints. Basic operations. | ✅ Done |
| **CORE TECHNICAL** | | | |
| 3 | [SQL Queries & Data Retrieval](prep/Part-3-SQL-Queries.md) | SELECT, WHERE, ORDER BY, LIMIT, LIKE. Built-in functions. Aliases. | ✅ Done |
| 4 | [SQL Advanced: Joins & Subqueries](prep/Part-4-SQL-Advanced.md) | INNER/LEFT/RIGHT/FULL JOINs. Subqueries, IN, NOT IN, EXISTS. Set operations. | ✅ Done |
| 5 | [SQL Expert: Window Functions & CTEs](prep/Part-5-SQL-Expert.md) | Window functions (OVER, PARTITION BY, ORDER BY). Frames. CTEs (recursive & iterative). CASE statements. GROUP BY, HAVING, GROUP_CONCAT. | ✅ Done |
| 6 | [Big Data Fundamentals](prep/Part-6-BigData-Fundamentals.md) | 5 V's of Big Data. Distributed computation & storage. Cluster architecture. Commodity hardware. | ✅ Done |
| 7 | [Hadoop Ecosystem](prep/Part-7-Hadoop.md) | Hadoop architecture. HDFS, NameNode, DataNode. MapReduce, YARN. Cluster setup on GCP (Dataproc). | ✅ Done |
| 8 | [Hive: SQL on Hadoop](prep/Part-8-Hive.md) | Hive architecture, metastore. Data types. CREATE TABLE, LOAD DATA. Internal vs external tables. SerDe. | ✅ Done |
| 9 | [Hive: Partitioning, Bucketing & Joins](prep/Part-9-Hive-Advanced.md) | Static & dynamic partitioning. Bucketing. Map-side joins, skew joins. File formats (ORC, Parquet, Avro). | ✅ Done |
| 10 | [Apache Kafka](prep/Part-10-Kafka.md) | Kafka cluster architecture. Brokers, topics, partitions. Producers & consumers. Consumer groups. Offset management. Replication. Commits. | ✅ Done |
| 11 | [Kafka Schema Registry & Streaming](prep/Part-11-Kafka-Advanced.md) | Schema Registry (Avro, Protobuf, JSON Schema). Key/value serialization. Confluent Kafka setup. | ✅ Done |
| **APPLIED & PROCESS** | | | |
| 12 | [Real-World Pipelines & Architectures](prep/Part-12-Pipelines.md) | ETL vs ELT. Data pipeline patterns. Slowly Changing Dimensions (SCD). Data warehousing concepts. | ✅ Done |
| 13 | [Performance & Optimization](prep/Part-13-Optimization.md) | Query optimization (indexes, EXPLAIN). Partitioning strategies. Compression trade-offs. | ✅ Done |
| **MISCELLANEOUS & DEEPER TOPICS** | | | |
| 14 | [Miscellaneous & Advanced Topics](prep/Part-14-Miscellaneous.md) | Data governance, metadata management, monitoring. Trends: Apache Iceberg, Delta Lake, Spark streaming intro. | ✅ Done |
| **INTERVIEW & BEHAVIORAL** | | | |
| 15 | [Interview Question Bank](prep/Part-15-Question-Bank.md) | 100+ interview questions (20% Basic, 20% Intermediate, 60% Advanced). Self-quiz tracker. | ✅ Done |
| 16 | [Behavioral & Closing Prep](prep/Part-16-Behavioral.md) | STAR method. Ready-to-adapt STAR stories. Why this role/company. Questions to ask. Night-before cheat sheet. | ✅ Done |
| **CAPSTONE** | | | |
| 🏆 | [Capstone Project: RetailStream Analytics](prep/Capstone-Project.md) | End-to-end pipeline: Kafka + Schema Registry → HDFS → Hive (ORC/partitioned) → star schema + SCD2 → optimized analytics. With labs & steps. | ✅ Done |

---

## 📌 How to Use This Guide

All 16 Parts plus the Capstone Project are complete. Each Part includes:
- Plain-English explanations with real-world analogies
- Mermaid diagrams for complex concepts
- 🔍 Deep-dive callouts for tricky topics
- Comparison tables (X vs Y)
- 🧪 Hands-on labs with runnable code/commands
- ⭐ 5–8 likely interview questions + model answers
- 🧠 30-second memory hooks

**Suggested path:** Work Parts 1 → 16 in order (each builds on the last), do the labs as you go, then build the [Capstone Project](prep/Capstone-Project.md). Finally, drill the [Question Bank](prep/Part-15-Question-Bank.md) aloud and rehearse your [Behavioral stories](prep/Part-16-Behavioral.md).

---

## 🎯 Your Learning Goals

- **Never go blank** — know fundamentals + the reasoning behind each answer
- **Pitch at your level** — assuming beginner basics, but building to intermediate/advanced depth
- **Ready for interviews** — 100+ questions, practice scenarios, STAR stories

---

✅ **All modules complete** — 16 Parts + Capstone, with explanations, diagrams, labs, 100+ interview questions, and STAR prep. Start at [Part 1](prep/Part-1-Database-Fundamentals.md).
