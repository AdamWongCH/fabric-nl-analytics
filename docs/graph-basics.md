# Graph Basics

This page introduces the **minimum graph concepts** you need.

You do NOT need graph theory  
You only need to understand how data connects  

---

## Why This Matters

In this course, you are not just querying tables.

You are working with **connected data**.

This structure is what enables AI to reason over data.

---

## Core Concepts

### Entity (Node)

An entity represents a thing.

Think:
- table (in data)
- node (in graph)

Examples:
- ResaleTransaction  
- Location  
- SaleMonth  

---

### Relationship (Edge)

A relationship connects two entities.

Think:
- join (in data)
- edge (in graph)

Example:

ResaleTransaction → located_at → Location

Read it as:  
“A transaction is located at a location”

---

### Direction Matters

Relationships are **directional**
ResaleTransaction → Location

NOT:
Location → ResaleTransaction

Direction helps define meaning

---

### Property

A property is data attached to an entity.

Think:
- column in a table  

Examples:
- resale_price  
- town  
- month_name  

---

## Thinking in Graphs vs Tables

### Table Thinking

- tables  
- joins  
- rows  
- columns  

---

### Graph Thinking

- entities  
- relationships  
- patterns  

You are querying **connections**, not just data

---

## Patterns in GQL

Instead of writing joins, you describe patterns.

Example of a basic GQL pattern:

<img width="253" height="41" alt="image" src="https://github.com/user-attachments/assets/d29b8499-f201-4710-965c-82cf31f9e289" />
<br>
This means:

- find transactions  
- connected to locations  

---

## Basic Query Flow

Most queries follow this flow:

- MATCH → find pattern  
- FILTER → apply condition  
- RETURN → show result  

---

Example of a query with filter and aggregation:

<img width="255" height="35" alt="image" src="https://github.com/user-attachments/assets/b7695149-ace3-48f1-b794-7b1c079de48d" />
<br>
Instead of joins, you describe **how things connect**

---

## Is This a Knowledge Graph?

Yes — in this exercise, you are building a simple **knowledge graph**.

It consists of:

- entities (things)  
- relationships (connections)  
- properties (data)  

Together, they form structured knowledge

---

## Important Reminder

Graph structure alone is not enough.

You still need:

- correct keys  
- correct relationships  
- correct binding  
- clear instructions  

Structure must be correct for queries to work

---

## Key Takeaways

- Entity = node  
- Relationship = connection  
- Property = data  
- GQL queries patterns, not tables  
- Relationships are directional  

Graph structure enables the system to understand connections

---

## Next Step

Proceed to the Learning Guide  
[`/docs/learning-guide.md`](./learning-guide.md)
