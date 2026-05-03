# Learning Guide — How to Think

This guide explains how to think about ontology and data agents.

You are not just following steps.  
You are learning how data is structured so AI can reason over it.

---

## Big Picture

    Data → Semantic Model → Ontology → Data Agent → Answer

- **Data** = raw tables  
- **Semantic Model** = analytical structure  
- **Ontology** = meaning layer  
- **Data Agent** = natural language interface  

Better structure leads to better answers.

---

## Why Structure Matters

AI does not understand data the way humans do.

It relies on structure:

- **entities** → what things are  
- **relationships** → how things connect  
- **properties** → what we know about them  

This structure enables the system to interpret questions and generate meaningful queries.

In this repo, the key idea is:

> Structure enables AI to reason over data.

---

## From Data Model to Ontology

You are translating a data model into meaning.

| Data Model | Ontology |
|-----------|----------|
| Table | Entity |
| Column | Property |
| Join | Relationship |

---

### Example

| Table | Entity |
|------|--------|
| fact_resale_transaction | ResaleTransaction |
| dim_location | Location |
| dim_date | SaleMonth |

A table such as `fact_resale_transaction` becomes more meaningful when it is represented as a business entity: `ResaleTransaction`.

---

## Taxonomy vs Data Model vs Ontology

These concepts are related, but they serve different purposes.

---

### Taxonomy — Hierarchy

A taxonomy organises concepts into categories.

Example:

    Flat Type → 3-Room → 4-Room → 5-Room

Good for:
- organising categories  
- simple navigation  

Less suitable for:
- querying complex relationships  
- supporting natural language analytics  

---

### Data Model — Tables

A data model structures data into tables and joins.

Good for:
- storing data  
- SQL queries  
- analytical modelling  

Limitation:
- meaning is often technical rather than explicit  

For example, a database may know that `location_key` connects two tables, but it does not automatically know that this means a transaction is **located at** a location.

---

### Ontology — Meaning Layer

An ontology adds business meaning to the data model.

It defines:

- entities  
- relationships  
- properties  

Example:

    ResaleTransaction → located_at → Location

Good for:
- expressing meaning  
- natural language queries  
- traversal across relationships  
- helping the data agent understand how concepts connect  

---

## Query vs Pattern

This is the key shift in how data is queried.

---

### Data Model — SQL

SQL queries use joins to combine tables.

Example of a SQL join:

<img width="253" height="64" alt="SQL join example" src="https://github.com/user-attachments/assets/207dfe67-ac63-43a0-9793-eccc1330cab6" />

<br>

Tables are connected using join conditions.  
You manually specify how data links together.

---

### Ontology / Graph — GQL

GQL queries relationships directly.

Example of a GQL pattern:

<img width="274" height="40" alt="GQL pattern example" src="https://github.com/user-attachments/assets/00407937-e594-4be2-839c-a232933cc144" />

<br>

Instead of joins, you describe how things connect.  
The structure defines the relationships.

---

## Key Difference

SQL connects tables.  
GQL follows relationships.

This is why ontology matters: it defines those relationships in a meaningful way.

---

## Thinking Like the Agent

Every question can be broken into three parts:

1. **Entity** — what are we querying?  
2. **Filters** — what conditions apply?  
3. **Operation** — what do we want to calculate or return?  

---

### 1. Entity — What are we querying?

Example:

- ResaleTransaction  

This tells the agent the main subject of the question.

---

### 2. Filters — What conditions apply?

Examples:

- town = TAMPINES  
- month = January  

Filters narrow the question to a specific location, time period, category, or condition.

---

### 3. Operation — What do we want?

Example:

- average → AVG(resale_price)  

The operation tells the agent whether the question asks for a summary, count, average, list, or comparison.

---

## Example Breakdown

**Question:**

> average resale price in Tampines in January

**Breakdown:**

| Component | Interpretation |
|----------|----------------|
| Entity | ResaleTransaction |
| Filter 1 | Location.town = TAMPINES |
| Filter 2 | SaleMonth = January |
| Operation | AVG(resale_price) |

If a query fails, revisit these three parts:

    Entity → Filters → Operation

---

## Why Queries Fail

Most issues are logical, not technical.

---

### 1. Mixing Detail + Summary

Example:

    transaction_id + AVG(resale_price)

This is not valid in an aggregated query.

Rule:

- **Summary query** → return aggregates only  
- **Detail query** → return row-level data  

Example:

| Query type | Appropriate output |
|-----------|--------------------|
| Summary | average resale price |
| Detail | transaction ID, resale price, town |

This is why a query may fail with a GROUP BY error if it tries to return `transaction_id` together with `AVG(resale_price)`.

---

### 2. Case Mismatch

Example:

    Tampines ≠ TAMPINES

If the data stores town names in uppercase, the query may need to use uppercase values.

Fix:

- standardise values  
- add instructions for normalization  

Example:

    Tampines → TAMPINES
    Bedok → BEDOK

---

### 3. Missing Mapping

Example:

    "January" not mapped to SaleMonth.month_name

If the agent does not know that “January” refers to `SaleMonth.month_name`, it may not be able to interpret the question correctly.

Fix:

- define clear entity meanings  
- map common business terms to ontology properties  
- provide examples in the data agent instructions  

---

## How the System Works

There are two layers that enable the agent:

---

### Structure — Ontology

The ontology defines:

- entities  
- relationships  
- properties  
- keys  
- bindings  

This defines what the data **means**.

Example:

    ResaleTransaction → located_at → Location

This tells the system that resale transactions are connected to locations.

---

### Behaviour — Instructions

Instructions define how the agent behaves.

They explain:

- how terms are mapped  
- how values should be normalized  
- how aggregations should be handled  
- what the agent should avoid  

Example:

    price → resale_price
    Tampines → TAMPINES
    average price → AVG(resale_price)
    Do not include transaction_id in aggregated queries

This helps the agent generate better queries.

---

## Key Insight

Ontology defines **structure**.  
Instructions define **behaviour**.

Together, they enable the system to reason over data.

---

## First Success

Try this simple query first:

> average resale price in BEDOK

This confirms that:

- the ontology is connected to the data  
- the `located_at` relationship works  
- `resale_price` can be aggregated  
- the data agent can interpret a basic question  

Start simple before testing complex questions.

---

## What Happens Behind the Scenes

When you ask a question, the agent follows a structured process:

    User question
    ↓
    Agent maps entities and filters
    ↓
    Agent generates query
    ↓
    Query runs on data
    ↓
    Result is returned

The agent is structured, not guessing.

If the answer is wrong or the query fails, check the structure first.

---

## Key Takeaways

- Ontology translates data into meaning  
- Relationships define connections  
- Properties provide values for filtering and aggregation  
- Instructions guide behaviour  
- Structure determines whether the agent works  
- Better structure leads to better answers  

---

## Next Step

Learning is best done through building — and fixing what breaks 😄

Go to the lab:  
[`/labs/lab-01-build-first-data-agent.md`](../labs/lab-01-build-first-data-agent.md)
