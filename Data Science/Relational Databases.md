#data_science #computer_science #math #database 

[[Graph Databases vs. Relational Databases]]

Here’s a concise yet thorough overview of **relational databases**, tailored for an expert audience:

A **relational database** is a digital data store built on the **relational model**, where information is organised into **relations** (tables) of **tuples** (rows) and **attributes** (columns). Each table represents an entity type, rows capture individual entity instances keyed by unique identifiers, and columns hold the properties of those instances. Data integrity and consistency are enforced via **ACID** transactions and **schema constraints**, while **SQL** provides a declarative interface for querying and updating the dataset.

---

## Definition and Core Principles

A relational database (RDB) adheres to the relational model first formalised by E. F. Codd in 1970. In this model:

- **Relations (Tables)** are collections of tuples sharing the same attributes.
    
- **Tuples (Rows)** represent individual records, each uniquely identified by a **primary key**.
    
- **Attributes (Columns)** define the domains and data types of the stored values. ([Wikipedia](https://en.wikipedia.org/wiki/Relational_database?utm_source=chatgpt.com "Relational database"))
    

A **Relational Database Management System (RDBMS)** is software that implements the relational model, providing storage, transaction management, security, and a query engine—most commonly via SQL ([oracle.com](https://www.oracle.com/database/what-is-a-relational-database/?utm_source=chatgpt.com "What Is a Relational Database? (RDBMS)?")).

---

## Data Integrity & Transactions

Relational databases guarantee **ACID** properties:

1. **Atomicity**: All operations within a transaction succeed or none are applied.
    
2. **Consistency**: Transactions move the database from one valid state to another, respecting all schema constraints (e.g., primary/foreign keys, check constraints).
    
3. **Isolation**: Concurrent transactions do not interfere, appearing to execute serially.
    
4. **Durability**: Once committed, transaction effects persist despite failures. ([Wikipedia](https://en.wikipedia.org/wiki/Relational_database?utm_source=chatgpt.com "Relational database"))
    

Constraints such as **entity integrity** (non-null primary keys) and **referential integrity** (valid foreign-key references) prevent anomalies and maintain data correctness.

---

## Schema & Normalization

Relational schemas define table structures and relationships. **Normalization** decomposes tables to eliminate redundancy and update anomalies:

- **1NF**: Atomic attribute values.
    
- **2NF–3NF**: Eliminate partial and transitive dependencies.
    
- **BCNF+**: Strengthened uniqueness constraints. ([Wikipedia](https://en.wikipedia.org/wiki/Relational_database?utm_source=chatgpt.com "Relational database"))
    

Well-normalised schemas aid consistency but may incur join overhead, sometimes prompting **denormalisation** for performance-critical read workloads.

---

## Querying with SQL

**Structured Query Language (SQL)** is the de facto standard for relational data operations:

- **Data Definition Language (DDL)**: `CREATE`, `ALTER`, `DROP` tables and constraints.
    
- **Data Manipulation Language (DML)**: `SELECT`, `INSERT`, `UPDATE`, `DELETE` rows.
    
- **Transaction Control**: `BEGIN`, `COMMIT`, `ROLLBACK`, isolation level hints.
    

Optimised query planners use indexes (B-trees, hash), cost-based execution plans, and join algorithms (nested loop, hash join) to satisfy complex queries efficiently.

---

## Use Cases & Ecosystem

### Common Applications

- **Transactional systems**: ERP, CRM, financial ledgers
    
- **Operational reporting**: Inventory management, order processing
    
- **General-purpose storage**: Logging, user data, metadata ([oracle.com](https://www.oracle.com/database/what-is-a-relational-database/?utm_source=chatgpt.com "What Is a Relational Database? (RDBMS)?"))
    

### Leading RDBMS Platforms

- **Oracle Database** (enterprise-grade features, strong ACID) ([oracle.com](https://www.oracle.com/database/what-is-a-relational-database/?utm_source=chatgpt.com "What Is a Relational Database? (RDBMS)?"))
    
- **MySQL / MariaDB** (open-source, widespread web adoption)
    
- **PostgreSQL** (advanced SQL features, extensibility)
    
- **Microsoft SQL Server** (tight .NET integration, BI tools)
    

---

## Historical Context

- **1970**: Codd’s seminal paper introduces the relational model.
    
- **1976–1979**: First commercial RDBMS products appear (e.g., Multics RDS, Oracle).
    
- **1980s–1990s**: SQL standardisation and widespread adoption in enterprise systems.
    
- **2000s–present**: Evolution into distributed SQL (NewSQL), cloud-managed RDS offerings.
    

---

Relational databases remain foundational for structured, consistent, transactional data management across virtually all industries. Their maturity, tooling, and well-understood performance characteristics make them the default choice for high-integrity workloads.