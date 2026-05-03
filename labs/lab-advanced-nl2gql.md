# Advanced Lab — NL2GQL Reasoning

This lab helps you understand how natural language questions are translated into graph query logic.

You are not writing perfect GQL from memory.

You are learning to reason through the translation.

---

## Goal

By the end of this lab, you should be able to break a natural language question into:

- intent
- entity
- filters
- relationship path
- operation
- expected result type

This helps you debug data-agent behaviour more systematically.

---

## Core Idea

NL2GQL means:

```text
Natural Language → Graph Query Language
```

A data agent must translate a user question into structured query logic.

The better the ontology and instructions, the better this translation becomes.

---

## Translation Framework

Use this framework for every question:

| Component | Question to ask |
|---|---|
| Intent | What is the user asking for? |
| Entity | What is the main object being queried? |
| Filters | What conditions apply? |
| Relationship path | How do the entities connect? |
| Operation | What calculation or result is needed? |
| Expected result type | Single value, grouped summary, detail records, or comparison? |

---

## Dataset Context

For this lab, assume the ontology includes:

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

## Example Walkthrough

### Question

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
| Expected result type | Single aggregated value |

---

### Expected GQL Shape

```text
MATCH transaction connected to location
FILTER town = BEDOK
RETURN average resale price
```

You do not need exact syntax at this stage.

Focus on the logic.

---

## Task 1 — Simple Location Filter

### Question

```text
What is the average resale price in Tampines?
```

---

### Your Breakdown

| Component | Your answer |
|---|---|
| Intent |  |
| Entity |  |
| Filter |  |
| Relationship path |  |
| Operation |  |
| Expected result type |  |

---

### Expected Answer

| Component | Expected mapping |
|---|---|
| Intent | Find average price |
| Entity | ResaleTransaction |
| Filter | Location.town = TAMPINES |
| Relationship path | ResaleTransaction → located_at → Location |
| Operation | AVG(resale_price) |
| Expected result type | Single aggregated value |

---

## Task 2 — Grouped Summary

### Question

```text
What is the number of transactions by town?
```

---

### Your Breakdown

| Component | Your answer |
|---|---|
| Intent |  |
| Entity |  |
| Filter |  |
| Grouping |  |
| Relationship path |  |
| Operation |  |
| Expected result type |  |

---

### Expected Answer

| Component | Expected mapping |
|---|---|
| Intent | Count transactions |
| Entity | ResaleTransaction |
| Filter | None |
| Grouping | Location.town |
| Relationship path | ResaleTransaction → located_at → Location |
| Operation | COUNT(ResaleTransaction) |
| Expected result type | Grouped summary |

---

## Task 3 — Time Filter

### Question

```text
What is the average resale price in January?
```

---

### Your Breakdown

| Component | Your answer |
|---|---|
| Intent |  |
| Entity |  |
| Filter |  |
| Relationship path |  |
| Operation |  |
| Expected result type |  |

---

### Expected Answer

| Component | Expected mapping |
|---|---|
| Intent | Find average price |
| Entity | ResaleTransaction |
| Filter | SaleMonth.month_name = January |
| Relationship path | ResaleTransaction → sold_in → SaleMonth |
| Operation | AVG(resale_price) |
| Expected result type | Single aggregated value |

---

## Task 4 — Multi-Entity Filter

### Question

```text
What is the average resale price in Bedok in January?
```

---

### Your Breakdown

| Component | Your answer |
|---|---|
| Intent |  |
| Entity |  |
| Filters |  |
| Relationship paths |  |
| Operation |  |
| Expected result type |  |

---

### Expected Answer

| Component | Expected mapping |
|---|---|
| Intent | Find average price |
| Entity | ResaleTransaction |
| Filters | Location.town = BEDOK; SaleMonth.month_name = January |
| Relationship paths | ResaleTransaction → located_at → Location; ResaleTransaction → sold_in → SaleMonth |
| Operation | AVG(resale_price) |
| Expected result type | Single aggregated value |

---

## Task 5 — Comparison

### Question

```text
Which town is cheaper, Bedok or Tampines?
```

---

### Your Breakdown

| Component | Your answer |
|---|---|
| Intent |  |
| Entity |  |
| Filters |  |
| Grouping |  |
| Relationship path |  |
| Operation |  |
| Expected result type |  |

---

### Expected Answer

| Component | Expected mapping |
|---|---|
| Intent | Compare average prices |
| Entity | ResaleTransaction |
| Filters | Location.town IN (BEDOK, TAMPINES) |
| Grouping | Location.town |
| Relationship path | ResaleTransaction → located_at → Location |
| Operation | AVG(resale_price), compare lower value |
| Expected result type | Comparison between two grouped averages |

---

## Task 6 — Detail Query

### Question

```text
Show transactions in Bedok
```

---

### Your Breakdown

| Component | Your answer |
|---|---|
| Intent |  |
| Entity |  |
| Filter |  |
| Relationship path |  |
| Operation |  |
| Expected result type |  |

---

### Expected Answer

| Component | Expected mapping |
|---|---|
| Intent | Show transaction records |
| Entity | ResaleTransaction |
| Filter | Location.town = BEDOK |
| Relationship path | ResaleTransaction → located_at → Location |
| Operation | None |
| Expected result type | Row-level detail records |

---

## Important Distinction

Summary questions and detail questions should return different outputs.

| Question type | Example | Expected output |
|---|---|---|
| Summary | average resale price in Bedok | aggregated value |
| Grouped summary | number of transactions by town | grouped table |
| Detail | show transactions in Bedok | row-level records |

This matters because a common error is mixing detail fields with aggregated results.

Example problem:

```text
transaction_id + AVG(resale_price)
```

That can cause a GROUP BY error.

---

## Challenge — Predict the Failure

### Question

```text
What is the average resale price in Bedok in January?
```

Suppose the generated query returns:

```text
transaction_id, location_key, AVG(resale_price)
```

---

### Questions

1. What did the agent get right?
2. What did the agent get wrong?
3. Why would this fail?
4. What instruction would you add?

---

### Suggested Answer

The agent likely got these right:

- entity: ResaleTransaction
- filters: town and month
- operation: AVG(resale_price)

The agent got this wrong:

- return fields

It returned row-level identifiers together with an aggregate.

This may fail because summary queries should not return row-level identifiers unless they are grouped.

Add this instruction:

```text
For aggregated queries, do not return transaction_id or any row-level identifier unless grouped.
```

---

## Reflection

Think about:

- Which questions were easiest to translate?
- Which questions required relationship paths?
- Which questions required grouping?
- Which questions required distinguishing summary from detail?
- What does this tell you about ontology design?

---

## Key Learning

NL2GQL is not magic.

It is structured translation:

```text
Question → Intent → Entity → Filter → Relationship path → Operation → Return fields → Answer
```

Better ontology design and clearer instructions improve this translation.

---

## Next Step

Continue to:

[`/labs/lab-debugging-agent-errors.md`](./lab-debugging-agent-errors.md)
