# Unit 1 — Introduction to Big Data
> 🔴 All topics below are **Tier A** (appeared 3–4/4 years) unless marked 🟡

---

## 📌 Topic 1: Types of Digital Data
> **Tier A | Frequency: 3/4 | Appears in Section A + C**

### Definition
Digital data is information stored in binary format. In Big Data context, it is classified based on structure into three types.

### Types of Digital Data

| Type | Description | Examples |
|------|-------------|---------|
| **Structured** | Data with predefined schema and format, stored in rows/columns | RDBMS tables, CSV, Excel spreadsheets |
| **Semi-Structured** | No rigid schema but has tags/markers to separate fields | XML, JSON, HTML, log files, emails |
| **Unstructured** | No predefined format, no inherent structure | Text documents, images, audio, video, social media posts |

### Key Points for Exam
- Structured data = 20% of all data; Unstructured = 80%
- Big Data is mainly unstructured (images, videos, sensor data)
- Semi-structured is the bridge — has organizational properties but not strict schema
- Example: A tweet is semi-structured (JSON metadata) but the tweet text is unstructured

### Exam Answer Tip
- 2-mark question: Name types + one example each
- 10/7-mark question: Write a full table + explain each with 3-4 sentences + real-world examples

---

## 📌 Topic 2: Big Data Platforms
> **🔴 Tier A | Frequency: 4/4 | Appears EVERY year in Section A**

### Definition
A Big Data platform is an integrated set of technologies for storing, processing, managing, and analyzing large-scale datasets that exceed the capacity of traditional systems.

### Top 5 Big Data Platforms (Memorize these)

| Platform | Key Feature | Use Case |
|----------|------------|---------|
| **Apache Hadoop** | Distributed storage (HDFS) + processing (MapReduce) | Batch processing of large datasets |
| **Apache Spark** | In-memory processing, 100x faster than MapReduce | Real-time analytics, ML pipelines |
| **Apache Kafka** | Distributed streaming platform | Real-time data pipelines, event streaming |
| **MongoDB** | Document-oriented NoSQL database | Flexible schema applications, e-commerce |
| **Apache Cassandra** | Column-family NoSQL, high availability | IoT data, time-series, social networks |
| **Google BigQuery** | Serverless cloud data warehouse | Ad-hoc large-scale SQL queries |
| **Amazon EMR** | Managed Hadoop/Spark on AWS | Cloud-based big data processing |
| **Apache Flink** | Stream processing framework | Low-latency event-driven applications |

### Exam Answer Tip
- Section A: List ANY 5 with one-line description — 2 marks
- Section B/C: Explain 5 platforms with their use cases and key differentiators

---

## 📌 Topic 3: Big Data Architecture & Components
> **🔴 Tier A | Frequency: 3/4 | Appears in Section B & C**

### Definition
Big Data architecture defines how data is collected, stored, processed, and analyzed at massive scale. It is a layered pipeline from raw data to actionable insights.

### Architecture Diagram
![Big Data Architecture](diagrams/bigdata_architecture.svg)

### Five Layers of Big Data Architecture

**1. Data Sources Layer**
Raw data comes from: social media, IoT sensors, transaction systems, logs, clickstreams, external databases.

**2. Ingestion Layer**
Tools that bring data into the system:
- **Apache Flume**: Collects log data from servers
- **Apache Sqoop**: Transfers data between RDBMS and Hadoop
- **Apache Kafka**: Real-time streaming ingestion
- **NiFi**: Automated data flow management

**3. Storage Layer**
- **HDFS**: Distributed file storage for large files
- **HBase**: NoSQL column store on top of HDFS (real-time access)
- **Amazon S3 / Azure Blob**: Cloud object storage

**4. Processing Layer**
- **Batch**: MapReduce (slow but robust), Apache Spark (fast)
- **Stream**: Apache Storm, Spark Streaming, Kafka Streams

**5. Analytics & Visualization Layer**
- Hive/Pig for querying, Tableau/Power BI for dashboards
- Machine Learning models for predictive analytics

### Components of Big Data Technology

| Component | Role |
|-----------|------|
| YARN | Resource management across cluster |
| ZooKeeper | Distributed coordination service |
| Oozie | Workflow scheduler for Hadoop jobs |
| Ambari | Cluster monitoring and management |
| Knox | Security gateway |
| Ranger | Fine-grained authorization |

### Exam Answer Tip
- Draw the pipeline diagram (Sources → Ingestion → Storage → Processing → Analytics)
- List tools at each layer
- Section C: 10/7 marks — expect 5-6 paragraphs + diagram

---

## 📌 Topic 4: The 5 Vs of Big Data
> **🔴 Tier A | Frequency: 3/4 (asked in Section C Q3 both 2023-24 and 2024-25)**

### Definition
The 5 Vs are the defining characteristics that distinguish Big Data from conventional data.

