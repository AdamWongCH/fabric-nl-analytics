# Lab 03 — Debugging Agent Errors

This lab helps you practise diagnosing and fixing common data-agent failures.

The goal is not just to make the query work.

The goal is to understand **why** it failed.

---

## Goal

By the end of this lab, you should be able to:

- read a failed agent response or generated query
- identify whether the issue is caused by entity, filter, relationship path, operation, or return fields
- distinguish between structure problems and instruction problems
- improve agent instructions to reduce repeated failures
- document the issue and fix clearly

---

## Before You Start

You should have completed:

- `/labs/lab-01-build-first-data-agent.md`
- `/labs/lab-02-advanced-nl2gql.md`

You should also be familiar with:

- `/docs/troubleshooting-guide.md`

---

## Core Debugging Framework

Use this framework whenever a query fails:

```text
Entity → Filter → Relationship path → Operation → Return fields
```

Ask:

| Component | Debugging question |
|---|---|
| Entity | Did the agent choose the right entity? |
| Filter | Did the agent apply the correct condition? |
| Relationship path | Did the agent traverse the correct relationship? |
| Operation | Did the agent use the correct calculation? |
| Return fields | Did the agent return the correct level of detail? |

---

## Dataset Context

Assume the ontology includes:

### Entities

- ResaleTransaction
- Location
- SaleMonth

---

### Relationships

```text
ResaleTransaction → located_at → Location
ResaleTransaction → sold_in → SaleMonth
```

---

### Key properties

| Entity | Properties |
|---|---|
| ResaleTransaction | transaction_id, resale_price, price_per_sqm |
| Location | town, region |
| SaleMonth | month_name, sale_year |

---

# Part A — Reproduce a Known Error

## Test Question

Ask the data agent:

```text
average resale price in BEDOK
```

---

## Expected Behaviour

The agent should return:

```text
AVG(resale_price)
```

filtered by:

```text
Location.town = BEDOK
```

The answer should be a single aggregated value.

---

## Possible Failure

The generated query may try to return something like:

```text
transaction_id, location_key, AVG(resale_price)
```

This is a problem because it mixes:

- row-level fields
- aggregated values

---

## Error Pattern

You may see an error similar to:

```text
The identifier node_ResaleTransaction.transaction_id cannot be used,
as it is neither part of the GROUP BY nor an aggregation.
```

---

## Diagnosis

Complete the table:

| Component | Expected | Actual | Correct? |
|---|---|---|---|
| Entity | ResaleTransaction |  |  |
| Filter | Location.town = BEDOK |  |  |
| Relationship path | ResaleTransaction → located_at → Location |  |  |
| Operation | AVG(resale_price) |  |  |
| Return fields | avg_resale_price only |  |  |

---

## What Went Wrong?

Most likely, the agent understood the entity, filter, relationship, and operation.

The failure happened at:

```text
Return fields
```

It returned row-level identifiers in an aggregated query.

---

## Fix

Add or strengthen this rule in the agent instructions:

```text
For aggregated queries, do not return transaction_id or any row-level identifier unless grouped.
Summary queries should return only grouped fields and aggregated measures.
Detail queries may return row-level identifiers and measures without aggregation.
```

---

## Retest

Ask again:

```text
average resale price in BEDOK
```

Expected result:

```text
single average resale price value
```

---

# Part B — Debug a Case Sensitivity Issue

## Test Question

Ask:

```text
average resale price in Tampines
```

---

## Expected Behaviour

The agent should interpret:

```text
Tampines → TAMPINES
```

and apply:

```text
Location.town = TAMPINES
```

---

## Possible Failure

The query returns no result.

---

## Diagnosis

Complete the table:

| Component | Expected | Actual | Correct? |
|---|---|---|---|
| Entity | ResaleTransaction |  |  |
| Filter | Location.town = TAMPINES |  |  |
| Relationship path | ResaleTransaction → located_at → Location |  |  |
| Operation | AVG(resale_price) |  |  |
| Return fields | avg_resale_price |  |  |

---

## What Went Wrong?

If the structure is correct but the result is empty, the problem may be:

```text
Filter value normalization
```

The data may store town names in uppercase.

---

## Fix

Add or strengthen this rule:

```text
Town values are stored in uppercase.
Convert user-provided town names to uppercase before querying.
Example: Tampines -> TAMPINES
Example: Bedok -> BEDOK
```

---

## Retest

Ask:

```text
average resale price in Tampines
```

Then ask:

```text
average resale price in TAMPINES
```

Expected result:

```text
Both should resolve to the same town filter.
```

---

# Part C — Debug a Month Mapping Issue

## Test Question

Ask:

```text
average resale price in January
```

---

## Expected Behaviour

The agent should apply:

```text
SaleMonth.month_name = January
```

using this relationship:

```text
ResaleTransaction → sold_in → SaleMonth
```

---

## Possible Failure

The agent may not understand January as a month filter.

It may:

- ignore the month
- return all transactions
- fail to generate a valid query
- use the wrong property

---

## Diagnosis

Complete the table:

