# Troubleshooting Guide

This guide helps you diagnose and fix common issues when building ontology-powered data agents.

The goal is not just to fix errors.

The goal is to understand **why** the error happened.

---

## Core Idea

Most data-agent failures are not random.

They usually come from one of five areas:

```text
Entity → Filter → Relationship path → Operation → Return fields
```

When something fails, trace the issue through this sequence.

---

## Troubleshooting Mindset

When a query fails, do not immediately assume the tool is broken.

Ask:

1. Did the agent choose the right entity?
2. Did the agent map the filter correctly?
3. Did the agent follow the right relationship?
4. Did the agent choose the right operation?
5. Did the agent return the right fields?

This is the same framework used in NL2GQL debugging.

---

## Quick Diagnostic Table

| Symptom | Likely Cause | What to Check |
|---|---|---|
| No result returned | Case mismatch or wrong filter | Check value format, e.g. BEDOK vs Bedok |
| Query fails with GROUP BY error | Mixing row-level and aggregated fields | Check return fields |
| Month filter fails | Missing or unclear time relationship | Check `sold_in` relationship |
| Town filter fails | Missing or incorrect location relationship | Check `located_at` relationship |
| Average price fails | `resale_price` not numeric or not bound | Check semantic model and property binding |
| Wrong grouping | Agent chose wrong grouping property | Check instructions and ontology properties |
| Query works alone but fails with two filters | Multi-entity traversal issue | Test filters separately |
| Answer is too technical | Weak response rules | Improve data-agent instructions |
| Agent gives unsupported answer | Boundary not defined | Add “Use only HousingResaleOntology” |

---

## Issue 1 — GROUP BY Error

### Example error

```text
The identifier node_ResaleTransaction.transaction_id cannot be used,
as it is neither part of the GROUP BY nor an aggregation.
```

---

### What this means

The generated query is mixing:

- row-level fields
- aggregated values

Example of a bad return shape:

```text
transaction_id + AVG(resale_price)
```

This is invalid for a summary query.

---

### Why it happens

The user asks for a summary, such as:

```text
average resale price in BEDOK
```

But the generated query tries to return:

```text
transaction_id
location_key
AVG(resale_price)
```

The agent is mixing detail-level and summary-level logic.

---

### How to fix it

For summary questions, return only:

- grouped fields
- aggregated measures

Do not return:

- transaction_id
- location_key
- flat_key
- lease_key

unless those fields are part of the grouping.

---

### Instruction fix

Add this to the agent instructions:

```text
For aggregated queries, do not return transaction_id or any row-level identifier unless grouped.
Summary queries should return only grouped fields and aggregated measures.
Detail queries may return row-level identifiers and measures without aggregation.
```

---

### Test again

Try:

```text
average resale price in BEDOK
```

Expected result:

```text
Single average resale price value
```

---

## Issue 2 — Town Query Returns No Result

### Example question

```text
average resale price in Tampines
```

---

### Possible cause

The data may store town names in uppercase:

```text
TAMPINES
BEDOK
JURONG EAST
```

But the user may type:

```text
Tampines
Bedok
Jurong East
```

---

### Why it happens

Text filters can be case-sensitive depending on how the query is generated.

If the generated query searches for:

```text
Tampines
```

but the data contains:

```text
TAMPINES
```

the query may return no result.

---

### How to fix it

Add a normalization rule.

```text
Town values are stored in uppercase.
Convert user-provided town names to uppercase before querying.
Example: Tampines -> TAMPINES
Example: Bedok -> BEDOK
```

---

### Test again

Try:

```text
average resale price in TAMPINES
```

Then try:

```text
average resale price in Tampines
```

Expected result:

```text
Both should resolve to the same town filter.
```

---

## Issue 3 — Month Query Fails

### Example question

```text
average resale price in January
```

---

### Possible cause

The agent may not know how to connect transactions to months.

It needs this relationship:

```text
ResaleTransaction → sold_in → SaleMonth
```

---

### What to check

Check that:

- `SaleMonth` exists as an entity
- `month_name` is available as a property
- `date_key` is defined as the key
- `ResaleTransaction → sold_in → SaleMonth` exists
- the relationship binding uses the correct date key

---

### Instruction fix

Add this mapping:

```text
month -> SaleMonth.month_name
year -> SaleMonth.sale_year
```

If relevant, add:

```text
January, February, March and other month names refer to SaleMonth.month_name.
```

---

### Test again

Try:

```text
average resale price in January
```

Expected result:

```text
Average resale price filtered by SaleMonth.month_name = January
```

---

## Issue 4 — Multi-Filter Query Fails

### Example question

```text
average resale price in BEDOK in January
```

---

### Why this is harder

This question requires:

- a location filter
- a time filter
- an aggregation
- two relationship paths

The agent must use:

```text
ResaleTransaction → located_at → Location
ResaleTransaction → sold_in → SaleMonth
```

---

### What to check

Check each part separately.

First test location:

```text
average resale price in BEDOK
```

Then test month:

```text
average resale price in January
```

Then test both:

```text
average resale price in BEDOK in January
```

---

### If the combined query fails

The issue is likely one of these:

- relationship path is unclear
- generated query returns row-level fields
- month mapping is missing
- town normalization is missing
- aggregation rule is incomplete

---

### Instruction fix

