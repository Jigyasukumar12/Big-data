# Unit 5 — Hadoop Ecosystem Frameworks: Pig, HIVE & HBase
> 🔴 HIVE architecture and Pig comparisons are asked EVERY year — highest priority in Unit 5.

---

## 📌 Topic 1: Apache Pig — Complete Guide
> **🔴 Tier A | Frequency: 4/4 in Section A | 2/4 in Section B/C**

### Definition
Apache Pig is a high-level data flow scripting language and execution framework built on top of Hadoop for processing large datasets. It abstracts the complexity of MapReduce programming.

### Key Facts
- Developed at Yahoo! (2006), open-sourced as Apache project
- Language: **Pig Latin** — procedural data flow language
- Each Pig Latin statement translates into a series of MapReduce jobs
- Can handle semi-structured and unstructured data
- Ideal for ETL (Extract, Transform, Load) pipelines

### Pig Architecture Diagram
![Pig Architecture](diagrams/pig_architecture.svg)

### Execution Modes of Pig

| Mode | Description | When to use |
|------|-------------|------------|
| **Local Mode** | Runs on single JVM, uses local filesystem | Development, testing with small data |
| **MapReduce Mode** | Runs on Hadoop cluster, uses HDFS | Production, large-scale data |
| **Tez Mode** | Runs on Apache Tez (DAG-based, faster than MR) | Production with speed requirement |
| **Spark Mode** | Runs on Apache Spark engine | When Spark cluster available |

### Grunt Shell
- Interactive command-line interpreter for Pig
- Start with: `pig` (MR mode) or `pig -x local` (local mode)
- Common Grunt commands:
  - `fs -ls /` — list HDFS files
  - `sh pwd` — run shell command
  - `exec myscript.pig` — execute a Pig script
  - `help` — list commands
  - `quit` — exit

### Pig Latin — Key Operators

| Operator | Description | Example |
|----------|-------------|---------|
| **LOAD** | Load data from HDFS | `A = LOAD '/data/file.txt' AS (name:chararray, age:int);` |
| **DUMP** | Print to screen | `DUMP A;` |
| **STORE** | Save to HDFS | `STORE A INTO '/output' USING PigStorage(',');` |
| **FILTER** | Filter rows by condition | `B = FILTER A BY age > 18;` |
| **GROUP** | Group by field | `C = GROUP A BY name;` |
| **JOIN** | Join two relations | `D = JOIN A BY id, B BY id;` |
| **ORDER BY** | Sort data | `E = ORDER A BY age DESC;` |
| **LIMIT** | Limit rows | `F = LIMIT A 10;` |
| **FOREACH...GENERATE** | Transform each row | `G = FOREACH A GENERATE name, age*2;` |
| **DISTINCT** | Remove duplicates | `H = DISTINCT A;` |
| **UNION** | Merge two relations | `I = UNION A, B;` |
| **CROSS** | Cartesian product | `J = CROSS A, B;` |

### Simple Pig Latin Example — Word Count
```pig
-- Load input
lines = LOAD '/input/data.txt' AS (line:chararray);

-- Tokenize each line into words
words = FOREACH lines GENERATE FLATTEN(TOKENIZE(line)) AS word;

-- Group by word
grouped = GROUP words BY word;

-- Count occurrences
counts = FOREACH grouped GENERATE group AS word, COUNT(words) AS count;

-- Store output
STORE counts INTO '/output/wordcount';
```

### User Defined Functions (UDF)
- Written in Java, Python, or JavaScript
- Extend Pig's built-in functions for custom processing
- Register: `REGISTER myudf.jar;`
- Use: `DEFINE MyFunc com.example.MyFunction();`
- Types: **EvalFunc** (transform), **FilterFunc** (filter), **LoadFunc** (load)

---

## 📌 Topic 2: Pig vs Databases, MapReduce, SQL, and HIVE
> **🔴 Tier A | Frequency: 3/4 — Most Repeated Question in Unit 5**

### Apache Pig vs MapReduce

