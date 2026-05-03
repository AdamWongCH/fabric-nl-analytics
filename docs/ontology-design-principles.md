# Ontology Design Principles

This page explains how to design an ontology that supports better data-agent reasoning.

In the beginner lab, you created an ontology.

In this advanced section, you will learn how to improve ontology design so that natural language questions are easier to interpret, translate, and answer.

---

## Core Idea

Ontology design is not just naming tables.

It is about defining a structure that helps the agent understand:

- what things are
- how things connect
- what properties matter
- how questions should be interpreted

The better the structure, the better the agent can reason over data.

---

## The Four Layers of Ontology Design

A practical ontology has four important layers:

```text
Keys → Relationships → Binding → Instructions
```

Each layer plays a different role.

| Layer | Purpose |
|---|---|
| Keys | define identity |
| Relationships | define structure |
| Binding | connects ontology to data |
| Instructions | control agent behaviour |

---

## 1. Design for Business Meaning

Ontology names should make sense to business users, not just technical users.

---

### Weak Naming

```text
fact_resale_transaction
dim_location
dim_date
```

These names reflect the database structure.

They are technically correct, but not ideal for natural language reasoning.

---

### Better Naming

```text
ResaleTransaction
Location
SaleMonth
```

These names represent concepts.

They are easier for both humans and agents to interpret.

---

## Principle

Use names that reflect **business meaning**, not only technical storage.

---

## 2. Keep the Ontology Simple First

A common mistake is to model too much too early.

For a beginner ontology, start with only the entities needed to answer common questions.

For example:

```text
ResaleTransaction
Location
SaleMonth
```

This is enough to answer questions such as:

- average resale price in BEDOK
- number of transactions by town
- average resale price by month

You can add more entities later, such as:

```text
Flat
Lease
```

---

## Principle

Start simple.

Add complexity only when it supports real questions.

---

## 3. Define Clear Entity Boundaries

Each entity should represent one clear concept.

---

### Good Entity Boundary

```text
Location
```

Represents:

- town
- region
- street
- block

---

### Potential Issue

If too many concepts are mixed into one entity, the agent may struggle to map questions correctly.

For example, if flat type, location, lease, and time are all kept in one large entity, the agent may still answer some questions, but relationships become less meaningful.

---

## Principle

Each entity should answer:

```text
What business thing does this represent?
```

If the answer is unclear, the entity design may be unclear.

---

## 4. Use Relationship Names That Read Naturally

Relationships are not just joins.

They express meaning.

---

### Weak Relationship Name

```text
ResaleTransaction → has_location → Location
```

This is understandable, but generic.

---

### Better Relationship Name

```text
ResaleTransaction → located_at → Location
```

This reads more naturally:

> A resale transaction is located at a location.

---

### Another Example

```text
ResaleTransaction → sold_in → SaleMonth
```

This reads naturally:

> A resale transaction was sold in a sale month.

---

## Principle

Relationship names should read like simple English phrases.

This helps both human readers and the data agent.

---

## 5. Get Relationship Direction Right

Direction matters.

---

### Good Direction

```text
ResaleTransaction → located_at → Location
```

This means:

> A transaction is located at a location.

---

### Less Natural Direction

```text
Location → located_at → ResaleTransaction
```

This is harder to read.

A location is not “located at” a transaction.

---

## Principle

Choose the direction that makes the relationship read naturally from source to target.

---

## 6. Keys Define Identity

Each entity needs a key.

A key uniquely identifies each record in the entity.

Examples:

| Entity | Key |
|---|---|
| ResaleTransaction | transaction_id |
| Location | location_key |
| SaleMonth | date_key |
| Flat | flat_key |
| Lease | lease_key |

---

## Why Keys Matter

Keys are important because they support:

- identity
- relationships
- correct traversal
- correct aggregation

If keys are wrong, the agent may generate queries that return incorrect or duplicated results.

---

## Principle

Define keys before testing relationships or agent queries.

---

## 7. Binding Grounds the Ontology in Data

Binding connects the ontology to the actual data.

There are two types of binding:

---

### Property Binding

Property binding maps ontology properties to data columns.

Example:

```text
resale_price → fact_resale_transaction.resale_price
town → dim_location.town
```

---

### Relationship Binding

Relationship binding maps keys between entities.

Example:

```text
ResaleTransaction.location_key = Location.location_key
```

---

## Why Binding Matters

Without correct binding:

