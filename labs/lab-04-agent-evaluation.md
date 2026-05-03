# Lab 04 — Agent Evaluation

This lab helps you evaluate an ontology-powered data agent.

The goal is to move beyond:

```text
It gave an answer.
```

towards:

```text
It gave the right answer, using the right data, at the right level of detail.
```

---

## Goal

By the end of this lab, you should be able to:

- design a small test set of questions
- define expected behaviour before testing
- score agent responses consistently
- identify whether failures are caused by ontology, binding, instructions, or query generation
- document improvements and retest after changes

---

## Before You Start

You should have completed:

- `/labs/lab-01-build-first-data-agent.md`
- `/labs/lab-02-advanced-nl2gql.md`
- `/labs/lab-03-debugging-agent-errors.md`

You should also be familiar with:

- `/docs/evaluation-framework.md`

---

## Core Evaluation Idea

A data agent should be evaluated like an analytical system.

Do not only ask:

```text
Did it answer?
```

Ask:

```text
Did it understand the intent?
Did it use the correct entity?
Did it apply the correct filters?
Did it follow the correct relationship path?
Did it use the correct aggregation?
Did it return the correct level of detail?
```

---

## Evaluation Framework

Use the same structure from NL2GQL and troubleshooting:

```text
Entity → Filter → Relationship path → Operation → Return fields
```

For each test question, evaluate:

| Component | Evaluation question |
|---|---|
| Entity | Did the agent choose the correct entity? |
| Filter | Did it apply the correct filter? |
| Relationship path | Did it follow the correct relationship? |
| Operation | Did it use the correct calculation or output type? |
| Return fields | Did it return the right level of detail? |
| Answer clarity | Was the final answer clear and business-friendly? |

---

# Part A — Build a Test Set

Create a small benchmark of questions.

Use at least one question from each category:

| Category | Example |
|---|---|
| Simple filter + aggregation | average resale price in BEDOK |
| Grouped summary | number of transactions by town |
| Time grouping | average resale price by month |
| Multi-filter query | average resale price in BEDOK in January |
| Detail query | show transactions in BEDOK |
| Comparison | which town is cheaper, BEDOK or TAMPINES? |
| Ambiguous query | show me price |
| Boundary query | why are prices high? |

---

## Your Test Set

Fill in your own test set below.

| No. | Test question | Category | Why this test matters |
|---|---|---|---|
| 1 |  |  |  |
| 2 |  |  |  |
| 3 |  |  |  |
| 4 |  |  |  |
| 5 |  |  |  |
| 6 |  |  |  |
| 7 |  |  |  |
| 8 |  |  |  |

---

# Part B — Define Expected Behaviour

Before running the agent, define what should happen.

This prevents you from accepting a fluent but incorrect answer.

---

## Expected Behaviour Template

| Question | Expected entity | Expected filter / grouping | Expected operation | Expected result type |
|---|---|---|---|---|
| average resale price in BEDOK | ResaleTransaction | Location.town = BEDOK | AVG(resale_price) | single aggregate |
| number of transactions by town | ResaleTransaction | group by Location.town | COUNT | grouped summary |
| average resale price by month | ResaleTransaction | group by SaleMonth.month_name | AVG(resale_price) | grouped summary |
| average resale price in BEDOK in January | ResaleTransaction | town + month | AVG(resale_price) | single aggregate |
| show transactions in BEDOK | ResaleTransaction | Location.town = BEDOK | none | row-level detail |

---

## Your Expected Behaviour

Complete this table before testing.

| Question | Expected entity | Expected filter / grouping | Expected relationship path | Expected operation | Expected result type |
|---|---|---|---|---|---|
|  |  |  |  |  |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |

---

# Part C — Run the Agent and Score

Run each question in the data agent.

Use a simple 0–2 scoring scale.

| Score | Meaning |
|---|---|
| 2 | Correct |
| 1 | Partially correct |
| 0 | Incorrect or failed |

---

## Scoring Guide

### Score 2 — Correct

Give 2 if:

- correct entity is used
- correct filters are applied
- correct relationship path is followed
- correct operation is used
- output level is appropriate
- answer is clear

---

### Score 1 — Partially Correct

Give 1 if the answer is directionally useful, but one component is wrong or missing.

Examples:

- correct town but wrong aggregation
- correct measure but wrong grouping
- correct query but unclear answer wording
- answer works but uses overly technical wording

---

### Score 0 — Incorrect

Give 0 if:

- query fails
- wrong entity is used
- wrong filter is applied
- unsupported answer is given
- result is misleading

---

## Evaluation Table

