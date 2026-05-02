# Unit 3 — HDFS & Hadoop Environment
> 🔴 HDFS topics are asked EVERY year — highest priority in the entire syllabus.

---

## 📌 Topic 1: HDFS Design & Core Concepts
> **🔴 Tier A | Frequency: 4/4 | Asked every year in both Section B and C**

### Definition
HDFS (Hadoop Distributed File System) is a distributed, fault-tolerant file system designed to store very large files across clusters of commodity hardware. It is modeled after Google File System (GFS).

### Design Goals of HDFS
1. **Very large files**: Files of gigabytes to terabytes
2. **Streaming data access**: Write-once, read-many access pattern (not random access)
3. **Commodity hardware**: Runs on cheap, failure-prone machines
4. **Fault tolerance**: Automatic recovery from hardware failure via replication

### What HDFS Does NOT Do Well
- Low-latency access (use HBase instead)
- Lots of small files (NameNode bottleneck)
- Multiple writers / arbitrary file modification

---

## 📌 Topic 2: HDFS Concepts — NameNode, DataNode, Blocks
> **🔴 Tier A | Frequency: 4/4**

### HDFS Architecture (Master-Slave)

```
         ┌──────────────┐
         │  NameNode    │  ← Master
         │  (Metadata)  │
         └──────┬───────┘
                │
    ┌───────────┼──────────────┐
    ↓           ↓              ↓
┌────────┐  ┌────────┐   ┌────────┐
│DataNode│  │DataNode│   │DataNode│  ← Slaves
│ (Data) │  │ (Data) │   │ (Data) │
└────────┘  └────────┘   └────────┘
```

### NameNode
- **Master node** — manages the file system namespace
- Stores **metadata**: file names, directory structure, block locations, permissions
- Maintains **EditLog** (all changes) and **FsImage** (filesystem snapshot)
- Does NOT store actual data
- **Single point of failure** in Hadoop 1.x (solved by Secondary NameNode and HA in 2.x)

### Secondary NameNode
- NOT a backup NameNode
- Periodically merges FsImage + EditLog to create a new FsImage
- Reduces NameNode startup time
- In Hadoop 2.x, replaced by **Standby NameNode** for true HA

### DataNode
- **Slave nodes** — store actual data as blocks
- Send **heartbeat** to NameNode every 3 seconds
- Send **block report** to NameNode every 6 hours (list of all blocks they store)
- If NameNode doesn't receive heartbeat for 10 minutes → marks DataNode dead → triggers re-replication

### File System Namespace
- Hierarchical (like Linux filesystem): `/user/hadoop/data.txt`
- Supports: mkdir, ls, rm, cp, mv operations via CLI
- NameNode tracks: path → list of block IDs → block ID → list of DataNodes

---

## 📌 Topic 3: Block Size & Data Replication
> **🔴 Tier A | Frequency: 4/4 | Always in Section A**

### Block Size
- **Default block size: 128 MB** (was 64 MB in older versions)
- A large file is split into blocks of 128 MB and stored across multiple DataNodes
- A 300 MB file = Block 1 (128MB) + Block 2 (128MB) + Block 3 (44MB)
- **Block abstraction benefits:**
  - A file larger than any single disk can be stored
  - Simplifies storage subsystem
  - Enables fault tolerance and parallelism

### Data Replication
- **Default replication factor: 3** (configurable via `dfs.replication`)
- Each block is copied to 3 different DataNodes
- **Rack-aware replication strategy:**
  - Replica 1: Local DataNode (same rack as writer)
  - Replica 2: DataNode in a different rack
  - Replica 3: Another DataNode in that different rack
  - This ensures: fast local write + rack-level fault tolerance

### Why Replication Factor = 3?
- If one DataNode fails: 2 replicas survive
- If one rack fails: 1 replica in other rack survives
- 3 gives balance between reliability and storage overhead

---

## 📌 Topic 4: HDFS Read & Write Operations
> **🔴 Tier A | Frequency: 4/4 — Most important topic in Unit 3**

### HDFS Read/Write Diagram
![HDFS Read Write](diagrams/hdfs_read_write.svg)

