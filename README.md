# 📊 BIG DATA (KCS061 / BCS061) — Complete Exam Study Guide

> **B.Tech Sem VI | AKTU** | Compiled from 4 years of PYQs (2021-22 to 2024-25)

---

## 🗂️ TABLE OF CONTENTS
1. [Exam Pattern](#exam-pattern)
2. [PYQ Analysis Summary](#pyq-analysis-summary)
3. [Scoring Strategy](#scoring-strategy)
4. [Tier A — Must Read Topics](#tier-a--must-read)
5. [Tier B — Should Read Topics](#tier-b--should-read)
6. [Tier C — Skip if Short on Time](#tier-c--skip)
7. [Day-wise Study Plan](#day-wise-study-plan)
8. [File Index](#file-index)

---

## Exam Pattern

### Current Pattern (2024-25 onwards — 70 Marks)

| Section | Description | Marks per Q | Questions | Total |
|---------|-------------|-------------|-----------|-------|
| Section A | Attempt ALL — Short answer | 2 marks | 7 | 14 |
| Section B | Attempt ANY 3 — Medium answer | 7 marks | 5 options | 21 |
| Section C | Attempt ANY 1 from each pair — Long answer | 7 marks | 5 pairs (Q3–Q7) | 35 |
| **TOTAL** | | | | **70** |

### Previous Pattern (2021-22 to 2023-24 — 100 Marks)

| Section | Description | Marks per Q | Questions | Total |
|---------|-------------|-------------|-----------|-------|
| Section A | Attempt ALL — Short answer | 2 marks | 10 | 20 |
| Section B | Attempt ANY 3 — Medium answer | 10 marks | 5 options | 30 |
| Section C | Attempt ANY 1 from each pair — Long answer | 10 marks | 5 pairs (Q3–Q7) | 50 |
| **TOTAL** | | | | **100** |

> ⚠️ **Note:** The exam changed to 70 marks in 2024-25. Prepare for 70-mark format.
> Each question in Section C is mapped to one unit (Q3→Unit1, Q4→Unit2, Q5→Unit3, Q6→Unit4, Q7→Unit5).

---

## PYQ Analysis Summary

### Unit-wise Question Frequency (Sections B & C)

| Topic | Unit | 21-22 | 22-23 | 23-24 | 24-25 | Freq | Tier |
|-------|------|-------|-------|-------|-------|------|------|
| Big Data Platforms | 1 | ✅ | ✅ | ✅ | ✅ | 4/4 | 🔴 A |
| 5 Vs of Big Data | 1 | ❌ | ❌ | ✅ | ✅ | 2/4 | 🟡 B→A* |
| Big Data Architecture/Components | 1 | ✅ | ✅ | ✅ | ❌ | 3/4 | 🔴 A |
| Analysis vs Reporting | 1 | ❌ | ✅ | ✅ | ✅ | 3/4 | 🔴 A |
| Forms/Types of Big Data | 1 | ✅ | ❌ | ✅ | ✅ | 3/4 | 🔴 A |
| Big Data Applications | 1 | ✅ | ❌ | ✅ | ❌ | 2/4 | 🟡 B |
| MapReduce Architecture | 2 | ✅ | ✅ | ❌ | ✅ | 3/4 | 🔴 A |
| Hadoop Ecosystem/Components | 2 | ❌ | ✅ | ✅ | ✅ | 3/4 | 🔴 A |
| Scale Up vs Scale Out | 2 | ✅ | ❌ | ❌ | ❌ | 1/4 | 🟢 C |
| MapReduce Application Development | 2 | ❌ | ❌ | ✅ | ❌ | 1/4 | 🟡 B |
| HDFS Design & Concepts | 3 | ✅ | ✅ | ✅ | ✅ | 4/4 | 🔴 A |
| HDFS Read/Write Operations | 3 | ✅ | ✅ | ✅ | ✅ | 4/4 | 🔴 A |
| HDFS Benefits & Challenges | 3 | ✅ | ❌ | ✅ | ❌ | 2/4 | 🟡 B |
| Block Size & Replication | 3 | ✅ | ✅ | ✅ | ✅ | 4/4 | 🔴 A |
| Setting Up Hadoop Cluster | 3 | ❌ | ✅ | ❌ | ✅ | 2/4 | 🟡 B |
| Flume & Sqoop | 3 | ❌ | ✅ | ❌ | ❌ | 1/4 | 🟢 C |
| Master-Slave Replication | 3 | ❌ | ✅ | ❌ | ✅ | 2/4 | 🟡 B |
| NoSQL Databases | 4 | ✅ | ✅ | ✅ | ❌ | 3/4 | 🔴 A |
| MongoDB CRUD | 4 | ✅ | ✅ | ✅ | ❌ | 3/4 | 🔴 A |
| YARN Architecture | 4 | ❌ | ✅ | ❌ | ✅ | 2/4 | 🟡 B→A* |
| Schedulers (Fair/Capacity) | 4 | ❌ | ✅ | ✅ | ❌ | 2/4 | 🟡 B |
| Apache Spark & RDDs | 4 | ❌ | ✅ | ✅ | ✅ | 3/4 | 🔴 A |
| MongoDB Indexing | 4 | ✅ | ❌ | ❌ | ❌ | 1/4 | 🟢 C |
| Scala | 4 | ❌ | ✅ | ❌ | ❌ | 1/4 | 🟢 C |
| HIVE Architecture | 5 | ✅ | ✅ | ✅ | ✅ | 4/4 | 🔴 A |
| Apache Pig (Modes, Latin) | 5 | ✅ | ❌ | ❌ | ✅ | 2/4 | 🔴 A* |
| Pig vs MapReduce/SQL/HIVE | 5 | ✅ | ✅ | ❌ | ✅ | 3/4 | 🔴 A |
| HBase | 5 | ❌ | ❌ | ✅ | ❌ | 1/4 | 🟢 C |
| HiveQL | 5 | ❌ | ❌ | ✅ | ❌ | 1/4 | 🟢 C |

> *These appear consistently in Section A (short questions) even when not in B/C

---

## Scoring Strategy

### For 70-Mark Exam

| Coverage | Section A | Section B | Section C | Total | Percentage |
|----------|-----------|-----------|-----------|-------|------------|
| **Tier A only** | ~10/14 | ~18/21 | ~28/35 | **~56/70** | **~80%** |
| **Tier A + B** | ~12/14 | ~21/21 | ~35/35 | **~68/70** | **~97%** |
| **Tier A only (minimum)** | 8/14 | 14/21 | 21/35 | **43/70** | **61%** ✅ Pass |

### Section-wise Approach
- **Section A:** Every Tier A topic generates 1–2 short questions. Know definitions cold.
- **Section B:** Prepare 4 out of 5 topics thoroughly (MapReduce, HDFS read/write, Hadoop ecosystem, HIVE, Pig comparisons).
- **Section C:** Each unit has exactly 1 question (2 options). Knowing 1 option well = full marks.

---

## Tier A — Must Read

> 🔴 These topics appear 3–4 times across 4 years. Master these first.

### Unit 1
| Topic | Freq | Focus |
|-------|------|-------|
| Big Data Platforms (list + explain) | 4/4 | Hadoop, Spark, Flink, Cassandra, MongoDB — list + 2-line each |
| Big Data Architecture & Components | 3/4 | Ingestion → Storage → Processing → Analytics → Visualization |
| Analysis vs Reporting | 3/4 | Difference table + what each enables |
| Forms/Types of Big Data | 3/4 | Structured, Semi-structured, Unstructured with examples |
| 5 Vs of Big Data | 3/4 | Volume, Velocity, Variety, Veracity, Value — diagram + examples |

### Unit 2
| Topic | Freq | Focus |
|-------|------|-------|
| MapReduce Architecture | 3/4 | Diagram mandatory — Input→Split→Map→Shuffle→Reduce→Output |
| Hadoop Ecosystem Components | 3/4 | All components: HDFS, YARN, MapReduce, Pig, Hive, HBase, ZooKeeper |

### Unit 3
| Topic | Freq | Focus |
|-------|------|-------|
| HDFS Design & Concepts | 4/4 | NameNode, DataNode, file namespace, replication |
| HDFS Read/Write Operations | 4/4 | Step-by-step client read and write with diagram |
| Block Size & Data Replication | 4/4 | Default 128MB block, replication factor 3 |

### Unit 4
| Topic | Freq | Focus |
|-------|------|-------|
| NoSQL Databases | 3/4 | Types: Key-Value, Document, Column, Graph + comparison with RDBMS |
| MongoDB CRUD Operations | 3/4 | All 4 operations with code examples |
| Apache Spark & RDDs | 3/4 | Architecture, RDD properties, Spark on YARN |

### Unit 5
| Topic | Freq | Focus |
|-------|------|-------|
| HIVE Architecture | 4/4 | Diagram: UI → Driver → Compiler → Metastore → Execution |
| Apache Pig (Pig Latin + Modes) | 4/4 | Local vs MapReduce mode, Grunt, Pig Latin operators |
| Pig vs MapReduce vs SQL vs HIVE | 3/4 | Comparison table — most repeated question |

---

## Tier B — Should Read

> 🟡 These appear 2/4 years. Cover after Tier A.

| Topic | Unit | Freq |
|-------|------|------|
| Big Data Applications (healthcare, finance, e-commerce) | 1 | 2/4 |
| MapReduce Application Development | 2 | 2/4 |
| HDFS Benefits & Challenges | 3 | 2/4 |
| Setting Up Hadoop Cluster | 3 | 2/4 |
| Master-Slave vs Peer-Peer Replication | 3 | 2/4 |
| YARN Architecture | 4 | 2/4 |
| Schedulers (Fair & Capacity) | 4 | 2/4 |
| MongoDB Indexing | 4 | 2/4 |
| HBase Concepts | 5 | 1/4 (new trend) |

---

## Tier C — Skip

> 🟢 Appeared 0–1 times. Skip if short on time.

| Topic | Unit | Reason |
|-------|------|--------|
| History of Big Data / Hadoop History | 1, 2 | Never asked directly |
| Scale Up vs Scale Out | 2 | Only 2021-22 |
| Hadoop Streaming & Pipes | 2 | Rarely asked long |
| Flume & Sqoop (detailed) | 3 | Only Section A |
| Hadoop I/O: Compression, Avro | 3 | Never long question |
| Hadoop Archives | 3 | Never asked |
| Scala (detailed) | 4 | Only 2022-23 |
| MongoDB Indexing (detailed) | 4 | Only 2021-22 |
| HiveQL | 5 | Only 2023-24 |
| HBase (detailed) | 5 | Only 2023-24 |
| CMM / Hadoop benchmarks | 3 | Never asked |

---

## Day-wise Study Plan

> Assuming 5 days of preparation. Adjust if you have more time.

| Day | Units | Topics | Priority |
|-----|-------|--------|----------|
| **Day 1** | Unit 1 | 5Vs, Big Data Platforms, Architecture, Analysis vs Reporting, Types of Data | 🔴 All Tier A |
| **Day 2** | Unit 2 | MapReduce Architecture + flow diagram, Hadoop Ecosystem overview | 🔴 All Tier A |
| **Day 3** | Unit 3 | HDFS Design, Read/Write operations, Block replication, Cluster setup | 🔴 All Tier A |
| **Day 4** | Unit 4 | NoSQL types, MongoDB CRUD, Spark + RDDs, YARN, Schedulers | 🔴 A + 🟡 B |
| **Day 5** | Unit 5 | HIVE architecture + data flow, Pig Latin, Pig comparisons table | 🔴 All Tier A |
| **Day 6** *(if available)* | All | Tier B topics, revision of diagrams, Section A short answers | 🟡 B |

---

## File Index

| File | Contents |
|------|----------|
| `README.md` | This file — Master guide |
| `Unit1_notes.md` | Big Data Introduction, 5Vs, Platforms, Architecture |
| `Unit2_notes.md` | Hadoop, MapReduce Architecture & Workflow |
| `Unit3_notes.md` | HDFS Design, Read/Write, Replication, Cluster Setup |
| `Unit4_notes.md` | YARN, NoSQL, MongoDB, Spark, Schedulers |
| `Unit5_notes.md` | Pig, HIVE, HBase — Frameworks |
| `diagrams/` | All SVG diagrams referenced in unit notes |

---

