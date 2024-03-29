# Databases

A **database** is a structured collection of data stored through a computer system.

## Transactions and Operations

A **database transaction** is a logical unit of work performed on a database and may consist of multiple **operations**. An **operation** is a smaller unit of work used to complete a transaction.

## Key Metrics

1. **Availability** - percentage of time the suers of the database can access it
2. **Reliability** - rate of data corruption, unsecure authorization, and recoverability
3. **Persistence** - the data can be either written stably to non-volatile memory such as a hard drive or solid state drive (which will retain data even if it is not powered), or to volatile memory (e.g RAM) on in-memory databases that lack persistence.

## Database Model

Defining the data types used and stored within a system, the relationship between these types, and the way the data is organized through its attributes.

**Entity** is an object represented in a database, and a property of this entity is called an **attribute**.

**Database schema** refers to the physical implementation of the database model to a specific database platform. The design of the data models is the same regardless of the database platform or type.

**ERDs** visualizes the relationships between entities and attributes.There are three main components: Entities, Attributes, and Relationships.

```json
// Courses
{
    "course_id": long,
    "created_at": timestamp,
    "title": string,
    "creator_id": long
}
// Creator
{
    "creator_id": long,
    "name": string
}

```

### Attributes and Relationships

Attributes have types and expected sizes specified.

Also they may be:

- **Primary Key (PK)** - used to identify an entity
- **Foreign Key (PK)** - used to uniquely identify another entity and link two entities together
- **Composite Primary Key (CPK)** - a key that uses two or more attributes to uniquely identify that entity (e.g. child entities that can be uniquely identified using other entities)

### Cardinality and Modality

**Cardinality** refers to the maximum number of elements of an entity that are associated with the elements in another entity. There are the following cardinalities:

- **One-to-One**
- **One-to-Many**
- **Many-to-Many**

**Modality** refers tot he minimum number of elements of an entity that are associated with elements in another entity.

## Relational vs. NoSQL/Non-Relational Databases

| Feature               | SQL Databases                                          | NoSQL Databases                                                |
| --------------------- | ------------------------------------------------------ | -------------------------------------------------------------- |
| **Data Model**        | Organized into structured tables, relational and normalized data                       | Flexible schema, supports various data models                  |
| **Query Language**    | SQL (Structured Query Language)                        | Custom query languages or APIs (e.g., MongoDB Query Language)  |
| **Consistency Model** | ACID-compliant transactions                            | Basically Available, Soft state, Eventually consistent (BASE)  |
| **Scalability**       | Generally vertical scaling, limited horizontal scaling | Horizontal scaling, distributed architectures                  |
| **Use Cases**         | Enterprise applications, OLTP, data warehousing, OLAP  | Real-time analytics, web applications, IoT, content management |
| **Typical Index Datastructure** | B-Trees, better for reads than writes | LSM Tree and SSTables, for fast writes |
| **Examples**          | MySQL, PostgreSQL, SQL Server                          | MongoDB, Cassandra, Redis, Elasticsearch, Neo4j                |
| **Challenges**        | Schema evolution, performance tuning, may require two phase commit                   | Data consistency, schema design, tooling ecosystem             |

### Storage Engines / Data Structures

Two families of storage engines used by databases:

- **Log structured** - LSM-Trees e.g. SSTables -> HBASE, Cassandra -> write optimized
- **Page-Oriented** - B-trees -> Traditional RDBMS -> read optimized

These are answers to limitations of disk access.

### Relational Database Details

#### Types

There are two main categories of databases - OLTP (Online Transaction Processing Database)
and OLAP (Online Analytical Processing Database) each with a different read pattern,
write patterns, user using it, data size etc.

1. OLTP - Online Transaction Processing Database optimized for latency.
   - eg. MySQL
   - Usually row-order store
     - easy to modify/add a record
     - might read in unnecessary data