### 5 Vs Diagram
![5 Vs of Big Data](diagrams/5vs_bigdata.svg)

### Detailed Explanation

| V | Meaning | Example | Challenge |
|---|---------|---------|-----------|
| **Volume** | Massive scale of data generated (TB, PB, ZB) | Facebook generates 4PB/day | Storage and processing at scale |
| **Velocity** | Speed at which data is generated and processed | Twitter: 6,000 tweets/second | Real-time processing requirements |
| **Variety** | Different forms: structured, semi, unstructured | Sensor data + logs + videos | Integration and normalization |
| **Veracity** | Trustworthiness and accuracy of data | Social media has noise, errors | Data cleaning and validation |
| **Value** | Business worth extracted from data | Customer behavior prediction | ROI justification, analytics cost |

### Extended Vs (sometimes asked)
- **Variability**: Inconsistent data flows (seasonal spikes)
- **Visualization**: Challenge of representing huge datasets
- **Validity**: Data correctness for intended use

### Exam Answer Tip
- Always draw or mention the 5Vs diagram/wheel
- Give one real-world example for each V
- 7-mark answer: 1 para per V + a table

---

## 📌 Topic 5: Analysis vs. Reporting
> **🔴 Tier A | Frequency: 3/4 | Asked in Section B and C**

### Definition
**Reporting** presents historical data in a structured format to answer "What happened?" **Analysis** digs deeper to answer "Why did it happen?" and "What will happen?"

### Comparison Table

| Aspect | Reporting | Analysis |
|--------|-----------|---------|
| **Purpose** | Describe past events | Understand causes and predict future |
| **Question** | What happened? | Why did it happen? What will happen? |
| **Output** | Dashboards, summary tables | Insights, models, recommendations |
| **Type** | Descriptive | Diagnostic, Predictive, Prescriptive |
| **User** | Managers, executives | Data scientists, analysts |
| **Tools** | Excel, Crystal Reports, Cognos | Python, R, Spark MLlib, Hadoop |
| **Data** | Structured, historical | Structured + unstructured, real-time |
| **Complexity** | Low | High |

### Types of Analytics
1. **Descriptive Analytics**: What happened? (Reporting)
2. **Diagnostic Analytics**: Why did it happen?
3. **Predictive Analytics**: What might happen?
4. **Prescriptive Analytics**: What should we do?

### Exam Answer Tip
- Table is mandatory for this question
- Then explain each type of analytics
- Conclude with: "Big Data moves organizations from reporting to advanced analytics"

---

## 📌 Topic 6: Big Data Features — Security, Privacy & Ethics
> **🟡 Tier B | Frequency: 2/4**

### Key Security Features
- **Authentication**: Kerberos-based user verification
- **Authorization**: Apache Ranger for role-based access
- **Auditing**: All access logged, tracked (compliance)
- **Encryption**: Data encrypted at rest (AES) and in transit (SSL/TLS)

### Privacy and Ethics Concerns
- **Data anonymization**: Remove personally identifiable info
- **Right to be forgotten** (GDPR): Users can request data deletion
- **Consent**: Users must agree to data collection
- **Bias in algorithms**: Skewed training data causes unfair decisions
- **Surveillance risks**: Location, behavioral tracking misuse

### Big Data Applications (Real-world)
> 🟡 Tier B | Freq 2/4

| Industry | Application |
|----------|------------|
| Healthcare | Disease prediction, genomics, patient monitoring |
| Finance | Fraud detection, credit scoring, algorithmic trading |
| E-Commerce | Recommendation engines (Netflix, Amazon) |
| Transportation | Route optimization, predictive maintenance |
| Government | Smart cities, crime prediction, election analysis |

---

## 📝 Section A Quick-Answer Bank (Unit 1)

| Question | Answer (2 marks) |
|----------|-----------------|
| List 5 Big Data platforms | Hadoop, Spark, Kafka, MongoDB, Cassandra |
| What are 5 Vs? | Volume, Velocity, Variety, Veracity, Value |
| What is structured data? | Data in rows/columns with predefined schema. E.g., RDBMS tables |
| What is Big Data analytics? | Process of examining large datasets to uncover patterns, correlations, market trends |
| Name Big Data architecture layers | Ingestion → Storage → Processing → Analytics → Visualization |

---

## 🎯 Exam Answer Tips — Unit 1

- **Section A (2 marks):** 2-3 lines max. Don't over-explain.
- **Section B (10/7 marks):** Introduction + explanation of each point + table/list + conclusion. ~1 page.
- **Section C (10/7 marks):** Full-length answer. Draw diagram. Give real-world examples. ~1.5 pages.
- **Diagram mandatory:** Big Data Architecture pipeline — always draw for Section C.
- **5Vs question:** Draw the wheel diagram, then explain each V with an industry example.
