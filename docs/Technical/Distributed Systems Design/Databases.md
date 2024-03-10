# Databases

A **database** is a structured collection of data stored through a computer system.

## Transactions and Operations

A **database transcation** is a logical unit of work performed on a datbaase and may consist of multiple **operations**. An **operation** is a smaller unit of work used to complete a transaction.

## Key Metrics

1. **Availability** - percentage of time the suers of the database can access it
2. **Reliability** - rate of data corruption, unsecure authorizaiton, and recoverabiity
3. **Persistence** - the data can be either written stably to non-volatile memory such as a hard drive or solid state drive (which will retain data even if it is not powered), or to volatile memory (e.g RAM) on in-memory databases that lack persistence.

## Database Model

Defining the data types used and stored within a system, the relationship between these types, and the way the data is organized through its attributes.

**Entity** is an object represented in a database, and a property of this entity is called an **attribute**. 

**Database schema** refers to the physical implementation of the database model to a specific database platform. The design of the data models is the same regardless of the databse platform or type. 

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

Attributes have types and expected sices specified. 

Also they may be:

- **Primary Key (PK)** - used to identify an entity
- **Foreign Key (PK)** - used to uniquely identify another entityand link two entities together
- **Composite Primary Key (CPK)** - a key that uses two or more attribtues to uniquely identify that entity (e.g. child entities that can be uniquely identified using other entities)

### Cardinality and Modality
**Cardinality** refers to the maximum number of elements of an entity that are associated with the elements in another entity. There are the following cardinalities:

- **One-to-One**
- **One-to-Many**
- **Many-to-Many**

**Modality** refers tot he minimum number of elements of an entitiy that are associated with elements in another entity.

## Relational vs. NoSQL/Non-Relational Databases

=== "Relational"
    Relational databse represents data in **tables** - each row being a record.

    The columns of the table can refer to data in other tables - creating relationships.

    Use cases:
    
    - OLTP: Online transaction processing
    - OLAP: Online analytical processing
    - Business intelligence and data warehouses
    - Logistics and inventory management
    - Financial services and payments

=== "NoSQL/Non-Relational"
    Don't store data in tables - a variety of data structures are used depending on the usecase. Additionally these databases don't have enforced relational models. 

    Examples are:

    - key-value
    - column-oriented
    - graph
    
    Use cases:

    - Real time analytics
    - Logging and click driven data
    - Fraud detection
    - Recommendations and news feeds
    - Internet of Things sensor data
    - High throughput messaging
    - Video and photo sharing

