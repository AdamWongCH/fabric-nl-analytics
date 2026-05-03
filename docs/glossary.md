# Glossary — Key Terms

Use this page as a quick reference while going through the learning guide and lab.

If something feels confusing, check here first.

---

## Entity

A business concept in your ontology.

Think:
- table in a data model
- node in a graph

Examples:
- ResaleTransaction  
- Location  
- SaleMonth  

---

## Property

A piece of data attached to an entity.

Think:
- column in a table
- attribute of an entity

Examples:
- resale_price  
- town  
- month_name  

---

## Relationship

A connection between two entities.

Think:
- join in a data model
- edge in a graph

Example:

    ResaleTransaction → located_at → Location

Relationships define how data connects.

---

## Key

A property that uniquely identifies each record in an entity.

Used to:
- define identity  
- enable relationships  
- support correct joins or traversal  

Examples:
- transaction_id  
- location_key  
- date_key  

---

## Binding

Linking the ontology to actual data.

There are two important types of binding:

### Property binding

Maps an ontology property to a data column.

Example:

    resale_price → fact_resale_transaction.resale_price

### Relationship binding

Maps keys between entities so relationships can work.

Example:

    ResaleTransaction.location_key = Location.location_key

Without binding, the ontology may describe the data, but the agent cannot reliably query the actual data.

---

## Ontology

A structured layer that gives meaning to data.

It defines:
- entities  
- relationships  
- properties  
- keys  
- bindings  

This is what allows the agent to understand what the data means and how concepts connect.

---

## Semantic Model

The analytical layer that prepares data for reporting and analysis.

It defines:
- tables  
- relationships  
- data types  
- measures or aggregations  

The ontology is built on top of this structure.

---

## Data Agent

The natural language interface that allows users to ask questions about data.

It:
- interprets questions  
- generates queries  
- returns results  

A data agent depends on the ontology and instructions to answer questions correctly.

---

## Instructions

Rules and guidance given to the data agent.

Instructions help define:
- how business terms are mapped  
- how values should be normalized  
- how aggregations should be handled  
- what the agent should avoid  

Example:

    price → resale_price
    Tampines → TAMPINES
    average price → AVG(resale_price)

Ontology defines structure.  
Instructions define behaviour.

---

## GQL (Graph Query Language)

A query language used to retrieve data based on graph relationships.

Instead of joining tables manually, GQL describes patterns.

Example:

    MATCH (A)-[:REL]->(B)

---

## Pattern (GQL)

A way of describing relationships in a graph query.

Example:

    MATCH (t:ResaleTransaction)-[:located_at]->(l:Location)

This means:
- find a resale transaction
- follow the located_at relationship
- reach a location

---

## NL2GQL

Natural Language to Graph Query Language.

This refers to the process of translating a user question into a GQL query.

Example:

    "average resale price in Bedok"
    → identify entity: ResaleTransaction
    → identify filter: Location.town = BEDOK
    → identify operation: AVG(resale_price)

NL2GQL works better when the ontology, relationships, bindings, and instructions are clear.

---

## Aggregation

A summary calculation.

Examples:
- AVG (average)  
- COUNT  
- SUM  

Used when questions ask for summaries, such as:
- average resale price
- number of transactions
- total resale value

---

## Row-level vs Aggregated Data

### Row-level

Individual records.

Example:
- transaction_id  
- resale_price for one transaction  

### Aggregated

Summaries across many records.

Example:
- average resale price  
- count of transactions  

Mixing row-level fields with aggregated results incorrectly can cause errors.

Example:

    transaction_id + AVG(resale_price)

This may cause a GROUP BY error.

---

## Case Sensitivity

Text must match exactly.

Example:
- TAMPINES ≠ Tampines  

If the data stores town names in uppercase, the query may also need uppercase values.

Standardise values to avoid errors.

---

## Traversal

Moving from one entity to another through relationships.

Example:

    ResaleTransaction → located_at → Location

A question like:

    average resale price in Bedok

requires the system to move from `ResaleTransaction` to `Location`.

---

## GROUP BY Error

An error that can occur when a query mixes row-level fields with aggregated values.

Example problem:

    transaction_id + AVG(resale_price)

Why it fails:
- `transaction_id` is row-level
- `AVG(resale_price)` is aggregated

Fix:
- for summary questions, return only grouped fields and aggregated measures

---

## Key Idea

Structure enables AI to reason over data.

Better structure leads to better answers.
