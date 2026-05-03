# Advanced NL2GQL — From Questions to Graph Queries

This page explains how natural language questions are translated into graph query logic.

NL2GQL means:

```text
Natural Language → Graph Query Language
```

In the beginner track, you learned how to build an ontology and data agent.

In this advanced track, you will learn how to reason about what happens behind the scenes.

---

## Why NL2GQL Matters

A data agent does not simply “know” the answer.

It must translate a user question into structured query logic.

For example:

```text
average resale price in Bedok
```

must be translated into something like:

```text
Entity → ResaleTransaction
Filter → Location.town = BEDOK
Relationship path → ResaleTransaction → located_at → Location
Operation → AVG(resale_price)
Return field → avg_resale_price
```

This is the core of NL2GQL.

---

## Research Context

NL2GQL is an emerging area related to natural language interfaces for graph databases and knowledge graphs.

Recent work highlights that translating natural language into graph queries is not just a simple prompt-to-query task. It often involves:

- identifying user intent
- linking terms to entities and properties
- selecting relationship paths
- generating graph query logic
- using execution feedback to refine failed queries
- evaluating whether the generated query matches the intended question

This is why ontology structure, relationship naming, binding, and data agent instructions matter.

---

## Core Mental Model

When a user asks a question, the system needs to identify:

1. **Intent** — what is the user asking for?
2. **Entity** — what is the main object being queried?
3. **Filters** — what conditions apply?
4. **Relationship path** — how do entities connect?
5. **Operation** — what calculation or result is needed?
6. **Return fields** — what should be returned?

A useful way to remember this is:

```text
Question
↓
Intent
↓
Entity
↓
Filters
↓
Relationship path
↓
Operation
↓
Return fields
↓
GQL
↓
Answer
```

This page repeats this framework across different examples on purpose.

The goal is to show that most NL2GQL problems can be diagnosed using the same structure.

---

## Example 1 — Simple Filter and Aggregation

### Natural language question

```text
What is the average resale price in Bedok?
```

---

### Breakdown

| Component | Mapping |
|---|---|
| Intent | Find average price |
| Entity | ResaleTransaction |
| Filter | Location.town = BEDOK |
| Relationship path | ResaleTransaction → located_at → Location |
| Operation | AVG(resale_price) |
| Return field | avg_resale_price |

---

### Possible GQL shape

```gql
MATCH (t:ResaleTransaction)-[:located_at]->(l:Location)
FILTER l.town = 'BEDOK'
RETURN AVG(t.resale_price) AS avg_resale_price
```

---

### What this teaches

This question works only if:

- `ResaleTransaction` exists as an entity
- `Location` exists as an entity
- `located_at` exists as a relationship
- `town` is correctly bound to data
- `resale_price` is numeric
- the agent knows that “price” means `resale_price`

---

## Example 2 — Time Filter

### Natural language question

```text
What is the average resale price in January?
```

---

### Breakdown

| Component | Mapping |
|---|---|
| Intent | Find average price |
| Entity | ResaleTransaction |
| Filter | SaleMonth.month_name = January |
| Relationship path | ResaleTransaction → sold_in → SaleMonth |
| Operation | AVG(resale_price) |
| Return field | avg_resale_price |

---

### Possible GQL shape

```gql
MATCH (t:ResaleTransaction)-[:sold_in]->(m:SaleMonth)
FILTER m.month_name = 'January'
RETURN AVG(t.resale_price) AS avg_resale_price
```

---

### What this teaches

Time questions only work if the ontology has a clear time relationship.

If `sold_in` is missing or unclear, the agent may not know how to connect transactions to months.

---

## Example 3 — Multi-Entity Filter

### Natural language question

```text
What is the average resale price in Tampines in January?
```

This is more difficult because the agent must use two filters from two different entities:

- `Location`
- `SaleMonth`

---

### Breakdown

| Component | Mapping |
|---|---|
| Intent | Find average price |
| Entity | ResaleTransaction |
| Filter 1 | Location.town = TAMPINES |
| Filter 2 | SaleMonth.month_name = January |
| Relationship path 1 | ResaleTransaction → located_at → Location |
| Relationship path 2 | ResaleTransaction → sold_in → SaleMonth |
| Operation | AVG(resale_price) |
| Return field | avg_resale_price |

---

### Possible GQL shape

```gql
MATCH (t:ResaleTransaction)-[:located_at]->(l:Location),
      (t)-[:sold_in]->(m:SaleMonth)
FILTER l.town = 'TAMPINES'
FILTER m.month_name = 'January'
RETURN AVG(t.resale_price) AS avg_resale_price
```

---

### Why this may fail

This query combines:

- location filtering
- time filtering
- relationship traversal
- aggregation

If the ontology, bindings, or instructions are unclear, the generated query may fail.

This is why advanced data-agent work is not just about asking questions.