| Aspect | Apache Pig | MapReduce |
|--------|-----------|-----------|
| **Language** | Pig Latin (simple scripting) | Java (complex) |
| **Lines of code** | 10x less code | Very verbose |
| **Flexibility** | Less flexible | Fully flexible |
| **Ease of use** | Easy, less coding | Hard, expert needed |
| **Optimization** | Automatic (optimizer) | Manual tuning needed |
| **Schema** | Optional (schema-on-read) | No schema concept |
| **Debugging** | Easy with Grunt | Complex |
| **Speed** | Same underlying MR | Same speed |

### Apache Pig vs SQL

| Aspect | Apache Pig | SQL |
|--------|-----------|-----|
| **Data model** | Nested, complex types (bags, tuples, maps) | Flat tables |
| **Type** | Procedural (step-by-step) | Declarative (what to get) |
| **Schema** | Optional | Mandatory |
| **Joins** | Supported, but explicit | Implicit via FK |
| **Scale** | Petabytes (HDFS) | Limited to one server (RDBMS) |
| **Usage** | ETL, data transformation | Reporting, ad-hoc queries |

### Apache Pig vs HIVE

| Aspect | Apache Pig | Apache HIVE |
|--------|-----------|-------------|
| **Language** | Pig Latin (procedural) | HiveQL (SQL-like, declarative) |
| **Target users** | Programmers, data engineers | Analysts, SQL users |
| **Schema** | Optional (schema-on-read) | Required (schema-on-write) |
| **Data flow** | Better for complex transforms | Better for ad-hoc queries |
| **When to use** | ETL pipelines | Data warehouse queries |
| **Latency** | Moderate | High (batch-oriented) |
| **Partitioning** | Manual | Built-in partitioning |

---

## 📌 Topic 3: Apache HIVE — Complete Guide
> **🔴 Tier A | Frequency: 4/4 — Asked EVERY YEAR**

### Definition
Apache HIVE is a data warehouse infrastructure built on top of Hadoop that provides SQL-like query language (HiveQL) to analyze large datasets stored in HDFS.

### HIVE Architecture Diagram
![HIVE Architecture](diagrams/hive_architecture.svg)

### HIVE Components

| Component | Description |
|-----------|-------------|
| **UI (CLI/Web/JDBC)** | User interface for submitting HiveQL queries |
| **Driver** | Receives queries, manages their lifecycle |
| **Compiler** | Parses HiveQL → creates logical plan → optimizes |
| **Optimizer** | Optimizes the logical plan for efficiency |
| **Execution Engine** | Executes physical plan as MapReduce/Tez/Spark jobs |
| **Metastore** | Stores metadata (table schemas, partitions, column types) |
| **HDFS** | Actual data storage for Hive tables |

### HIVE Metastore
- A central repository of HIVE metadata
- Stores: database names, table names, column types, table locations, partition info
- Backed by a relational DB (MySQL, Derby)
- Without Metastore, HIVE cannot know where tables are or their schema

### Data Flow in HIVE
```
1. User writes HiveQL query in CLI/JDBC
2. Driver receives query
3. Compiler parses and converts to Abstract Syntax Tree (AST)
4. Semantic Analyzer validates against Metastore
5. Logical Plan created
6. Optimizer applies rule-based/cost-based optimizations
7. Physical Plan generated
8. Execution Engine submits MapReduce job to YARN
9. Results read from HDFS and returned to user
```

### HiveQL — Key Commands

```sql
-- Create database
CREATE DATABASE sales_db;
USE sales_db;

-- Create table
CREATE TABLE employees (
  id INT,
  name STRING,
  salary FLOAT,
  dept STRING
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
STORED AS TEXTFILE;

-- Load data from HDFS
LOAD DATA INPATH '/data/emp.csv' INTO TABLE employees;

-- Query
SELECT dept, AVG(salary) as avg_sal
FROM employees
WHERE salary > 30000
GROUP BY dept
ORDER BY avg_sal DESC;

-- Create partitioned table
CREATE TABLE orders (
  order_id INT,
  amount FLOAT
)
PARTITIONED BY (year INT, month INT);

-- Insert into partition
INSERT INTO orders PARTITION(year=2024, month=6)
SELECT order_id, amount FROM raw_orders WHERE year=2024;
```

### HIVE Table Types

