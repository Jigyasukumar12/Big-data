# Unit 4 — Hadoop Ecosystem: YARN, NoSQL, MongoDB, Spark
> 🔴 NoSQL, MongoDB, and Spark are all Tier A. YARN is Tier B but frequently tested.

---

## 📌 Topic 1: YARN — Yet Another Resource Negotiator
> **🟡 Tier B→A | Frequency: 2/4 (long) + every year Section A**

### Definition
YARN is the resource management layer of Hadoop 2.x. It separates resource management and job scheduling from the processing engine (MapReduce), enabling multiple processing frameworks to run on the same cluster.

### Why YARN? (Hadoop 1.x Problems)
- JobTracker did BOTH resource management and job scheduling — bottleneck
- Only MapReduce could run on Hadoop 1.x
- Single point of failure at JobTracker

### YARN Architecture Diagram
![YARN Architecture](diagrams/yarn_architecture.svg)

### YARN Components

| Component | Role |
|-----------|------|
| **ResourceManager (RM)** | Global master — allocates cluster resources across all applications |
| **NodeManager (NM)** | Per-node agent — manages containers, monitors resource usage |
| **ApplicationMaster (AM)** | Per-application — negotiates resources, monitors task execution |
| **Container** | Unit of resource allocation (fixed CPU + memory) |

### YARN ResourceManager Sub-components
1. **Scheduler**: Allocates resources (Fair/Capacity/FIFO)
2. **Applications Manager**: Accepts job submissions, starts ApplicationMaster

### How YARN Works — Step by Step
```
1. Client submits application to ResourceManager
2. ResourceManager's ApplicationsManager starts ApplicationMaster in a Container
3. ApplicationMaster registers with ResourceManager
4. ApplicationMaster requests resource containers from ResourceManager's Scheduler
5. ResourceManager provides containers (CPU + RAM) on NodeManagers
6. ApplicationMaster launches tasks inside those containers
7. Tasks run and report progress to ApplicationMaster
8. ApplicationMaster reports completion to ResourceManager
9. ResourceManager releases all containers
```

### Hadoop 2.0 New Features
- **YARN**: Enables multiple frameworks (Spark, Tez, Storm) alongside MapReduce
- **NameNode High Availability**: Active + Standby NameNode — no single point of failure
- **HDFS Federation**: Multiple NameNodes manage different namespace volumes
- **MRv2**: MapReduce runs as YARN application
- **Running MRv1 in YARN**: Via compatibility layer

---

## 📌 Topic 2: Schedulers — Fair & Capacity
> **🟡 Tier B | Frequency: 2/4**

### FIFO Scheduler (Default)
- Jobs queued in order submitted
- First job gets all resources until complete
- Simple but unfair — small jobs starve behind large jobs

### Fair Scheduler
- Each application gets an **equal share** of resources
- If only 1 job → gets all resources; when 2nd arrives → each gets 50%
- **Preemptive**: Can kill lower-priority containers to give to new job
- Organized into **pools** (queues) — each pool gets fair share

### Capacity Scheduler (Default in Hadoop 2.x)
- Cluster divided into **queues** with guaranteed minimum capacity
- Each queue can have multiple users with guaranteed resources
- Unused capacity from one queue can be borrowed by others
- **No preemption** by default

| Feature | FIFO | Fair | Capacity |
|---------|------|------|---------|
| **Multiple jobs** | One at a time | Shares equally | Guaranteed per queue |
| **Small job latency** | High | Low | Medium |
| **Preemption** | N/A | Yes | Optional |
| **Multi-tenant** | No | Yes | Yes |
| **Hadoop default** | 1.x | Not default | 2.x default |

---

## 📌 Topic 3: NoSQL Databases
> **🔴 Tier A | Frequency: 3/4 | Asked in both Section B & C**

### Definition
NoSQL (Not Only SQL) databases are non-relational data stores that provide flexible schema, horizontal scalability, and high performance for modern web-scale applications.

### Why NoSQL? (Limitations of RDBMS)
- RDBMS requires predefined schema — inflexible for semi/unstructured data
- RDBMS scaling is vertical (expensive) — NoSQL scales horizontally
- RDBMS struggles with very high write throughput (social media)
- RDBMS JOINS are slow for huge distributed datasets

### Types of NoSQL Databases

| Type | Description | Examples | Use Case |
|------|-------------|---------|---------|
| **Key-Value** | Simple dict — key maps to value | Redis, DynamoDB, Riak | Session cache, shopping cart |
| **Document** | Documents (JSON/BSON) with nested structure | MongoDB, CouchDB | E-commerce, CMS, catalog |
| **Column-Family** | Columns grouped into families, sparse | HBase, Cassandra | Time-series, IoT, analytics |
| **Graph** | Nodes and edges representing relationships | Neo4j, Amazon Neptune | Social networks, fraud detection |

