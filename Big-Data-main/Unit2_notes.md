# Unit 2 — Hadoop & MapReduce
> 🔴 Both topics in this unit are **Tier A** — MapReduce architecture is asked every year.

---

## 📌 Topic 1: Apache Hadoop Overview
> **🔴 Tier A | Frequency: 3/4 | Appears in Section A & B**

### Definition
Apache Hadoop is an open-source framework for distributed storage and processing of large datasets across clusters of commodity hardware using a simple programming model.

### History of Hadoop
- 2003: Google published GFS (Google File System) paper
- 2004: Google published MapReduce paper
- 2006: Doug Cutting and Mike Cafarella created Hadoop (named after Doug's son's toy elephant 🐘)
- 2008: Hadoop became Apache top-level project
- 2012: Hadoop 2.0 introduced YARN

### Core Components of Apache Hadoop

| Component | Description |
|-----------|-------------|
| **HDFS** | Hadoop Distributed File System — storage layer |
| **YARN** | Yet Another Resource Negotiator — resource management |
| **MapReduce** | Programming model for distributed data processing |
| **Hadoop Common** | Shared utilities and libraries for other modules |

### Hadoop Ecosystem (Full)

![Hadoop Ecosystem](diagrams/hadoop_ecosystem.svg)

### How Hadoop Processes Data
1. Data stored across multiple DataNodes in HDFS (as blocks)
2. YARN's ResourceManager allocates compute resources
3. ApplicationMaster requests containers from NodeManagers
4. MapReduce jobs run inside containers co-located with data (data locality)
5. Results written back to HDFS

### Key Features of Hadoop
- **Fault Tolerant**: Replicates data across 3 nodes by default
- **Scalable**: Add nodes without downtime (linear scaling)
- **Cost-effective**: Runs on commodity hardware
- **Flexible**: Handles structured, semi, and unstructured data
- **Data Locality**: Moves computation to data, not data to computation

### Hadoop Streaming
- Allows programs written in ANY language (Python, Ruby, Perl) to run as MapReduce jobs
- Uses standard I/O (stdin/stdout) as the communication channel
- Mapper reads from stdin, emits key-value to stdout
- Reducer reads sorted key-value pairs from stdin

### Hadoop Pipes
- C++ API for Hadoop that allows MapReduce in C++
- Uses socket-based communication between Java framework and C++ program

### Scale Up vs Scale Out

| | Scale Up (Vertical) | Scale Out (Horizontal) |
|-|--------------------|-----------------------|
| **Method** | Add more CPU/RAM to one server | Add more servers to the cluster |
| **Cost** | Expensive (enterprise hardware) | Cheap (commodity servers) |
| **Limit** | Physical hardware limits | Virtually unlimited |
| **Hadoop uses** | ❌ Not preferred | ✅ Primary strategy |
| **Fault tolerance** | Low (single point of failure) | High (distributed replication) |

> Hadoop uses **Scale Out** — when data grows, just add more DataNodes!

---

## 📌 Topic 2: MapReduce Architecture & Workflow
> **🔴 Tier A | Frequency: 4/4 — Asked EVERY YEAR**

### Definition
MapReduce is a programming model for processing large datasets in parallel across a distributed cluster. It divides the work into two phases: **Map** (transformation) and **Reduce** (aggregation).

### MapReduce Architecture Diagram
![MapReduce Architecture](diagrams/mapreduce_architecture.svg)

### MapReduce Workflow — Step by Step

**Step 1: Input Splitting**
- Input file is split into fixed-size chunks (InputSplits), typically matching HDFS block size (128MB)
- Each split is processed by one mapper

**Step 2: Map Phase**
- Each Mapper reads one split, processes records, emits (key, value) pairs
- Example (Word Count): ("hello", 1), ("world", 1), ("hello", 1)

**Step 3: Combiner (optional)**
- A mini-reducer that runs locally on the mapper output before shuffle
- Reduces network traffic
- Example: Combines ("hello",1),("hello",1) → ("hello",2) before sending

**Step 4: Shuffle & Sort**
- Framework transfers mapper output to the correct reducer (by key)
- Sorts all (key, value) pairs by key
- All values for the same key are grouped together: ("hello", [2,1])