### WRITE Operation — Step by Step

```
1. Client calls DistributedFileSystem.create()
2. DistributedFileSystem makes RPC call to NameNode
3. NameNode creates new file entry in namespace (no blocks yet)
4. NameNode returns list of DataNodes for first block (e.g., DN1, DN2, DN3)
5. Client creates DFSOutputStream — writes data in packets
6. Data flows as a PIPELINE: Client → DN1 → DN2 → DN3
7. Each DataNode writes block, then sends ACK back upstream
8. DN3 → DN2 → DN1 → Client (ACK pipeline)
9. Client notifies NameNode when file is complete (close())
10. NameNode records final block locations
```

### READ Operation — Step by Step

```
1. Client calls DistributedFileSystem.open()
2. DistributedFileSystem calls NameNode → gets block locations
3. NameNode returns list: Block1→[DN1,DN2,DN3], Block2→[DN2,DN3,DN1]
4. Client connects to NEAREST DataNode for Block1 (minimizes latency)
5. Data streams from DataNode to client
6. If DataNode fails, client tries next DataNode in list
7. Process repeats for all blocks
8. Client closes stream
```

### HDFS Benefits

| Benefit | Explanation |
|---------|-------------|
| **Fault Tolerant** | 3x replication — any 2 DataNodes can fail, data survives |
| **High Throughput** | Streaming access, data locality, parallel reads |
| **Scalable** | Add DataNodes without downtime |
| **Cost-effective** | Commodity hardware |
| **Data Locality** | Computation moved to data (not vice versa) |

### HDFS Challenges

| Challenge | Explanation |
|-----------|-------------|
| **Small files** | Each file's metadata stored in NameNode RAM — millions of small files exhaust RAM |
| **Random access** | Designed for streaming, not random seeks |
| **Single NameNode** | NameNode RAM is the limit for namespace size (Hadoop 1.x) |
| **Write once** | Files cannot be modified in place — only append supported |
| **Latency** | High latency compared to local disk for small operations |

---

## 📌 Topic 5: Java Interface & Command Line Interface to HDFS

### Key Java Classes
- `FileSystem` — base class for filesystem operations
- `Path` — represents file/directory path in HDFS
- `FSDataInputStream` — for reading from HDFS
- `FSDataOutputStream` — for writing to HDFS

### Important CLI Commands

```bash
# List files
hadoop fs -ls /user/hadoop/

# Create directory
hadoop fs -mkdir /user/hadoop/data

# Upload file from local to HDFS
hadoop fs -put localfile.txt /user/hadoop/

# Download from HDFS to local
hadoop fs -get /user/hadoop/file.txt ./

# Show file content
hadoop fs -cat /user/hadoop/file.txt

# Delete file
hadoop fs -rm /user/hadoop/file.txt

# Show disk usage
hadoop fs -du -h /user/hadoop/

# Check replication
hadoop fsck /user/hadoop/file.txt -files -blocks -locations

# Change replication factor
hadoop fs -setrep -w 2 /user/hadoop/file.txt
```

---

## 📌 Topic 6: Data Ingest — Flume & Sqoop
> **🟡 Tier B | Frequency: 1/4 (Section A only)**

### Apache Flume
- Tool for **collecting and moving large amounts of log data** from servers to HDFS
- Architecture: **Source → Channel → Sink**
- Source: Web server logs, syslog, JMS
- Channel: Memory, file-based buffer
- Sink: HDFS, HBase, Kafka

### Apache Sqoop
- Tool for **bulk transfer between RDBMS (MySQL, Oracle) and HDFS**
- Import: RDBMS → HDFS (sqoop import)
- Export: HDFS → RDBMS (sqoop export)
- Uses parallel MapReduce to transfer data

### Flume vs Sqoop

| Aspect | Flume | Sqoop |
|--------|-------|-------|
| **Data source** | Streaming/event data (logs) | Relational databases |
| **Direction** | One-way (to Hadoop) | Two-way (import/export) |
| **Use case** | Log aggregation | DB migration |
| **Protocol** | Event-based | JDBC |

