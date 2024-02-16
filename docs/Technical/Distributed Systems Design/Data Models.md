## Overview

==**Data Modeling** (how data is stored) and **Data Querying** (how data is retrieved) choices
go hand in hand.==

## Data Abstractions

![](https://scimos.com/wp-content/uploads/2020/07/ABSTRACTION-LAYERS-e1594137453801.png)

## Main Categories of Databases

### 1. Relational Database

When you have a relatively fixed structure and you know this structure is not going to
change too rapidly.

- Optimized for Transaction and Batch Processing (Read Throughput), Joins etc.
- Data Organized as tables/relations
- Object Relational Mapping Needed
- Oracle, MySQL, PostgreSQL

### 2. Document Database

Target use cases are where data comes in self-constrained documents and relationships
between one document and another are rare. Also the schema is easily evolved.

- NoSQL - or not only SQL
- Flexible schemas, better performance due to locality / high write throughput
- Mainly free and open source
- MongoDB, CouchDB, Espresso

### 3. Graph Database

Target use cases are where anything is potentially related to everything.

- Best suited for highly interconnected data - many to many relationships
- Social graphs, web graphs etc.
- Neo4j, SPARQL, Cypher