| No. | Question | Answered? | Score | What worked? | What failed? | Possible fix |
|---|---|---|---|---|---|---|
| 1 |  | Yes / No | 0 / 1 / 2 |  |  |  |
| 2 |  | Yes / No | 0 / 1 / 2 |  |  |  |
| 3 |  | Yes / No | 0 / 1 / 2 |  |  |  |
| 4 |  | Yes / No | 0 / 1 / 2 |  |  |  |
| 5 |  | Yes / No | 0 / 1 / 2 |  |  |  |
| 6 |  | Yes / No | 0 / 1 / 2 |  |  |  |
| 7 |  | Yes / No | 0 / 1 / 2 |  |  |  |
| 8 |  | Yes / No | 0 / 1 / 2 |  |  |  |

---

# Part D — Component-Level Scoring

For more detailed evaluation, score each component separately.

| Question | Entity | Filter | Relationship path | Operation | Return fields | Answer clarity | Total |
|---|---|---|---|---|---|---|---|
|  | 0 / 1 / 2 | 0 / 1 / 2 | 0 / 1 / 2 | 0 / 1 / 2 | 0 / 1 / 2 | 0 / 1 / 2 |  |
|  | 0 / 1 / 2 | 0 / 1 / 2 | 0 / 1 / 2 | 0 / 1 / 2 | 0 / 1 / 2 | 0 / 1 / 2 |  |
|  | 0 / 1 / 2 | 0 / 1 / 2 | 0 / 1 / 2 | 0 / 1 / 2 | 0 / 1 / 2 | 0 / 1 / 2 |  |

---

## How to Interpret Component Scores

| Low score area | Likely improvement |
|---|---|
| Entity | clarify entity names and meanings |
| Filter | improve mappings and normalization |
| Relationship path | improve ontology relationships |
| Operation | improve aggregation rules |
| Return fields | improve summary/detail instructions |
| Answer clarity | improve response rules |

---

# Part E — Diagnose Failure Source

For each failed or partially correct answer, decide whether the issue is likely caused by:

- ontology design
- key or binding issue
- weak instructions
- generated query issue
- ambiguous user question
- unsupported data

---

## Failure Diagnosis Table

| Question | Symptom | Likely source | Evidence | Fix |
|---|---|---|---|---|
|  |  | Ontology / Binding / Instructions / Query / Ambiguity / Unsupported |  |  |
|  |  | Ontology / Binding / Instructions / Query / Ambiguity / Unsupported |  |  |
|  |  | Ontology / Binding / Instructions / Query / Ambiguity / Unsupported |  |  |

---

# Part F — Improve and Retest

Choose one issue to fix.

Example fixes:

| Problem | Possible fix |
|---|---|
| town not recognised | add normalization rule |
| month not recognised | add SaleMonth mapping |
| GROUP BY error | strengthen summary query rule |
| wrong grouping | add grouping rules |
| technical answer | add business-friendly response rule |
| unsupported answer | add data boundary rule |

---

## Improvement Log

| Change made | Why it was made | Question retested | Result after fix |
|---|---|---|---|
|  |  |  |  |
|  |  |  |  |

---

# Part G — Regression Testing

After fixing one issue, retest earlier working questions.

This checks whether the fix created new problems.

---

## Regression Test Set

Retest:

```text
average resale price in BEDOK
number of transactions by town
average resale price by month
average resale price in BEDOK in January
show transactions in BEDOK
```

---

## Regression Results

| Question | Before fix | After fix | Status |
|---|---|---|---|
| average resale price in BEDOK |  |  | Pass / Fail |
| number of transactions by town |  |  | Pass / Fail |
| average resale price by month |  |  | Pass / Fail |
| average resale price in BEDOK in January |  |  | Pass / Fail |
| show transactions in BEDOK |  |  | Pass / Fail |

---

# Part H — Summarise Agent Quality

Based on your evaluation, classify the agent.

| Level | Description |
|---|---|
| Level 1 — Demo Agent | works for a few simple questions |
| Level 2 — Guided Agent | works for common business questions with known limitations |
| Level 3 — Reliable Agent | works consistently across common and edge-case questions |

---

## Your Assessment

Complete this:

```text
Current agent level:
Reason:
Top 3 issues:
Top 3 improvements:
```

---

# Reflection

Think about:

- Which test questions were easiest?
- Which questions revealed the most problems?
- Did the agent fail because of ontology, instructions, or ambiguity?
- Which fix improved behaviour the most?
- What would you test before sharing the agent with others?

---

# Key Learning

A fluent answer is not always a correct answer.

Evaluation turns a data agent from a demo into a more reliable analytical capability.

---

# Final Note

You are not just asking whether the agent can answer.

You are checking whether the agent can reason correctly over structured data.

That is the difference between a demo and a dependable data agent.
