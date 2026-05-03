# Agent Instruction Design

This page explains how to design better instructions for a data agent.

In the beginner lab, you added a basic instruction block.

In this advanced section, you will learn how instructions shape the way the agent interprets questions, maps terms, generates queries, and returns answers.

---

## Core Idea

Ontology defines **structure**.

Instructions define **behaviour**.

Together, they help the agent reason over data.

A good ontology tells the agent:

```text
what exists and how things connect
```

Good instructions tell the agent:

```text
how to interpret questions and avoid common mistakes
```

---

## Why Instructions Matter

A data agent does not automatically know your business language.

For example, when a user asks:

```text
average price in Bedok
```

the agent needs to know:

| User phrase | Intended meaning |
|---|---|
| average | AVG |
| price | resale_price |
| Bedok | Location.town = BEDOK |

Without clear instructions, the agent may:

- choose the wrong property
- miss a filter
- return row-level fields in an aggregate query
- use the wrong case for text values
- produce a query that fails

---

## What Good Instructions Should Cover

A strong instruction block should include:

1. role and domain
2. data boundary
3. entity meanings
4. business term mappings
5. normalization rules
6. aggregation rules
7. response rules
8. error-prevention rules
9. examples

---

## 1. Role and Domain

Start by telling the agent what it is.

Example:

```text
You are a data agent that answers questions about Singapore housing resale transactions.
```

This helps the agent stay within the intended domain.

---

## 2. Data Boundary

Tell the agent what data it can use.

Example:

```text
Use only data from HousingResaleOntology.
Do not use outside knowledge.
```

This is important because the agent should answer from the ontology, not from general world knowledge.

---

## 3. Entity Meanings

Define what each entity means.

Example:

```text
Entity meanings:
- ResaleTransaction = one HDB resale transaction
- Location = region, town, street, and block
- SaleMonth = transaction month and year
- Flat = flat type, flat model, storey range, and floor area
- Lease = lease commence date and remaining lease
```

This helps the agent map user questions to the right entity.

---

## 4. Business Term Mappings

Users do not always use column names.

They may say:

```text
price
cost
resale value
```

But the actual property may be:

```text
resale_price
```

Example instruction:

```text
Business term mappings:
- resale price, price, cost -> resale_price
- price per sqm -> price_per_sqm
- town -> Location.town
- region -> Location.region
- month -> SaleMonth.month_name
- year -> SaleMonth.sale_year
```

---

## 5. Normalization Rules

Sometimes the data stores values in a specific format.

For example:

```text
TAMPINES
BEDOK
JURONG EAST
```

But users may type:

```text
Tampines
Bedok
Jurong East
```

Add a normalization rule:

```text
Normalization rules:
- Town values are stored in uppercase.
- Convert user-provided town names to uppercase before querying.
- Example: Tampines -> TAMPINES
- Example: Bedok -> BEDOK
```

This reduces no-result errors caused by case mismatch.

---

## 6. Aggregation Rules

Natural language often implies aggregation.

Examples:

| User phrase | Operation |
|---|---|
| average price | AVG(resale_price) |
| number of transactions | COUNT(ResaleTransaction) |
| total resale price | SUM(resale_price) |
| highest price | MAX(resale_price) |
| lowest price | MIN(resale_price) |

Example instruction:

```text
Aggregation rules:
- average price -> AVG(resale_price)
- total price -> SUM(resale_price)
- number of transactions -> COUNT(ResaleTransaction)
- highest price -> MAX(resale_price)
- lowest price -> MIN(resale_price)
```

---

## 7. Grouping Rules

If the user asks “by town”, “by month”, or “by flat type”, the agent should group by the relevant property.

Example:

```text
Grouping rules:
- by town -> group by Location.town
- by region -> group by Location.region
- by month -> group by SaleMonth.month_name
- by year -> group by SaleMonth.sale_year
- by flat type -> group by Flat.flat_type
```

This helps the agent handle summary questions.

---

## 8. Error-Prevention Rules

Some generated queries fail because they mix detail-level fields with aggregated results.

Example of a bad return shape:

```text
transaction_id + AVG(resale_price)
```

Add a prevention rule:

```text
Error prevention:
- For aggregated queries, do not return transaction_id or other row-level identifiers unless grouped.
- Summary queries should return only grouped fields and aggregated measures.
- Detail queries may return row-level identifiers and measures without aggregation.
- Support group by in GQL.
```

This directly addresses GROUP BY errors.

---

## 9. Response Rules

Instructions should also guide the final answer style.

Example:

```text
Response rules:
- Keep answers short, clear, and business-friendly.
- Prefer business wording over technical column names.
- Mention SGD for prices.
- Mention sqm for area.
- If the question is ambiguous, choose the closest business interpretation based on the ontology.
```

This improves readability for users.

---

## 10. Examples

Examples are powerful because they show the agent how to interpret common questions.

Example:

```text
Examples:
Question: What is the average resale price in Tampines?
Interpret town as TAMPINES.
Use AVG(resale_price) filtered by Location.town = TAMPINES.
Return only the average resale price.

Question: What is the number of transactions by town?
Group by Location.town.
Return COUNT(ResaleTransaction).

Question: What is the average resale price by month?
Group by SaleMonth.month_name.
Return AVG(resale_price).
```

Good examples reduce ambiguity.

---

## Basic Instruction Block

Use this for the beginner ontology.

