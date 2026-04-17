# 🧠 Learning Guide — Ontology & Data Agent

This guide explains **how to think**, not just what to click.

---

# 🎯 The Big Idea
Data → Ontology → Data Agent → Answers

- Data = raw tables  
- Ontology = meaning layer  
- Agent = interface  

---

# 🧱 From Data Model to Ontology

| Data Model | Ontology |
|----------|---------|
| Fact table | Core entity |
| Dimension | Supporting entity |
| Join | Relationship |

---

## Example

| Table | Entity |
|------|--------|
| fact_resale_transaction | ResaleTransaction |
| dim_location | Location |
| dim_date | SaleMonth |

---

# 🔗 Relationships = Meaning
ResaleTransaction → located_at → Location

ResaleTransaction → sold_in → SaleMonth


👉 Relationships must read like English

---

# 🧠 How the Agent Thinks
Question
↓
Map entities
↓
Apply filters
↓
Aggregate
↓
Answer

---

## Example

**Question:**
> average resale price in Tampines in January

**Mapping:**
- Tampines → Location.town  
- January → SaleMonth  
- average → AVG(resale_price)

---

# ⚠️ Why Queries Fail

## 1. Mixing Detail + Summary

Error:
transaction_id not in GROUP BY


Cause:
- mixing row-level + aggregated data

---

## Rule

- Summary → aggregates only  
- Detail → row-level only  

---

## 2. Case Sensitivity
Tampines ≠ TAMPINES


---

## 3. Missing Mapping

Example:
- "January" not mapped

---

# 🧩 Role of Instructions

Instructions tell the agent:

- how to map  
- how to aggregate  
- what NOT to do  

---

## Example
price → resale_price
Tampines → TAMPINES
avg → AVG(resale_price)
Do not include transaction_id in aggregated queries


---

# ⚠️ Limitations

- case sensitivity  
- multi-filter instability  
- aggregation quirks  

👉 This is normal (preview feature)

---

# 🎯 Key Takeaways

- Ontology design matters  
- Relationships define logic  
- Instructions guide behaviour  
- Failures are part of learning  

---

👉 Next: go to the Lab Guide
