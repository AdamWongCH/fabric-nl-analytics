# 🧠 Learning Guide — Ontology & Data Agent

This guide focuses on **how to think with data**, not just how to use the interface.

---

# 🎯 The Big Idea

Data → Ontology → Data Agent → Answers

- **Data** = raw tables  
- **Ontology** = meaning layer  
- **Data Agent** = interface  

> The agent is only as smart as the ontology.

---

# 🧱 From Data Model to Ontology

You are not starting from scratch — you are translating a data model into meaning.

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

👉 Relationships should read like natural language

---

# 🧠 How the Data Agent Works

The agent does NOT “understand” like a human.

It follows a structured process:

Question → Map entities → Apply filters → Aggregate → Answer

---

# 🧠 Thinking Like the Agent

When you ask a question, don’t treat it as free-form language.

Break it into **three parts**:

---

## 1. What is the subject?

Which entity are we querying?

Example:
- ResaleTransaction

---

## 2. What are the filters?

What conditions apply?

Example:
- Tampines → Location.town  
- January → SaleMonth  

---

## 3. What is the operation?

Are we summarising or listing data?

Example:
- average → AVG(resale_price)

---

## Example Breakdown

**Question:**
> average resale price in Tampines in January

**Thinking:**
- Entity → ResaleTransaction  
- Filters → town = TAMPINES, month = January  
- Operation → AVG(resale_price)

---

👉 If a query fails, revisit these three parts.

---

# ⚠️ Why Queries Fail

Most failures are predictable.

---

## 1. Mixing Detail + Summary

**Error:**
transaction_id not in GROUP BY  

**Cause:**
- mixing row-level data with aggregated results  

---

## Rule

- Summary → aggregates only  
- Detail → row-level only  

---

## 2. Case Sensitivity

Tampines ≠ TAMPINES  

👉 Fix:
- normalize inputs

---

## 3. Missing Mapping

Example:
- "January" not mapped to SaleMonth  

👉 The agent cannot interpret it

---

# 🧩 Role of Instructions

Instructions act as the **control layer**.

They tell the agent:
- how to map business terms to data  
- how to handle aggregation  
- what NOT to include  

---

## Example

price → resale_price  
Tampines → TAMPINES  
avg → AVG(resale_price)  
Do not include transaction_id in aggregated queries  

---

# ⚠️ Current Limitations

Fabric IQ is still in preview.

You may encounter:
- case sensitivity issues  
- multi-filter instability  
- aggregation quirks  

👉 This is expected

---

# 🎯 Key Takeaways

- Ontology design shapes how the agent thinks  
- Relationships define meaning  
- Instructions guide behaviour  
- Queries fail for logical reasons, not randomly  
- Understanding the logic helps you debug effectively  

---

# 💬 Reflection

Before moving to the lab, consider:

- How would you break a question into entity, filters, and operation?  
- Why might a query fail?  
- What role do instructions play in guiding the agent?  

---

👉 Next: proceed to the **Lab Guide**