Add this:

```text
For questions combining location and time, use both relationship paths:
ResaleTransaction -> located_at -> Location
ResaleTransaction -> sold_in -> SaleMonth

Apply filters first, then aggregate.
```

Also keep:

```text
For aggregated queries, do not return transaction_id or any row-level identifier unless grouped.
```

---

## Issue 5 — Average Price Does Not Work

### Example question

```text
average resale price in BEDOK
```

---

### Possible causes

- `resale_price` is not numeric
- `resale_price` is not bound correctly
- the agent does not know that “price” means `resale_price`
- aggregation rules are missing

---

### What to check

In the semantic model:

- does `resale_price` show the ∑ symbol?
- is it treated as a numeric field?

In ontology:

- is `resale_price` bound to the transaction table?
- does `ResaleTransaction` include `resale_price` as a property?

In instructions:

```text
resale price, price, cost -> resale_price
average price -> AVG(resale_price)
```

---

### Test again

Try:

```text
average resale price in BEDOK
```

Expected result:

```text
Average of resale_price filtered by town.
```

---

## Issue 6 — Wrong Grouping

### Example question

```text
number of transactions by town
```

---

### Expected interpretation

| Part | Expected mapping |
|---|---|
| Entity | ResaleTransaction |
| Grouping | Location.town |
| Operation | COUNT(ResaleTransaction) |

---

### Possible problem

The agent may group by the wrong field, such as:

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

### Instruction fix

Add grouping rules:

```text
by town -> group by Location.town
by region -> group by Location.region
by month -> group by SaleMonth.month_name
by year -> group by SaleMonth.sale_year
```

---

### Test again

Try:

```text
number of transactions by town
```

Expected result:

```text
Grouped count by town.
```

---

## Issue 7 — Agent Returns Technical Output

### Example problem

The answer includes:

```text
transaction_id
location_key
date_key
node_ResaleTransaction
node_Location
```

---

### Why it happens

The agent is returning technical column or node names instead of business-friendly language.

---

### Instruction fix

Add response rules:

```text
Prefer business wording over technical column names.
Keep answers short, clear, and business-friendly.
Mention SGD for prices.
Mention sqm for area.
```

---

### Better answer style

Instead of:

```text
AVG(node_ResaleTransaction.resale_price) = 512000
```

Prefer:

```text
The average resale price is SGD 512,000.
```

---

## Issue 8 — Agent Answers Beyond the Data

### Example problem

The agent gives explanations about the housing market that are not supported by the ontology.

---

### Why it happens

The agent may rely on general knowledge instead of the selected ontology.

---

### Instruction fix

Add a strict data boundary:

```text
Use only data from HousingResaleOntology.
Do not use outside knowledge.
If the answer cannot be found from the ontology, say that the data does not support the answer.
```

---

## Issue 9 — Relationship Looks Correct but Query Still Fails

### Why this happens

Creating a relationship is not enough.

You also need:

- keys
- relationship binding
- property binding

---

### What to check

For `ResaleTransaction → located_at → Location`, check:

- `ResaleTransaction.location_key` exists
- `Location.location_key` exists
- `Location.location_key` is the key
- relationship binding maps `location_key = location_key`
- `Location.town` is property-bound to the correct column

---

### Key reminder

```text
Relationship = meaning
Binding = data connection
```

Both are required.

---

## Issue 10 — Query Works Before, Then Breaks After Ontology Changes

### Why it happens

Small ontology changes can affect data-agent behaviour.

Examples:

- renaming an entity
- changing a relationship name
- changing a key
- editing bindings
- changing agent instructions

---

### What to do

Use version control.

Record:

- what changed
- why it changed
- what query was tested
- whether it improved or worsened the result

---

## Troubleshooting Workflow

Use this workflow whenever something breaks.

```text
1. Re-run the simplest working query
2. Check the entity
3. Check the filter
4. Check the relationship path
5. Check the operation
6. Check return fields
7. Check keys and binding
8. Check instructions
9. Retest
10. Document the fix
```

---

## Troubleshooting Template

Use this template when documenting an issue.

```markdown
## Issue

Describe the failed question.

## Failed question

```text
average resale price in BEDOK in January
```

## Error message

Paste the error message.

## Expected behaviour

Describe what should have happened.

## Diagnosis

- Entity:
- Filter:
- Relationship path:
- Operation:
- Return fields:

## Fix

Describe what you changed.

## Retest result

Describe whether the query now works.

---

## Common Fixes Summary

| Problem | Common Fix |
|---|---|
| Town not found | Add uppercase normalization |
| Month not found | Map month names to SaleMonth.month_name |
| GROUP BY error | Remove row-level identifiers from aggregated queries |
| Average price fails | Check numeric type and property binding |
| Wrong grouping | Add grouping rules |
| Technical response | Add response style rules |
| Unsupported answer | Add data boundary rule |
| Relationship failure | Check keys and relationship binding |

---

## Key Takeaways

- Most failures are diagnostic clues.
- Do not treat errors as random.
- Check entity, filter, relationship path, operation, and return fields.
- Keys and binding matter as much as relationships.
- Instructions can prevent repeated query failures.
- Debugging is part of designing better data agents.

---

## Next Step

Continue to:

[`/docs/evaluation-framework.md`](./evaluation-framework.md)