**Step 5: Reduce Phase**
- Reducer receives (key, list of values), performs aggregation
- Example: ("hello", [2,1]) → ("hello", 3)

**Step 6: Output**
- Final (key, value) pairs written to HDFS output directory

### Key Components

| Component | Role |
|-----------|------|
| **JobTracker** | Master — schedules jobs, monitors TaskTrackers (Hadoop 1.x) |
| **TaskTracker** | Slave — executes map/reduce tasks on each node |
| **ResourceManager** | YARN master — allocates cluster resources |
| **NodeManager** | YARN slave — manages containers on each node |
| **ApplicationMaster** | Manages lifecycle of one application |
| **InputFormat** | Defines how input files are split and read |
| **OutputFormat** | Defines how output is written |
| **Partitioner** | Determines which reducer gets which key (default: hash) |
| **RecordReader** | Reads (key,value) from each input split |

### MapReduce Types

| Type | Description |
|------|-------------|
| **Mapper** | One-to-one or one-to-many transformation |
| **Reducer** | Many-to-one aggregation |
| **Combiner** | Local reducer, reduces shuffle data |
| **Partitioner** | Controls data distribution to reducers |

### Input Formats

| Format | Description |
|--------|-------------|
| **TextInputFormat** | Default. Each line = one record |
| **KeyValueTextInputFormat** | Splits by tab character |
| **SequenceFileInputFormat** | Binary format, compressed |
| **NLineInputFormat** | N lines per split |

### Job Scheduling in MapReduce

| Scheduler | How it works |
|-----------|-------------|
| **FIFO** | Default. First In First Out — simple but no fairness |
| **Fair Scheduler** | Distributes resources equally across all jobs |
| **Capacity Scheduler** | Predefined capacity per queue, guarantees minimum resources |

### Anatomy of a MapReduce Job Run

```
1. Client submits job (jar + config + input/output paths)
2. JobTracker assigns JobID
3. Input splits calculated and stored in HDFS
4. TaskTracker gets task assignment from JobTracker
5. TaskTracker launches JVM for each task
6. Map tasks run, output spilled to local disk
7. Shuffle: Mapper output transferred to Reducer nodes
8. Sort: All key-value pairs sorted by key
9. Reduce tasks run, output written to HDFS
10. JobTracker marks job complete, notifies client
```

### Failures in MapReduce
- **Task failure**: TaskTracker reports failure, JobTracker reschedules on another node
- **TaskTracker failure**: JobTracker marks it dead (no heartbeat for 1 min), reassigns tasks
- **JobTracker failure**: Entire job fails (single point of failure in Hadoop 1.x — fixed by YARN in 2.x)

### Real-world MapReduce Example (Word Count)

**Map Function:**
```
Input: "big data is big"
Output: (big,1), (data,1), (is,1), (big,1)
```

**After Shuffle & Sort:**
```
(big, [1,1]), (data, [1]), (is, [1])
```

**Reduce Function:**
```
Output: (big,2), (data,1), (is,1)
```

---

## 📝 Section A Quick-Answer Bank (Unit 2)

| Question | Answer (2 marks) |
|----------|-----------------|
| Role of Sort & Shuffle | Groups all values with same key together and sends them to correct reducer. Reduces network traffic and sorts mapper output. |
| What is Hadoop Streaming? | Allows non-Java programs (Python, Ruby) to be used as mapper/reducer via stdin/stdout. |
| Three benefits of MapReduce | Scalable, fault-tolerant, cost-effective on commodity hardware |
| What is Hadoop? | Open-source framework for distributed storage (HDFS) and processing (MapReduce) at petabyte scale |
| What are Hadoop components? | HDFS, YARN, MapReduce, Hadoop Common |

---

## 🎯 Exam Answer Tips — Unit 2

- **MapReduce diagram is mandatory** for Section B and C answers — always draw it.
- For "Illustrate the architecture of Map-Reduce" — draw the full pipeline and explain each step.
- Scale Up vs Scale Out question: Make a comparison table (asked once but good for 5-mark part).
- For Hadoop ecosystem: Draw the layered diagram (tools on top, YARN in middle, HDFS at bottom).
- Word Count is the canonical example — always use it to explain MapReduce steps.
- Section C MapReduce question: Introduction + Architecture Diagram + 6 steps explained + components table = full marks.
