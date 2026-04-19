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

Relationships are **directional**.
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

Instead of writing joins, you describe patterns:

```gql
MATCH (t:ResaleTransaction)-[:located_at]->(l:Location)
RETURN t, l

This means:
- find transactions
- connected to locations
