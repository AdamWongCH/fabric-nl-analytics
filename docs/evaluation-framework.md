# Evaluation Framework

This page explains how to evaluate an ontology-powered data agent.

Building a data agent is not enough.

You also need to test whether it understands questions correctly, generates appropriate queries, and returns useful answers.

---

## Core Idea

A data agent should be evaluated like an analytical system.

Do not only ask:

```text
Did it answer?
```

Ask:

```text
Did it answer correctly?
Did it use the right data?
Did it apply the right filters?
Did it aggregate correctly?
Did it return the answer at the right level of detail?
```

---

## Why Evaluation Matters

Natural language interfaces can feel impressive even when they are wrong.

A fluent answer is not the same as a correct answer.

Evaluation helps you detect:

- wrong entity mapping
- wrong filter mapping
- missing relationship paths
- incorrect aggregation
- invalid return fields
- unsupported explanations
- inconsistent behaviour across similar questions

---

## Evaluation Mindset

Evaluate the data agent using the same NL2GQL framework:

```text
Entity → Filter → Relationship path → Operation → Return fields
```

For each test question, ask:

| Component | Evaluation question |
|---|---|
| Entity | Did the agent use the correct entity? |
| Filter | Did it apply the correct filters? |
| Relationship path | Did it traverse the right relationships? |
| Operation | Did it use the correct calculation? |
| Return fields | Did it return the correct level of detail? |

---

## Evaluation Dimensions

Use these dimensions to assess agent behaviour.

| Dimension | What it tests |
|---|---|
| Intent understanding | Did the agent understand what the user wanted? |
| Entity mapping | Did it select the right entity? |
| Filter mapping | Did it map user terms to the correct properties? |
| Relationship traversal | Did it follow the correct relationship path? |
| Aggregation correctness | Did it use AVG, COUNT, SUM, MIN, or MAX correctly? |
| Grouping correctness | Did it group by the right dimension? |
| Return field correctness | Did it avoid unnecessary row-level identifiers? |
| Answer clarity | Was the response understandable and business-friendly? |
| Data grounding | Did it answer only from the ontology? |
| Failure handling | Did it fail clearly when it could not answer? |

---

## Basic Test Set

Start with a small set of test questions.

These should cover:

- simple filters
- grouped summaries
- time filters
- multi-filter questions
- detail queries
- ambiguous questions

---

## Example Test Questions

| No. | Test question | What it tests |
|---|---|---|
| 1 | average resale price in BEDOK | simple filter + aggregation |
| 2 | number of transactions by town | grouping + count |
| 3 | average resale price by month | time grouping |
| 4 | average resale price in BEDOK in January | multi-filter + aggregation |
| 5 | show transactions in BEDOK | detail-level output |
| 6 | which town is cheaper, BEDOK or TAMPINES? | comparison |
| 7 | average price per sqm in BEDOK | alternate measure |
| 8 | average resale price in bedok | normalization / case handling |
| 9 | give me insights | ambiguity handling |
| 10 | why are prices high? | unsupported explanation / boundary |

---

## Expected Behaviour Table

Create an expected behaviour table before testing.

| Question | Expected entity | Expected filter | Expected operation | Expected result type |
|---|---|---|---|---|
| average resale price in BEDOK | ResaleTransaction | Location.town = BEDOK | AVG(resale_price) | single aggregate |
| number of transactions by town | ResaleTransaction | group by Location.town | COUNT | grouped summary |
| average resale price by month | ResaleTransaction | group by SaleMonth.month_name | AVG(resale_price) | grouped summary |
| average resale price in BEDOK in January | ResaleTransaction | town + month | AVG(resale_price) | single aggregate |
| show transactions in BEDOK | ResaleTransaction | Location.town = BEDOK | none | row-level records |

---

## Pass / Fail Scoring

A simple evaluation score is enough for this repo.

Use:

| Score | Meaning |
|---|---|
| 2 | correct |
| 1 | partially correct |
| 0 | incorrect or failed |

---

## Scoring Guide

### Score 2 — Correct

Give 2 if:

- correct entity is used
- correct filters are applied
- correct operation is used
- output level is appropriate
- answer is clear

---

### Score 1 — Partially Correct

Give 1 if:

- the answer is directionally useful
- but one component is wrong or missing

Examples:

- correct town but wrong aggregation
- correct measure but wrong grouping
- correct query but unclear answer wording

---

### Score 0 — Incorrect

Give 0 if:

- query fails
- wrong entity is used
- wrong filter is applied
- unsupported answer is given
- result is misleading

---

## Evaluation Template

Use this template to evaluate each question.

```markdown
## Test Case

### Question

```text
average resale price in BEDOK
```

### Expected behaviour

- Entity:
- Filter:
- Relationship path:
- Operation:
- Expected result type:

### Actual behaviour

- Did it answer?
- Did it generate the right query?
- Was the answer clear?

### Score

0 / 1 / 2

### Notes

What went wrong or what worked well?

### Fix

What should be improved?
```

