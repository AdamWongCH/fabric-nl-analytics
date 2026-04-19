# Learning Guide — How to Think

This guide explains how to think about ontology and data agents.

You are not just following steps.  
You are learning how data is structured so AI can reason over it.  

---

## Big Picture
Data → Semantic Model → Ontology → Data Agent → Answer

- Data = raw tables  
- Semantic Model = structure  
- Ontology = meaning  
- Data Agent = interface  

Better structure leads to better answers.  

---

## Why Structure Matters

AI does not understand data the way humans do.

It relies on structure:

- entities → what things are  
- relationships → how things connect  
- properties → what we know about them  

This structure enables the system to interpret questions and generate meaningful queries.

---

## From Data Model to Ontology

You are translating a data model into meaning.

| Data Model | Ontology |
|-----------|----------|
| Table     | Entity   |
| Column    | Property |
| Join      | Relationship |

---

### Example

| Table                    | Entity              |
|--------------------------|--------------------|
| fact_resale_transaction  | ResaleTransaction  |
| dim_location             | Location           |
| dim_date                 | SaleMonth          |

---

## Taxonomy vs Data Model vs Ontology

These concepts are related but serve different purposes.

---

### Taxonomy (Hierarchy)

- classification  
- categories  
- parent-child  

Example:
Flat Type → 3-Room → 4-Room → 5-Room  

Good for organisation.  
Not good for querying relationships.  

---

### Data Model (Tables)

- structured storage  
- tables and joins  

Good for SQL queries.  
Meaning is not explicit.  

---

### Ontology (Meaning Layer)

- entities  
- relationships  
- properties  

Example:
ResaleTransaction → located_at → Location  

Good for:
- expressing meaning  
- natural language queries  
- traversal across relationships  

---

## Query vs Pattern

This is the key shift in how data is queried.

---

### Data Model (SQL)

SQL queries use joins to combine tables.

Example of a SQL join:

<img width="253" height="64" alt="image" src="https://github.com/user-attachments/assets/207dfe67-ac63-43a0-9793-eccc1330cab6" />
</br>
Tables are connected using join conditions.  
You manually specify how data links together.  

---

### Ontology / Graph (GQL)

GQL queries relationships directly.

Example of a GQL pattern:

<img width="274" height="40" alt="image" src="https://github.com/user-attachments/assets/00407937-e594-4be2-839c-a232933cc144" />
</br>
Instead of joins, you describe how things connect.  
The structure defines the relationships.  

---

## Key Difference

SQL connects tables.  
GQL follows relationships.  

This is why ontology matters — it defines those relationships

---

## Thinking Like the Agent

Every question can be broken into 3 parts:

---

### 1. Entity (What are we querying?)

Example:
- ResaleTransaction  

---

### 2. Filters (What conditions apply?)

Example:
- town = TAMPINES  
- month = January  

---

### 3. Operation (What do we want?)

Example:
- average → AVG(resale_price)  

---

## Example

**Question:**
> average resale price in Tampines in January  

**Breakdown:**

- Entity → ResaleTransaction  
- Filters → Location.town, SaleMonth  
- Operation → AVG(resale_price)  

---

If a query fails, revisit these three parts

---

## Why Queries Fail

Most issues are logical, not technical.

---

### 1️. Mixing Detail + Summary

Example:
- transaction_id + AVG(resale_price)  

Not valid  

Rule:
- Summary → aggregates only  
- Detail → row-level only  

---

### 2️. Case Mismatch

Example:
- Tampines ≠ TAMPINES  

Fix:
- standardise values  

---

### 3️. Missing Mapping

Example:
- "January" not mapped  

 The agent cannot interpret it  

---

## How the System Works

There are **two layers** that enable the agent:

---

### Structure (Ontology)

Defines:
- entities  
- relationships  
- properties  
- keys  

This defines what the data *means*

---

### Behaviour (Instructions)

Defines:
- how the agent interprets questions  
- how terms are mapped  
- how queries are constructed  

This defines how the agent *behaves*

---

## Key Insight

Ontology defines structure  
Instructions define behaviour  

Together, they enable the system to reason over data  

---

## First Success

Try this simple query first:

> average resale price in BEDOK  

This confirms your setup is working  

---

## What Happens Behind the Scenes
User question → Agent maps entities and filters → Agent generates query → Query runs on data → Result is returned

The agent is structured, not guessing  

---

## Key Takeaways

- Ontology translates data into meaning  
- Relationships define connections  
- Instructions guide behaviour  
- Structure determines whether the agent works  

---

## Next Step

Learning is best done through building 😄

Go to the lab:  
[`/labs/lab-guide.md`](../labs/lab-guide.md)