| Component | Expected | Actual | Correct? |
|---|---|---|---|
| Entity | ResaleTransaction |  |  |
| Filter | SaleMonth.month_name = January |  |  |
| Relationship path | ResaleTransaction → sold_in → SaleMonth |  |  |
| Operation | AVG(resale_price) |  |  |
| Return fields | avg_resale_price |  |  |

---

## What to Check

Check ontology structure:

- Does `SaleMonth` exist?
- Does `SaleMonth.month_name` exist?
- Is `date_key` defined as the key?
- Does `ResaleTransaction → sold_in → SaleMonth` exist?
- Is the relationship binding correct?

---

## Fix

Add or strengthen this mapping:

```text
month -> SaleMonth.month_name
year -> SaleMonth.sale_year
January, February, March and other month names refer to SaleMonth.month_name.
```

---

## Retest

Ask:

```text
average resale price in January
```

Expected result:

```text
Average resale price filtered by SaleMonth.month_name = January.
```

---

# Part D — Debug a Multi-Filter Query

## Test Question

Ask:

```text
average resale price in BEDOK in January
```

---

## Expected Behaviour

The agent should combine:

```text
Location.town = BEDOK
SaleMonth.month_name = January
```

using both relationship paths:

```text
ResaleTransaction → located_at → Location
ResaleTransaction → sold_in → SaleMonth
```

The result should be:

```text
AVG(resale_price)
```

---

## Why This Is Harder

This query combines:

- location filter
- time filter
- relationship traversal
- aggregation

It is a stronger test of the ontology and instructions.

---

## Diagnosis

Complete the table:

| Component | Expected | Actual | Correct? |
|---|---|---|---|
| Entity | ResaleTransaction |  |  |
| Filter 1 | Location.town = BEDOK |  |  |
| Filter 2 | SaleMonth.month_name = January |  |  |
| Relationship path 1 | ResaleTransaction → located_at → Location |  |  |
| Relationship path 2 | ResaleTransaction → sold_in → SaleMonth |  |  |
| Operation | AVG(resale_price) |  |  |
| Return fields | avg_resale_price |  |  |

---

## Debugging Strategy

Test simpler versions first:

```text
average resale price in BEDOK
```

Then:

```text
average resale price in January
```

Then:

```text
average resale price in BEDOK in January
```

If the first two work but the third fails, the issue is likely in:

- multi-relationship traversal
- combined filter logic
- return fields
- instructions

---

## Fix

Add or strengthen this rule:

```text
For questions combining location and time, use both relationship paths:
ResaleTransaction -> located_at -> Location
ResaleTransaction -> sold_in -> SaleMonth

Apply filters first, then aggregate.
For aggregated queries, do not return transaction_id or any row-level identifier unless grouped.
```

---

## Retest

Ask:

```text
average resale price in BEDOK in January
```

Expected result:

```text
Single average resale price value filtered by town and month.
```

---

# Part E — Debug a Grouped Summary Query

## Test Question

Ask:

```text
number of transactions by town
```

---

## Expected Behaviour

The agent should:

```text
Group by Location.town
Return COUNT(ResaleTransaction)
```

---

## Possible Failure

The agent may group by:

```text
location_key
region
street_name
```

instead of:

```text
town
```

---

## Diagnosis

Complete the table:

| Component | Expected | Actual | Correct? |
|---|---|---|---|
| Entity | ResaleTransaction |  |  |
| Grouping | Location.town |  |  |
| Relationship path | ResaleTransaction → located_at → Location |  |  |
| Operation | COUNT(ResaleTransaction) |  |  |
| Return fields | town, transaction_count |  |  |

---

## Fix

Add or strengthen grouping rules:

```text
by town -> group by Location.town
by region -> group by Location.region
by month -> group by SaleMonth.month_name
by year -> group by SaleMonth.sale_year
```

---

## Retest

Ask:

```text
number of transactions by town
```

Expected result:

```text
Grouped count of transactions by town.
```

---

# Part F — Document the Issue

Use this template to document one error you encountered.

```markdown
## Issue

Describe the failed question.

## Failed Question

```text
average resale price in BEDOK in January
```

## Error Message

Paste the error message or describe the failure.

## Expected Behaviour

Describe what should have happened.

## Diagnosis

| Component | Expected | Actual | Correct? |
|---|---|---|---|
| Entity |  |  |  |
| Filter |  |  |  |
| Relationship path |  |  |  |
| Operation |  |  |  |
| Return fields |  |  |  |

## Fix

Describe what you changed.

## Retest Result

Describe whether the query now works.
```

---

# Reflection

Think about:

- Which failures were caused by ontology structure?
- Which failures were caused by instructions?
- Which failures were caused by value formatting?
- Which failures were caused by return fields?
- Which fix was most effective?

---

# Key Learning

Most agent errors are clues.

They point to problems in:

```text
Entity → Filter → Relationship path → Operation → Return fields
```

Debugging is not separate from design.

Debugging is how you improve the ontology and the agent.

---

# Next Step

Continue to:

[`/labs/lab-04-agent-evaluation.md`](./lab-04-agent-evaluation.md)
