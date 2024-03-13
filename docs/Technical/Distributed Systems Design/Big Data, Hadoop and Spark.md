# Big Data, Hadoop and Spark

## Big Data
Big data refers to extremely large and complex datasets that traditional data processing applications are inadequate to handle efficiently. It encompasses various types of data, including structured, unstructured, and semi-structured data, and presents challenges related to storage, processing, and analysis.

### Types of Big Data

1. **Structured Data:**
   - **Definition:** Data organized in a predefined format with a well-defined schema.
   - **Examples:** Relational databases, Excel spreadsheets, CSV files.
   
2. **Unstructured Data:**
   - **Definition:** Data that does not have a predefined data model or structure.
   - **Examples:** Text documents, images, videos, social media posts.

3. **Semi-Structured Data:**
   - **Definition:** Data that does not fit neatly into a structured format but contains some organizational properties.
   - **Examples:** JSON, XML, log files.

### Storage Solutions for Big Data

1. **Data Warehouses:**
   - **Description:** Centralized repositories for structured data collected from various sources within an organization.
   - **Characteristics:** Optimized for query and analysis, typically using SQL-based querying languages.
   - **Examples:** Amazon Redshift, Google BigQuery, Snowflake.

2. **Data Lakes:**
   - **Description:** Storage systems that can store vast amounts of structured, semi-structured, and unstructured data in its raw format.
   - **Characteristics:** Designed for storing and processing raw data without the need for a predefined schema.
   - **Examples:** Amazon S3, Hadoop Distributed File System (HDFS), Azure Data Lake Storage.

3. **Databases:**
   - **Description:** Software systems for storing, managing, and retrieving data efficiently.
   - **Characteristics:** Can handle structured, semi-structured, and sometimes unstructured data depending on the database type.
   - **Examples:** MongoDB (NoSQL for semi-structured and unstructured data), Cassandra (NoSQL for time-series data), MySQL (Structured data).

4. **NoSQL Databases:**
   - **Description:** Databases designed for handling unstructured or semi-structured data at scale.
   - **Characteristics:** Schema-less or flexible schema design, horizontal scalability.
   - **Examples:** MongoDB, Cassandra, Couchbase.

5. **Object Storage:**
   - **Description:** Storage architecture that manages data as objects rather than files or blocks.
   - **Characteristics:** Scalability, durability, and flexibility in handling various types of data.
   - **Examples:** Amazon S3, Google Cloud Storage, Azure Blob Storage.

## Hadoop

Hadoop is an open-source distributed processing framework for big data workloads. It consists of two main components:

### Hadoop Distributed File System (HDFS)

- **Architecture**:

   - **NameNode**: Manages the file system metadata and coordinates file operations.
   - **DataNodes**: Stores the actual data blocks and serves read/write requests.
   - Files are split into blocks (default 128MB) and replicated across multiple DataNodes for fault tolerance.

- **Features**:
   - Highly fault-tolerant through data replication and automatic failover.
   - Suitable for batch processing of large datasets.
   - Supports high throughput data access patterns.

### MapReduce

- **Architecture**:
      - **JobTracker**: Manages and schedules jobs on the cluster.
      - **TaskTrackers**: Run individual Map and Reduce tasks on worker nodes.

- **Advantages**:
      - Suitable for batch processing of large datasets.
      - Scalable and fault-tolerant through task parallelization and automatic retries.

- **Limitations**:
      - High latency due to disk I/O and batch processing nature.
      - Not suitable for real-time or iterative workloads.

#### Workflow

The MapReduce programming model follows a specific workflow to process large datasets in parallel across a cluster of machines:

1. **Input Data Splitting**:
      - The input data (e.g., files from HDFS) is split into fixed-size chunks called `input splits` (typically 128MB or 256MB).
      - Each input split is processed by an individual `map` task.

2. **Map Phase**:
      - The `map` tasks run in parallel across the cluster nodes.
      - Each map task processes its input split by applying the user-defined `map` function.
      - The map function takes key-value pairs as input and emits intermediate key-value pairs.

3. **Shuffle and Sort**:
      - The intermediate key-value pairs produced by the map tasks are shuffled and sorted by their keys.
      - This process involves transferring data across the network to ensure that all values with the same key are grouped together.
      - The shuffling is typically performed using HTTP or specialized shuffling services.

4. **Reduce Phase**:
      - The `reduce` tasks run in parallel across the cluster nodes.
      - Each reduce task processes a bucket of intermediate key-value pairs with the same key.
      - The reduce task applies the user-defined `reduce` function to the grouped values, producing the final output key-value pairs.

5. **Output Writing**:
      - The final output key-value pairs from the reduce tasks are written to the desired output location (e.g., HDFS, database, or other storage systems).

6. **Job Tracking and Fault Tolerance**:
      - The `JobTracker` (in Hadoop 1.x) or `ResourceManager` (in Hadoop 2.x) manages and monitors the execution of MapReduce jobs.
      - If a task fails, it can be automatically restarted on another node, leveraging the fault tolerance capabilities of the framework.

7. **Combiner (Optional)**:
      - A `combiner` function can be used to perform partial aggregation or filtering on the intermediate key-value pairs before the shuffle phase.
      - This can reduce the amount of data transferred across the network and improve performance.

## Apache Spark

Apache Spark is a fast and general-purpose cluster computing system for big data analytics. It offers several advantages over Hadoop MapReduce:

### Resilient Distributed Datasets (RDDs)

- Immutable, partitioned collections of records cached in memory for faster processing.
- Fault-tolerant through lineage and automatic recomputation of lost partitions.

### In-Memory Processing

- Spark processes data in memory, avoiding expensive disk I/O operations.
- Suitable for iterative algorithms and real-time streaming workloads.

### Spark Components

- **Spark Core**: Provides RDDs and parallel processing capabilities.
- **Spark SQL**: Structured data processing with SQL-like queries.
- **Spark Streaming**: Real-time streaming data processing.
- **MLlib**: Machine learning algorithms and utilities.
- **GraphX**: Graph processing and analysis.

### Advantages over Hadoop MapReduce

- Faster processing due to in-memory computation.
- Supports iterative and interactive workloads.
- Integrated libraries for SQL, streaming, machine learning, and graph processing.
- Simplified programming model with RDDs and high-level APIs.

### Deployment Modes

- **Standalone Mode**: Spark manages its own cluster resources.
- **YARN Mode**: Spark runs on top of Hadoop YARN for resource management.
- **Kubernetes Mode**: Spark runs on Kubernetes for container orchestration.

### Spark Architecture

- **Driver**: Coordinates the execution of Spark applications.
- **Executors**: Run tasks and cache data on worker nodes.
- **Cluster Manager**: Allocates resources (e.g., YARN, Kubernetes, Mesos).

Hadoop and Spark are complementary technologies, with Hadoop providing a robust distributed file system and batch processing capabilities, while Spark offers faster in-memory processing and advanced analytics capabilities.