---

## Component-Level Evaluation

For more detailed evaluation, score each component separately.

| Component | Score | Notes |
|---|---|---|
| Entity mapping | 0 / 1 / 2 |  |
| Filter mapping | 0 / 1 / 2 |  |
| Relationship path | 0 / 1 / 2 |  |
| Operation | 0 / 1 / 2 |  |
| Return fields | 0 / 1 / 2 |  |
| Answer clarity | 0 / 1 / 2 |  |

This helps identify where the agent is failing.

For example:

- low filter score → improve mappings or normalization
- low relationship score → improve ontology relationships
- low operation score → improve aggregation instructions
- low clarity score → improve response rules

---

## Example Evaluation

### Question

```text
average resale price in BEDOK
```

---

### Expected

| Component | Expected |
|---|---|
| Entity | ResaleTransaction |
| Filter | Location.town = BEDOK |
| Relationship path | ResaleTransaction → located_at → Location |
| Operation | AVG(resale_price) |
| Return fields | avg_resale_price only |

---

### Actual

The agent returned:

```text
transaction_id, location_key, AVG(resale_price)
```

---

### Evaluation

| Component | Score | Notes |
|---|---|---|
| Entity mapping | 2 | Correct entity |
| Filter mapping | 2 | Correct town filter |
| Relationship path | 2 | Correct location relationship |
| Operation | 2 | Correct AVG |
| Return fields | 0 | Included row-level fields |
| Answer clarity | 0 | Query failed |

---

### Diagnosis

The agent understood the question, but returned invalid fields for an aggregated query.

---

### Fix

Add or strengthen this instruction:

```text
For aggregated queries, do not return transaction_id or any row-level identifier unless grouped.
```

---

## Regression Testing

After fixing an issue, do not test only the failed question.

Retest a small set of earlier questions to make sure the fix did not break something else.

---

## Regression Test Set

After every major ontology or instruction change, retest:

```text
average resale price in BEDOK
number of transactions by town
average resale price by month
average resale price in BEDOK in January
show transactions in BEDOK
```

---

## Why Regression Testing Matters

A change can fix one query but break another.

Examples:

- adding a grouping rule may improve summary queries but affect detail queries
- changing a relationship name may improve readability but break generated paths
- adding too many instructions may make the agent inconsistent

Regression testing helps maintain reliability.

---

## Evaluation Log

Keep a simple evaluation log.

| Date | Change made | Question tested | Result | Notes |
|---|---|---|---|---|
| YYYY-MM-DD | Added aggregation rule | average resale price in BEDOK | Pass | Returned average only |
| YYYY-MM-DD | Added normalization rule | average resale price in Tampines | Pass | Interpreted as TAMPINES |
| YYYY-MM-DD | Added month mapping | average resale price in January | Partial | Query worked but sorting unclear |

---

## When to Improve Ontology vs Instructions

Not all problems should be fixed with instructions.

Use this guide:

| Symptom | Likely fix |
|---|---|
| Wrong relationship path | improve ontology relationship |
| Field not found | check binding |
| Wrong business term mapping | improve instructions |
| Case mismatch | improve instructions or data preparation |
| Wrong aggregation | improve instructions |
| Missing entity | improve ontology |
| Too many ambiguous paths | simplify ontology |
| Query returns unsupported answer | improve data boundary instruction |

---

## Quality Levels

You can describe agent maturity using three levels.

---

### Level 1 — Demo Agent

Works for a few simple questions.

Characteristics:

- limited testing
- unclear failure handling
- depends on carefully phrased questions

---

### Level 2 — Guided Agent

Works for common business questions.

Characteristics:

- clear ontology
- useful instructions
- known limitations
- tested query set

---

### Level 3 — Reliable Agent

Works consistently across common and edge-case questions.

Characteristics:

- evaluation framework
- regression testing
- documented fixes
- clear ownership
- governed updates

---

## Final Evaluation Checklist

Before sharing the agent with others, check:

- [ ] common questions tested
- [ ] expected behaviour documented
- [ ] known failures documented
- [ ] instructions refined
- [ ] ontology relationships verified
- [ ] keys verified
- [ ] bindings verified
- [ ] regression tests completed
- [ ] limitations communicated

---

## Key Takeaways

- A fluent answer is not always a correct answer.
- Evaluate the agent using entity, filter, relationship path, operation, and return fields.
- Keep a test set of common questions.
- Score results consistently.
- Retest after every ontology or instruction change.
- Evaluation turns a demo into a reliable analytical capability.

---

## Next Step

Continue to the advanced labs:

[`/labs/lab-advanced-nl2gql.md`](../labs/lab-advanced-nl2gql.md)