### NoSQL vs Relational Database (RDBMS)

| Aspect | RDBMS | NoSQL |
|--------|-------|-------|
| **Schema** | Fixed, predefined | Dynamic, flexible |
| **Scaling** | Vertical (scale up) | Horizontal (scale out) |
| **ACID** | Full ACID | BASE (eventually consistent) |
| **Query** | SQL (complex joins) | Simple key-based, limited joins |
| **Data model** | Tables (rows/columns) | Documents, graphs, KV pairs |
| **Best for** | Complex queries, transactions | High volume, varied data |
| **Examples** | MySQL, Oracle, PostgreSQL | MongoDB, Cassandra, Redis |

### CAP Theorem (for context)
- **Consistency**: All nodes see same data at same time
- **Availability**: Every request gets a response
- **Partition Tolerance**: System works despite network partitions
- NoSQL systems choose **AP** (available + partition tolerant) over strict consistency

---

## 📌 Topic 4: MongoDB — Complete Guide
> **🔴 Tier A | Frequency: 3/4 | CRUD is asked every time**

### Definition
MongoDB is a document-oriented NoSQL database that stores data as BSON (Binary JSON) documents. It provides flexible schema, horizontal scaling, and rich query language.

### Key MongoDB Concepts

| Concept | RDBMS Equivalent | Description |
|---------|-----------------|-------------|
| **Database** | Database | Container for collections |
| **Collection** | Table | Group of documents |
| **Document** | Row | BSON object |
| **Field** | Column | Key-value in document |
| **_id** | Primary Key | Auto-generated unique identifier |

### MongoDB Data Types
- String, Integer, Double, Boolean
- Date, Timestamp
- ObjectId (12-byte unique ID)
- Array, Embedded Document
- Binary Data, Regular Expression

### CRUD Operations with Examples

#### CREATE (Insert)
```javascript
// Insert one document
db.students.insertOne({
  name: "Rahul",
  age: 21,
  course: "B.Tech",
  marks: 85
})

// Insert many documents
db.students.insertMany([
  { name: "Priya", age: 20, marks: 92 },
  { name: "Amit", age: 22, marks: 78 }
])
```

#### READ (Query)
```javascript
// Find all
db.students.find()

// Find with condition
db.students.find({ marks: { $gt: 80 } })

// Find with projection (show only name, marks)
db.students.find({}, { name: 1, marks: 1, _id: 0 })

// Find one
db.students.findOne({ name: "Rahul" })
```

#### UPDATE
```javascript
// Update one document
db.students.updateOne(
  { name: "Rahul" },
  { $set: { marks: 90 } }
)

// Update many
db.students.updateMany(
  { marks: { $lt: 50 } },
  { $set: { status: "fail" } }
)
```

#### DELETE
```javascript
// Delete one
db.students.deleteOne({ name: "Amit" })

// Delete many
db.students.deleteMany({ marks: { $lt: 40 } })

// Delete all
db.students.deleteMany({})
```

### MongoDB Indexing
> 🟡 Tier B — know the concept

- Indexes improve query performance (like index in a book)
- Default index: `_id` field (always exists)
- Create index: `db.students.createIndex({ name: 1 })` (1=ascending, -1=descending)
- Compound index: `db.students.createIndex({ name: 1, age: -1 })`
- **Without index**: Full collection scan (slow)
- **With index**: B-tree search (fast)

### Does MongoDB Support ACID?
- **Traditional MongoDB**: NO full ACID (only document-level atomicity)
- **MongoDB 4.x+**: YES, supports multi-document ACID transactions
- Single document operations are always atomic
- For exam (2021-22 question): "MongoDB supports atomicity at document level. Full ACID transactions added in MongoDB 4.0"

### Capped Collections
- Fixed-size collections that automatically overwrite oldest documents when full
- Like a circular buffer / ring buffer
- Use case: Logging, event data, temporary storage
- `db.createCollection("logs", { capped: true, size: 5242880, max: 5000 })`

---

## 📌 Topic 5: Apache Spark & RDDs
> **🔴 Tier A | Frequency: 3/4**

### Definition
Apache Spark is an open-source, in-memory distributed computing framework that provides fast and general-purpose cluster computing. It is up to 100x faster than MapReduce for certain workloads.

### Spark vs MapReduce

| Aspect | MapReduce | Apache Spark |
|--------|-----------|-------------|
| **Processing** | Disk-based | In-memory (RAM) |
| **Speed** | Slow (disk I/O each step) | 10-100x faster |
| **Ease of use** | Java-heavy, verbose | Concise APIs (Python, Scala, R) |
| **Real-time** | No (batch only) | Yes (Spark Streaming) |
| **ML support** | Limited | Built-in MLlib |
| **Graph** | No | GraphX |
| **Fault tolerance** | HDFS replication | RDD lineage |

