# Glossary — Key Terms

Use this page as a quick reference while going through the learning guide and lab.

If something feels confusing, check here first.

---

## Entity

A business concept in your ontology.

Think:
- table (in data)
- node (in graph)

Examples:
- ResaleTransaction  
- Location  
- SaleMonth  

---

## Property

A piece of data attached to an entity.

Think:
- column in a table  

Examples:
- resale_price  
- town  
- month_name  

---

## Relationship

A connection between two entities.

Think:
- join (in data)
- edge (in graph)

Example:
ResaleTransaction → located_at → Location  

Relationships define how data connects

---

## Key

A property that uniquely identifies each record.

Used to:
- define identity  
- enable relationships  

Examples:
- transaction_id  
- location_key  

---

## Binding

Linking the ontology to actual data.

Two types:

### Property binding
- maps a property → data column  

### Relationship binding
- maps keys between entities  

Without binding, the agent cannot access data

---

## Ontology

A structured layer that gives meaning to data.

It defines:
- entities  
- relationships  
- properties  

This is what allows the agent to understand your data

---

## Semantic Model

The layer that prepares data for analysis.

It defines:
- tables  
- relationships  
- data types  

The ontology is built on top of this

---

## Data Agent

The interface that allows users to ask questions in natural language.

It:
- interprets questions  
- generates queries  
- returns results  

---

## GQL (Graph Query Language)

A query language used to retrieve data based on relationships.

Instead of joins, it uses patterns  

---

## Aggregation

A summary calculation.

Examples:
- AVG (average)  
- COUNT  
- SUM  

---

## Row-level vs Aggregated Data

### Row-level
- individual records  
- e.g. transaction_id  

### Aggregated
- summaries  
- e.g. average resale price  

Mixing both incorrectly causes errors

---

## Case Sensitivity

Text must match exactly.

Example:
- TAMPINES ≠ Tampines  

Standardise values to avoid errors  

---

## Pattern (GQL)

A way of describing relationships in a query.

Example:
- MATCH (A)-[:REL]->(B)

You describe how things connect  