| Type | Description |
|------|-------------|
| **Managed (Internal)** | HIVE controls data. DROP TABLE = data deleted |
| **External** | HIVE only has metadata. DROP TABLE = only metadata deleted, HDFS data remains |
| **Partitioned** | Data physically separated by partition column (year, month) — improves query performance |
| **Bucketed** | Data distributed into fixed number of files using hash — enables efficient sampling/joins |

### HIVE vs Traditional RDBMS

| Aspect | HIVE | RDBMS (MySQL) |
|--------|------|---------------|
| **Scale** | Petabytes (HDFS) | Gigabytes (single server) |
| **Latency** | High (minutes) | Low (seconds) |
| **ACID** | Limited (ORC format + Transactions) | Full ACID |
| **Update/Delete** | Limited support | Full DML |
| **Schema** | Schema-on-read | Schema-on-write |
| **Use case** | Batch analytics | OLTP |

---

## 📌 Topic 4: HBase — Brief Overview
> **🟡 Tier B | Frequency: 1/4 — Appeared in 2023-24 Section C**

### Definition
HBase is an open-source, distributed, column-oriented NoSQL database built on top of HDFS. It is modeled after Google's Bigtable and provides real-time read/write access to large datasets.

### HBase vs RDBMS

| Aspect | HBase | RDBMS |
|--------|-------|-------|
| **Data model** | Column-family oriented | Row-oriented (tables) |
| **Scale** | Billions of rows, millions of columns | Millions of rows |
| **Schema** | Sparse, dynamic columns | Fixed columns |
| **Transactions** | Single-row ACID only | Full multi-row ACID |
| **Query** | Simple get/put/scan | Complex SQL with joins |
| **Use case** | Real-time random access to Big Data | OLTP systems |

### HBase Key Concepts
- **Table**: Collection of rows, organized into column families
- **Row Key**: Unique identifier for each row (sorted lexicographically)
- **Column Family**: Group of columns stored together (defined at table creation)
- **Column Qualifier**: Name within a column family (dynamic)
- **Cell**: Intersection of row + column + timestamp
- **Region**: Horizontal partition of a table (like a shard)
- **RegionServer**: Server managing a set of regions

### HBase Architecture
```
HBase Master → manages RegionServers
RegionServer → manages Regions (partitions of table)
Region → contains HFile data stored in HDFS
ZooKeeper → coordinates distributed HBase components
```

### When to Use HBase (vs HIVE)

| Use Case | Choose |
|----------|--------|
| Real-time lookups (ms) | HBase |
| Batch analytics, SQL queries | HIVE |
| Sparse, evolving schema | HBase |
| Structured data with joins | HIVE |

---

## 📝 Section A Quick-Answer Bank (Unit 5)

| Question | Answer |
|----------|--------|
| What is Apache Pig? | High-level scripting language (Pig Latin) for processing large datasets on Hadoop. Abstracts MapReduce complexity. |
| Describe Grunt shell | Interactive command-line shell for Pig. Used for development and testing. Commands: LOAD, DUMP, STORE, etc. |
| Describe schema | Structure that defines column names and data types. In HIVE: schema stored in Metastore. In Pig: optional. |
| Types of data in HIVE | Primitive (INT, STRING, FLOAT, BOOLEAN) and Complex (ARRAY, MAP, STRUCT) |
| Metastore in HIVE | Central metadata repository for HIVE — stores table schemas, partition info, column types, stored in relational DB |
| Pig vs MapReduce | Pig: simple Pig Latin, 10x less code. MR: Java, verbose but more flexible. Both run MapReduce underneath. |
| Execution modes of Pig | Local mode (single JVM, testing) and MapReduce mode (cluster, production) |

---

## 🎯 Exam Answer Tips — Unit 5

- **HIVE Architecture diagram is MANDATORY** — draw UI → Driver → Compiler → Optimizer → Metastore + HDFS
- **Pig vs MapReduce vs SQL vs HIVE comparison table** is the single most repeated Unit 5 question — always have it ready.
- For "Execution Modes of Pig" — explain Local mode + MapReduce mode with when to use each.
- For "Pig Latin" question — write 5-6 operators with syntax and example.
- HIVE data flow step-by-step (9 steps) scores full marks for architecture question.
- For Section C (HIVE architecture + data flow) — diagram + component table + data flow steps = 7-10 marks.
- HBase: Know the comparison table with RDBMS and key concepts — sufficient for Tier B coverage.