- the ontology may look right
- but the agent may not retrieve the correct data
- queries may fail or return incorrect results

---

## Principle

Do not assume the ontology works just because the relationship exists.

Always verify binding.

---

## 8. Properties Should Support Questions

Not every column needs to become a prominent property.

Focus on properties that users are likely to ask about.

---

### Important Properties

For this dataset:

```text
resale_price
price_per_sqm
town
region
month_name
sale_year
```

These support common questions such as:

- average resale price
- price per sqm
- price by town
- price by month
- transactions by year

---

### Less Important Properties

Some technical fields may be necessary for relationships but not useful for user-facing questions.

Examples:

```text
location_key
date_key
flat_key
lease_key
```

These are important structurally, but users rarely ask about them directly.

---

## Principle

Separate properties into:

- user-facing properties
- technical properties

Both may be needed, but they serve different purposes.

---

## 9. Avoid Ambiguous Terms

Ambiguous terms make NL2GQL harder.

For example:

```text
price
```

could mean:

- resale_price
- price_per_sqm
- total price
- average price

The agent may need help deciding.

---

## Better Design

Use clear mappings in instructions:

```text
resale price, price, cost -> resale_price
price per sqm -> price_per_sqm
```

---

## Principle

If a term can mean multiple things, define the intended mapping.

---

## 10. Design for Common Questions

A good ontology should be shaped by the questions users are likely to ask.

---

## Example User Questions

```text
What is the average resale price in BEDOK?
Which town has the highest number of transactions?
What is the average resale price by month?
What is the average resale price in TAMPINES in January?
```

---

## What the Ontology Needs

To support these questions, the ontology needs:

- ResaleTransaction entity
- Location entity
- SaleMonth entity
- located_at relationship
- sold_in relationship
- resale_price property
- town property
- month_name property

---

## Principle

Start from likely questions.

Then design the ontology to support those questions.

---

## 11. Design for Debugging

A good ontology is not only easy to query.

It is also easy to debug.

---

## Debugging Questions

When something fails, ask:

```text
Did the agent choose the right entity?
Did it use the right filter?
Did it follow the right relationship?
Did it use the right operation?
Did it return the right fields?
```

These questions are easier to answer when the ontology is clearly designed.

---

## Principle

Design the ontology so failures are easier to diagnose.

---

## 12. Avoid Over-Modelling

Over-modelling happens when too many entities and relationships are added too early.

This may cause:

- ambiguity
- longer relationship paths
- incorrect traversal
- harder debugging

---

## Example

If the beginner goal is to answer questions by town and month, you may not need to model every possible detail immediately.

Start with:

```text
ResaleTransaction
Location
SaleMonth
```

Then extend later:

```text
Flat
Lease
```

---

## Principle

Model only what supports meaningful questions.

---

## Ontology Design Checklist

Use this checklist before testing the data agent.

| Check | Question |
|---|---|
| Entity names | Are they business-friendly? |
| Entity boundaries | Does each entity represent one clear concept? |
| Keys | Does each entity have a unique key? |
| Relationships | Do relationships read naturally? |
| Direction | Does the relationship direction make sense? |
| Binding | Are properties and relationship keys correctly bound? |
| Properties | Are important user-facing properties exposed? |
| Ambiguity | Are common terms mapped clearly? |
| Questions | Can the ontology support target user questions? |
| Debugging | Can failures be diagnosed clearly? |

---

## Example: Improving the Beginner Ontology

### Initial Ontology

```text
ResaleTransaction
Location
SaleMonth
```

Relationships:

```text
ResaleTransaction → located_at → Location
ResaleTransaction → sold_in → SaleMonth
```

This supports:

- location questions
- time questions
- price aggregation

---

### Extended Ontology

Add:

```text
Flat
Lease
```

Relationships:

```text
ResaleTransaction → for_flat → Flat
ResaleTransaction → under_lease → Lease
```

This supports:

- price by flat type
- price by flat model
- analysis by lease remaining

---

## Key Takeaways

- Ontology design shapes how the data agent reasons.
- Entity names should be business-friendly.
- Relationships should read naturally.
- Keys define identity.
- Binding grounds ontology in data.
- Properties should support real user questions.
- Simpler ontology is usually better at the start.
- Better ontology design makes NL2GQL more reliable.

---

## Next Step

Continue to:

[`/docs/agent-instruction-design.md`](./agent-instruction-design.md)
