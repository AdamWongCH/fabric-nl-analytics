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

## Next Step

Learning is best done through building 😄

Go to the lab:  
[`/labs/lab-guide.md`](../labs/lab-guide.md)