2. OLAP - Online Analytical Processing Databases optimized for data crunching.
   - Data Warehousing (Star/Snowflake schema), column oriented
   - Column compression, data cubes, optimized for reads/queries
   - Materialized views, lack of flexibility
   - HBase, Hive, Spark
   - Usually column-order store
     - Only need to read in relevant data
     - Tuple writes require multiple accesses
     - Suitable for read-mostly, read-intensive, large data repositories


### NoSQL Database Details

#### Key Characteristics

- Flexible Schema: NoSQL databases typically support schema-less or flexible schema designs, allowing for dynamic and varied data structures within the same database.
- Scalability: NoSQL databases are designed to scale horizontally, allowing for distributed architectures and seamless expansion across multiple nodes or clusters.
- High Performance: NoSQL databases are optimized for specific use cases, such as high-volume read/write operations, real-time analytics, and large-scale data processing.
- Diverse Data Models: NoSQL databases support various data models, including document-oriented, key-value, wide-column, and graph databases, catering to different data storage and retrieval requirements.
- CAP Theorem: NoSQL databases often prioritize availability and partition tolerance over strict consistency, adhering to the principles outlined in the CAP theorem.

#### Types of NoSQL Databases

##### Document-oriented Databases
Store data in flexible, JSON-like documents, allowing for nested structures and dynamic schemas. Nested documents allow for embedded schemas for data locality, and is denormalized.

Examples include MongoDB, Couchbase, and Google Firestore.

They are suitable for unstructured catalog data, and complex structured hierarchical data. For example in e-commerce applications a product can have thousands of attributes, and content management applications like blogs and video platforms.

##### Key-Value Stores
Store data as key-value pairs using methods like hash tables, providing fast access to individual records. 

Examples include Redis, Amazon DynamoDB.

They are efficient for session-oriented applications such as user session data/profile information, recommendations, discounts, promotions etc. 

##### Wide-column Stores (Column-family Databases)
Store data in columns rather than rows, allowing for efficient storage and retrieval of large datasets. The store has shard key and sort key, allows for flexible schemas and easy partitioning.

Examples include Apache Cassandra, HBase.

Efficient for large number of aggregation and data anlytics queries. They reduce the disk I/O requirements drastically. Examples may be applications related to financial institutions that need to sum financial transaction over a period of time. 

##### Graph Databases
Store data in graph structures, representing relationships between entities as nodes and edges. Hence we are able to store the data once and interpret it differently based on the relationships. 

Examples include Neo4j, Amazon Neptune.

Can be used in social applications, provide interesting facts and figures among different kinds of users and activities. 

## Indexing

- **Purpose**: Indexing is a technique used in databases to optimize query performance by creating data structures that allow for fast data retrieval.
- **Why it's Used**: Indexing improves query performance by providing efficient access paths to data, reducing the time required to search and retrieve data from tables.

**Types of Indices**:

- **B-Tree Index**: Widely used for range queries, ordered traversal, and equality searches in both SQL and NoSQL databases.
- **Hash-based Index**: Provides fast lookups for equality searches, commonly used in SQL databases.
- **Bitmap Index**: Efficient for low-cardinality columns, suitable for boolean and categorical data. Not commonly used in SQL databases, but may be utilized in some NoSQL databases.
- **Full-Text Index**: Optimized for searching textual data, supports complex text searches and relevance ranking. Used in some SQL databases (e.g., MySQL, PostgreSQL) and NoSQL databases (e.g., Elasticsearch, MongoDB with text search).
- **Spatial Index**: Designed for indexing geometric data, enables efficient spatial queries and joins. Used in SQL databases supporting geospatial data types (e.g., PostGIS in PostgreSQL) and some NoSQL databases (e.g., MongoDB with geospatial queries).

Additionally you would also sometimes define a secondary index for more read optimization. You can have local secondary indices (usual default) or global secondary indices (for cases where the service is read heavy for a range of queries).