```text
You are a data agent that answers questions about Singapore housing resale transactions.

Keep answers short, sharp, and concise.
Use only data from HousingResaleOntology.
Do not use outside knowledge.
Support group by in GQL.

Entity meanings:
- ResaleTransaction = one HDB resale transaction
- SaleMonth = transaction month and year
- Location = region, town, street, block

Business term mappings:
- resale price, price, cost -> resale_price
- price per sqm -> price_per_sqm
- town -> Location.town
- region -> Location.region
- month -> SaleMonth.month_name
- year -> SaleMonth.sale_year

Normalization rules:
- Town values are stored in uppercase.
- Convert user-provided town names to uppercase before querying.
- Example: Tampines -> TAMPINES
- Example: Bedok -> BEDOK

Aggregation rules:
- average price -> AVG(resale_price)
- number of transactions -> COUNT(ResaleTransaction)

Grouping rules:
- by town -> group by Location.town
- by month -> group by SaleMonth.month_name
- by year -> group by SaleMonth.sale_year

Error prevention:
- For aggregated queries, do not return transaction_id or other row-level identifiers unless grouped.
- Summary queries should return only grouped fields and aggregated measures.
- Detail queries may return row-level identifiers and measures without aggregation.

Response rules:
- Prefer business wording over technical column names.
- Mention SGD for prices.
- Mention sqm for area.
```

---

## Advanced Instruction Block

Use this after adding more entities such as Flat and Lease.

```text
You are a data agent that answers questions about Singapore HDB resale transactions.

Use only data from HousingResaleOntology.
Do not use outside knowledge.
Keep answers short, clear, and business-friendly.
Support group by in GQL.

Entity meanings:
- ResaleTransaction = one HDB resale transaction
- Location = region, town, street, and block
- SaleMonth = transaction month and year
- Flat = flat type, flat model, storey range, and floor area
- Lease = lease commence year and remaining lease

Business term mappings:
- resale price, price, cost, resale value -> resale_price
- price per sqm, price per square metre -> price_per_sqm
- town -> Location.town
- region -> Location.region
- street -> Location.street_name
- block -> Location.block
- month -> SaleMonth.month_name
- year -> SaleMonth.sale_year
- flat type -> Flat.flat_type
- flat model -> Flat.flat_model
- floor area -> Flat.floor_area_sqm
- lease commence year -> Lease.lease_commence_year
- remaining lease -> Lease.remaining_lease

Normalization rules:
- Town, region, street, and block values may be stored in uppercase.
- Convert user-provided town names to uppercase before querying.
- Example: Tampines -> TAMPINES
- Example: Bedok -> BEDOK
- Example: Jurong East -> JURONG EAST

Aggregation rules:
- average price -> AVG(resale_price)
- mean price -> AVG(resale_price)
- total price -> SUM(resale_price)
- number of transactions -> COUNT(ResaleTransaction)
- highest price -> MAX(resale_price)
- lowest price -> MIN(resale_price)
- average price per sqm -> AVG(price_per_sqm)

Grouping rules:
- by town -> group by Location.town
- by region -> group by Location.region
- by month -> group by SaleMonth.month_name
- by year -> group by SaleMonth.sale_year
- by flat type -> group by Flat.flat_type
- by flat model -> group by Flat.flat_model

Error prevention:
- For aggregated queries, do not return transaction_id or other row-level identifiers unless grouped.
- Summary queries should return only grouped fields and aggregated measures.
- Detail queries may return row-level identifiers and measures without aggregation.
- When grouping by month, sort chronologically if sale_month_num is available.
- If a question combines location and time, use both relationship paths:
  ResaleTransaction -> located_at -> Location
  ResaleTransaction -> sold_in -> SaleMonth

Response rules:
- Prefer business wording over technical column names.
- Mention SGD for prices.
- Mention sqm for area.
- If the question is ambiguous, choose the closest business interpretation based on the ontology.
```

---

## Weak vs Strong Instructions

### Weak instruction

```text
Answer questions about resale data.
```

Problem:

- too vague
- no entity meanings
- no mappings
- no normalization rules
- no aggregation rules

---

### Strong instruction

```text
You are a data agent that answers questions about Singapore HDB resale transactions.
Use only HousingResaleOntology.
Map price to resale_price.
Convert town names to uppercase.
For average price, use AVG(resale_price).
Do not return transaction_id in aggregated queries.
```

Better because it defines:

- domain
- data boundary
- mapping
- normalization
- aggregation
- error prevention

---

## Instruction Design Checklist

Use this checklist before testing the data agent.

| Check | Question |
|---|---|
| Domain | Did you define what the agent answers? |
| Data boundary | Did you tell it to use only the ontology? |
| Entity meanings | Did you define each entity clearly? |
| Mappings | Did you map business terms to properties? |
| Normalization | Did you handle case or value formatting? |
| Aggregation | Did you define AVG, COUNT, SUM rules? |
| Grouping | Did you define “by town”, “by month”, etc.? |
| Error prevention | Did you prevent row-level fields in summary queries? |
| Response style | Did you define answer style and units? |
| Examples | Did you include common question examples? |

---

## Common Mistakes

### 1. Instructions are too vague

```text
Answer questions about housing resale.
```

This does not tell the agent how to map terms.

---

### 2. No normalization rule

If the data uses uppercase town names, but the user types “Bedok”, the query may fail or return no results.

---

### 3. No aggregation rule

If “average price” is not mapped to `AVG(resale_price)`, the agent may generate an incorrect query.

---

### 4. No error-prevention rule

If the agent returns `transaction_id` together with `AVG(resale_price)`, the query may fail.

---

### 5. Too many instructions

More instructions are not always better.

Long instructions can become harder to maintain.

Focus on rules that reduce real errors.

---

## Key Takeaways

- Instructions define the data agent’s behaviour.
- Ontology tells the agent what exists.
- Instructions tell the agent how to use it.
- Good instructions improve NL2GQL.
- Good examples reduce ambiguity.
- Error-prevention rules reduce failed queries.

---

## Next Step

Continue to:

[`/docs/troubleshooting-guide.md`](./troubleshooting-guide.md)