### Spark Architecture

```
┌─────────────────────────────────────────┐
│              Spark Driver               │
│    (SparkContext, DAGScheduler)         │
└──────────────────┬──────────────────────┘
                   │
         ┌─────────┼─────────┐
         ↓         ↓         ↓
    ┌─────────┐ ┌─────────┐ ┌─────────┐
    │Executor │ │Executor │ │Executor │
    │ (Node1) │ │ (Node2) │ │ (Node3) │
    └─────────┘ └─────────┘ └─────────┘
```

### Spark Components

| Component | Description |
|-----------|-------------|
| **Spark Core** | Basic functionality: RDDs, task scheduling, memory management |
| **Spark SQL** | SQL queries on structured data |
| **Spark Streaming** | Real-time stream processing |
| **MLlib** | Machine learning library |
| **GraphX** | Graph computation |
| **SparkR** | R language interface |

### RDDs — Resilient Distributed Datasets
> Core abstraction of Spark

### Definition
RDD is an immutable, fault-tolerant, distributed collection of elements that can be processed in parallel across a cluster.

### RDD Properties
1. **Resilient**: Fault-tolerant via lineage — if partition lost, recompute from parent
2. **Distributed**: Data distributed across cluster nodes
3. **Dataset**: Collection of data records
4. **Immutable**: Once created, cannot be modified (create new RDD via transformation)
5. **Lazily evaluated**: Transformations not computed until an action is called

### RDD Operations

| Type | Operations | Description |
|------|-----------|-------------|
| **Transformations** | map, filter, flatMap, groupByKey, reduceByKey, join | Return new RDD (lazy) |
| **Actions** | count, collect, first, take, reduce, saveAsTextFile | Return value to driver (triggers computation) |

### Anatomy of a Spark Job Run
```
1. SparkContext created (driver connects to cluster)
2. User code creates RDDs from data source
3. Transformations applied (build DAG of operations)
4. Action triggered → DAG Scheduler creates Stages
5. Task Scheduler assigns Tasks to Executors
6. Executors run tasks on partitions
7. Results returned to driver or written to storage
```

### Spark on YARN
- Spark runs as a YARN application
- Two deploy modes:
  - **Client mode**: Driver runs on client machine
  - **Cluster mode**: Driver runs inside YARN ApplicationMaster
- `spark-submit --master yarn --deploy-mode cluster myapp.jar`

---

## 📌 Topic 6: Scala (Brief Overview)
> **🟢 Tier C | Frequency: 1/4 — skip if time is limited**

### Key Points
- Scala = Scalable Language — runs on JVM, interoperable with Java
- Statically typed, functional + object-oriented hybrid
- Preferred language for Spark development
- **Classes and Objects**: `class Person(name: String, age: Int)`
- **Functions**: `def add(x: Int, y: Int): Int = x + y`
- **Pattern matching**: Like advanced switch-case
- **Closures**: Functions that capture outer scope variables

---

## 📝 Section A Quick-Answer Bank (Unit 4)

| Question | Answer |
|----------|--------|
| What is YARN? | Resource management layer of Hadoop 2.x that decouples resource management from MapReduce, enabling multiple frameworks to run on Hadoop cluster |
| Compare NoSQL vs RDBMS | NoSQL: flexible schema, horizontal scaling, eventual consistency. RDBMS: fixed schema, vertical scaling, ACID |
| Does MongoDB support ACID? | Partially — atomic at document level; full ACID transactions from MongoDB 4.0 |
| What is Fair Scheduler? | YARN scheduler that gives equal share of resources to all running jobs simultaneously |
| What is Capacity Scheduler? | YARN scheduler that allocates guaranteed minimum capacity to each queue/department |
| What are RDDs? | Resilient Distributed Datasets — immutable, fault-tolerant distributed collections in Spark |
| What is Spark vs MapReduce? | Spark is 100x faster due to in-memory processing; MapReduce uses disk I/O |

---

## 🎯 Exam Answer Tips — Unit 4

- **MongoDB CRUD**: Always write code examples — examiners expect actual `db.collection.insertOne()` syntax.
- **NoSQL types table** (Key-Value, Document, Column, Graph) is mandatory for "Classify NoSQL" question.
- **YARN components**: ResourceManager + NodeManager + ApplicationMaster + Container — draw the diagram.
- **RDD properties**: The 5 properties (Resilient, Distributed, Dataset, Immutable, Lazy) are a standard answer.
- **Spark vs MapReduce table** is very likely to appear — memorize at least 5 rows of comparison.
- For Section C on NoSQL: define NoSQL → list 4 types with table → compare with RDBMS → conclusion.
