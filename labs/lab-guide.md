# 🧪 Lab Guide — Build & Debug a Data Agent

This lab is designed for **learning by doing**.

👉 You will build an ontology and data agent  
👉 You will encounter errors  
👉 You will debug them  

---

# 🎯 Goal

By the end of this lab, you will have:

- a semantic model  
- an ontology  
- a data agent  
- working queries  

---

# 🟦 Lab 1 — Load Data into Fabric

## Steps

1. Create a **Lakehouse**
2. Upload the CSV files from `/data/sample/`
3. Convert each CSV into a **table**

---

## Tables to create

- fact_resale_transaction  
- dim_location  
- dim_date  
- dim_flat  
- dim_lease  

---

## 📸 Screenshot
👉 Lakehouse view showing uploaded tables
<img width="598" height="272" alt="image" src="https://github.com/user-attachments/assets/c7506eb8-268d-4d1c-8d4d-89a42caaec5b" />


---

# 🟦 Lab 2 — Create Semantic Model

## Steps

1. Create a **semantic model** from the lakehouse  
2. Include all 5 tables  

---

## Check

- `resale_price` shows ∑ (numeric)  
- tables are visible  

---

## 📸 Screenshot
👉 Semantic model showing tables

---

# 🟦 Lab 3 — Define Relationships

## Create relationships

- fact_resale_transaction → dim_location (location_key)  
- fact_resale_transaction → dim_date (date_key)  
- fact_resale_transaction → dim_flat (flat_key)  
- fact_resale_transaction → dim_lease (lease_key)  

---

## Important

- Direction: fact → dimension  
- Cardinality: many-to-one  

---

## 📸 Screenshot
👉 Model view with relationships

---

# 🟦 Lab 4 — Generate Ontology

## Steps

1. Generate ontology from semantic model  
2. Rename entities:

| Table | Entity |
|------|--------|
| fact_resale_transaction | ResaleTransaction |
| dim_location | Location |
| dim_date | SaleMonth |

---

## 📸 Screenshot
👉 Ontology entity list

---

# 🟦 Lab 5 — Define Relationships in Ontology

Create:

ResaleTransaction → located_at → Location  
ResaleTransaction → sold_in → SaleMonth  

---

## Check

- Names read naturally  
- Mapping is correct  

---

## 📸 Screenshot
👉 Ontology relationship configuration

---

# 🟦 Lab 6 — Create Data Agent

## Steps

1. Create a new Data Agent  
2. Attach the ontology  

---

## 📸 Screenshot
👉 Data agent setup screen

---

# 🟦 Lab 7 — Add Instructions

Copy and paste:
