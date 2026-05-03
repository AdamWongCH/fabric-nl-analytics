# Graph Basics

This page introduces the basic graph concepts you need before working with ontology, GQL, and data agents in Microsoft Fabric.

You do **not** need to learn graph theory in depth.  
You only need to understand how data connects.

---

## Why This Matters

In this course, you are not just querying tables.

You are working with **connected data**.

This matters because many real-world questions are not only about individual records, but about how things are related:

- Which transactions happened in a location?
- Which month did a transaction occur in?
- Which flat type is associated with higher resale prices?
- How does a transaction connect to location, time, flat type, and lease?

This structure is what enables AI to reason over data.

---

## Conceptual Foundation

A graph represents data as **connected elements**.

In a graph:

- **nodes** represent things
- **edges** represent relationships
- **properties** describe nodes or edges

Microsoft Fabric Graph uses GQL, a graph query language for querying connected data using graph patterns. GQL is also an ISO-standardized language for property graphs.  
See: [`/docs/references.md`](./references.md)

---

## Core Concepts

### Entity (Node)

An entity represents a thing.

Think:

- table in a data model
- node in a graph
- business concept in an ontology

Examples:

- ResaleTransaction
- Location
- SaleMonth
- Flat
- Lease

In graph terms, these are the “things” we want to ask questions about.

---

### Relationship (Edge)

A relationship connects two entities.

Think:

- join in a data model
- edge in a graph
- meaningful connection in an ontology

Example:

ResaleTransaction → located_at → Location

Read it as:

> A resale transaction is located at a location.

This is more meaningful than just saying:

> fact_resale_transaction joins dim_location.

The relationship gives the connection a business meaning.

---

### Direction Matters

Relationships are **directional**.

Example:

ResaleTransaction → located_at → Location

This reads naturally as:

> A transaction is located at a location.

The reverse direction would read differently:

Location → located_at → ResaleTransaction

This is less natural.

Direction matters because it helps define the intended meaning of the relationship.

In this course, relationship names should read like simple English phrases.

---

### Property

A property is data attached to an entity or relationship.

Think:

- column in a table
- attribute of an entity
- value used for filtering, grouping, or aggregation

Examples:

- ResaleTransaction.resale_price
- Location.town
- SaleMonth.month_name
- Flat.flat_type

Properties are important because they allow the agent to answer questions such as:

- average resale price
- transactions in BEDOK
- price by month
- count by flat type

---

## Property Graphs

The graph model used here is closest to a **property graph**.

A property graph contains:

- nodes
- relationships
- labels
- properties

Example:

- Node label: Location
- Property: town = BEDOK
- Relationship: ResaleTransaction → located_at → Location

This is useful because it allows us to describe both:

1. what things exist  
2. how they are connected  

That is why graph structure is useful for natural language analytics.

---

## Labels

A label describes what type of thing a node or relationship is.

Examples:

- ResaleTransaction is a node label
- Location is a node label
- located_at is a relationship label

Labels help the system understand the role of each element in the graph.

For example:

ResaleTransaction → located_at → Location

This tells the system that:

- the starting node is a transaction
- the relationship is about location
- the target node is a location

---

## Thinking in Graphs vs Tables

### Table Thinking

In a traditional data model, you think in terms of:

- tables
- rows
- columns
- joins

Example:

> Join the transaction table to the location table using location_key.

---

### Graph Thinking

In a graph, you think in terms of:

- entities
- relationships
- properties
- patterns

Example:

> Find transactions that are located at a location.

You are querying **connections**, not just data.

---

## Why This Matters for Natural Language Analytics

Natural language questions usually sound like business questions, not database queries.

For example:

> What is the average resale price in Bedok?

A person understands this easily.

But the system must translate it into structure:

- Entity → ResaleTransaction
- Filter → Location.town = BEDOK
- Relationship → ResaleTransaction located_at Location
- Operation → AVG(resale_price)

The graph structure helps the system know how the concepts connect.

---

## Patterns in GQL

Instead of writing joins, you describe patterns.

Example of a basic GQL pattern:

<img width="253" height="41" alt="GQL basic pattern" src="https://github.com/user-attachments/assets/d29b8499-f201-4710-965c-82cf31f9e289" />

<br>

This means:

- find transactions
- connected to locations

The pattern shows the structure you want the query to follow.

---

## Basic Query Flow

Most graph queries follow this flow:

- MATCH → find a graph pattern
- FILTER → apply a condition
- RETURN → show the result

Example of a query with filter and aggregation:

<img width="255" height="35" alt="GQL query with filter and aggregation" src="https://github.com/user-attachments/assets/b7695149-ace3-48f1-b794-7b1c079de48d" />

<br>

Instead of manually joining tables, you describe **how things connect**.

---

## Traversal

Traversal means moving through relationships in the graph.

Example:

ResaleTransaction → located_at → Location

This means we can start from a transaction and move to its location.

Another example:

ResaleTransaction → sold_in → SaleMonth

This means we can start from a transaction and move to the month in which it was sold.

Traversal is important because many questions require the system to move from one concept to another.

Example question:

> What is the average resale price in Tampines in January?

To answer this, the system must traverse:

- ResaleTransaction → located_at → Location
- ResaleTransaction → sold_in → SaleMonth

This is why relationship design matters.

---

## Graph Schema and Ontology

A graph schema defines what kinds of nodes, relationships, and properties can exist.

An ontology does something similar, but with more business meaning.

In this course:

| Graph idea | Ontology idea |
|---|---|
| Node | Entity |
| Edge | Relationship |
| Property | Property |
| Graph schema | Ontology structure |

The ontology helps the system understand the business meaning of the graph.

For example:

ResaleTransaction → located_at → Location

is more meaningful than simply saying:

fact table joins dimension table.

---

## Is This a Knowledge Graph?

Yes — in this exercise, you are building a simple **knowledge graph**.

A knowledge graph connects concepts using meaningful relationships.

It consists of:

- entities
- relationships
- properties

Together, they form structured knowledge.

In this repo, the knowledge graph is simple:

- transactions connect to locations
- transactions connect to sale months
- transactions connect to flat types
- transactions connect to lease information

This is enough to support basic natural language analytics.

---

## What This Is Not

This is **not** a social network analysis course.

We are not focusing on:

- centrality
- community detection
- shortest paths
- graph algorithms

Instead, we focus on graph structure as a way to help an AI system understand and query data.

The goal is not graph theory.

The goal is:

> Structure that supports reasoning over data.

---

## Important Reminder

Graph structure alone is not enough.

You still need:

- correct keys
- correct relationships
- correct binding
- clear instructions

Structure must be correct for queries to work.

For example:

- if keys are wrong, relationships may not work
- if binding is wrong, the agent may not find the data
- if instructions are unclear, the agent may generate invalid queries

---

## Key Takeaways

- Entity = node
- Relationship = edge
- Property = data attached to an entity
- GQL queries patterns, not tables
- Relationships are directional
- Traversal means moving through relationships
- Ontology gives graph structure business meaning
- This is a simple knowledge graph for natural language analytics

Graph structure enables the system to understand connections.

Better structure leads to better answers.

---

## Further Reading

- [`/docs/references.md`](./references.md)

---

## Next Step

Proceed to the Learning Guide:

[`/docs/learning-guide.md`](./learning-guide.md)