---

## 📌 Topic 7: Setting Up a Hadoop Cluster
> **🟡 Tier B | Frequency: 2/4**

### Cluster Specification
- **Master node**: NameNode + ResourceManager (more RAM — 64GB+)
- **Slave nodes**: DataNode + NodeManager (commodity — 16-32GB RAM, 12+ disks)
- **Network**: 1Gbps minimum, 10Gbps preferred between racks

### Hadoop Configuration Files

| File | Purpose |
|------|---------|
| `core-site.xml` | NameNode URI, temp directory |
| `hdfs-site.xml` | Replication factor, block size, DataNode dirs |
| `mapred-site.xml` | MapReduce framework (yarn/classic) |
| `yarn-site.xml` | ResourceManager host, NodeManager settings |
| `hadoop-env.sh` | JAVA_HOME and JVM settings |

### Cluster Setup Steps
1. Install Java (JDK 8+) on all nodes
2. Configure passwordless SSH between master and slaves
3. Set JAVA_HOME in hadoop-env.sh
4. Configure core-site.xml with NameNode URI
5. Configure hdfs-site.xml with DataNode directories
6. Configure yarn-site.xml with ResourceManager host
7. List slave hostnames in `slaves` file
8. Format NameNode: `hadoop namenode -format`
9. Start HDFS: `start-dfs.sh`
10. Start YARN: `start-yarn.sh`

### Master-Slave vs Peer-to-Peer Replication
> **🟡 Tier B | Frequency: 2/4**

| Aspect | Master-Slave | Peer-to-Peer |
|--------|-------------|-------------|
| **Control** | Master controls all writes | Any peer can write |
| **Consistency** | Strong (master is source of truth) | Eventual consistency |
| **Failure** | Master failure = system down | No single point of failure |
| **Example** | MySQL replication, HDFS NameNode | BitTorrent, Cassandra |
| **Read** | Can read from slave | Any peer |
| **Write** | Only to master, propagated to slaves | Any peer |

---

## 📌 Topic 8: Hadoop I/O — Compression & Serialization
> **🟢 Tier C | Low exam priority**

### Compression Codecs
| Codec | Extension | Splittable | Speed |
|-------|-----------|-----------|-------|
| Gzip | .gz | No | Medium |
| Bzip2 | .bz2 | Yes | Slow |
| LZO | .lzo | Yes (with index) | Fast |
| Snappy | .snappy | No | Very Fast |

### Avro
- Binary serialization format for Hadoop
- Schema stored with data (self-describing)
- Language-neutral, supports schema evolution
- Used in Kafka and Hadoop pipelines

---

## 📝 Section A Quick-Answer Bank (Unit 3)

| Question | Answer |
|----------|--------|
| Full form of HDFS | Hadoop Distributed File System |
| Block size of HDFS | 128 MB (default) |
| Two types of nodes in Hadoop | NameNode (master) and DataNode (slave) |
| What is heartbeat in HDFS? | Signal sent by DataNode to NameNode every 3 seconds to confirm it is alive. If NameNode misses 10 minutes of heartbeats, DataNode is marked dead. |
| What is data replication? | HDFS automatically creates 3 copies of each block across different DataNodes to ensure fault tolerance. |
| Flume vs Sqoop? | Flume: log data → HDFS. Sqoop: RDBMS ↔ HDFS |
| Block abstraction benefit? | Allows files larger than any single disk; enables parallelism |

---

## 🎯 Exam Answer Tips — Unit 3

- **HDFS read/write diagram is MANDATORY** for full marks — memorize both pipelines.
- For "Design of HDFS" question — draw master-slave architecture + explain NameNode, DataNode, Blocks.
- Benefits & Challenges: Write as two separate tables (5 benefits, 5 challenges).
- Cluster setup: Memorize the 4 config files and their purpose.
- HDFS is Unit 3's most asked topic — if you study only one thing from Unit 3, study read/write operations.
- **Heartbeat** is a common 2-mark question — memorize: every 3 seconds, 10 min timeout = dead.