It is about designing the structure that allows questions to be translated correctly.

---

## Common NL2GQL Failure Patterns

### 1. Wrong Entity Mapping

Question:

```text
What is the price in Bedok?
```

Possible problem:

```text
price of what?
```

The agent may be unsure whether the user means:

- resale transaction price
- flat price
- location-level price
- price per sqm

Fix:

- define `ResaleTransaction` clearly
- map “price” to `resale_price`
- map “price per sqm” to `price_per_sqm`

---

### 2. Wrong Filter Mapping

Question:

```text
What is the average resale price in Tampines?
```

Possible problem:

```text
Tampines ≠ TAMPINES
```

If town values are stored in uppercase, the generated query may fail or return no results.

Fix:

```text
Normalize town names to uppercase before querying.
```

---

### 3. Missing Relationship Path

Question:

```text
What is the average resale price in January?
```

Possible problem:

The agent needs this path:

```text
ResaleTransaction → sold_in → SaleMonth
```

If the relationship is missing, reversed, or poorly named, the agent may not know how to reach the month.

Fix:

- create the `sold_in` relationship
- ensure it points from `ResaleTransaction` to `SaleMonth`
- verify relationship binding

---

### 4. Mixing Detail and Summary

Bad generated query:

```gql
RETURN t.transaction_id, AVG(t.resale_price)
```

Problem:

The query mixes:

- row-level field: `transaction_id`
- aggregated value: `AVG(resale_price)`

This can cause a GROUP BY error.

Fix:

```gql
RETURN AVG(t.resale_price)
```

Rule:

```text
Summary queries should return grouped fields and aggregated measures only.
Detail queries may return row-level identifiers.
```

---

### 5. Wrong Return Fields

Question:

```text
What is the average resale price in Bedok?
```

The answer should return:

```text
avg_resale_price
```

It should not return:

```text
transaction_id
location_key
flat_key
lease_key
```

Return fields matter because they define the level of detail in the answer.

---

## NL2GQL Debugging Framework

When a generated query fails, debug it in this order.

---

### 1. Entity

Ask:

```text
Did the agent choose the right entity?
```

Example:

```text
ResaleTransaction
```

---

### 2. Filter

Ask:

```text
Did the agent map the filter correctly?
```

Example:

```text
Bedok → Location.town = BEDOK
```

---

### 3. Relationship Path

Ask:

```text
Did the agent traverse the correct relationship?
```

Example:

```text
ResaleTransaction → located_at → Location
```

---

### 4. Operation

Ask:

```text
Did the agent choose the right calculation?
```

Example:

```text
average → AVG(resale_price)
```

---

### 5. Return Fields

Ask:

```text
Did the agent return only what is needed?
```

For summary queries, avoid row-level fields such as:

```text
transaction_id
location_key
flat_key
lease_key
```

---

## Instruction Design for Better NL2GQL

Instructions help shape how the agent translates questions.

A strong instruction block should define:

- domain
- entity meanings
- business term mappings
- normalization rules
- aggregation rules
- grouping rules
- error-prevention rules
- response rules

---

### Example instruction block

```text
You are a data agent that answers questions about Singapore housing resale transactions.

Use only data from HousingResaleOntology.
Do not use outside knowledge.

Entity meanings:
- ResaleTransaction = one HDB resale transaction
- Location = region, town, street, and block
- SaleMonth = transaction month and year
- Flat = flat type, flat model, storey range, and floor area
- Lease = lease commence date and remaining lease

Business term mappings:
- resale price, price, cost -> resale_price
- price per sqm -> price_per_sqm
- town -> Location.town
- month -> SaleMonth.month_name
- year -> SaleMonth.sale_year

Normalization rules:
- Town values are stored in uppercase.
- Convert user-provided town names to uppercase before querying.
- Example: Tampines -> TAMPINES

Aggregation rules:
- average price -> AVG(resale_price)
- total price -> SUM(resale_price)
- number of transactions -> COUNT(ResaleTransaction)

Grouping rules:
- by town -> group by Location.town
- by month -> group by SaleMonth.month_name

Error prevention:
- Do not return transaction_id or other row-level identifiers in aggregated queries.
- Summary queries should return only grouped fields and aggregated measures.
- Detail queries may return row-level identifiers and measures without aggregation.
- Support group by in GQL.
```

---

## Key Takeaways

- NL2GQL translates natural language into graph query logic.
- The quality of NL2GQL depends on ontology quality.
- Good relationship names make query generation easier.
- Correct binding makes the ontology usable.
- Clear instructions reduce query errors.
- Most failures can be debugged by checking entity, filter, relationship path, operation, and return fields.

---

## Further Reading

For academic and technical references on NL2GQL and natural language querying over graphs, see:

[`/docs/references.md`](./references.md)

---

## Next Step

Continue to:

[`/docs/ontology-design-principles.md`](./ontology-design-principles.md)
