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
2. Include 3 tables
- fact_resale_transaction  
- dim_location  
- dim_date  

---

## Check

- `resale_price` shows ∑ (numeric)  
- tables are visible  

---

## 📸 Screenshot
👉 Semantic model showing tables

<img width="602" height="308" alt="image" src="https://github.com/user-attachments/assets/4610f1d8-faf6-4612-94bb-ed928e2e2ca7" />

---

# 🟦 Lab 3 — Define Relationships

## Create relationships

- fact_resale_transaction → dim_location (location_key)  
- fact_resale_transaction → dim_date (date_key)   

---

## Important

- Direction: fact → dimension  
- Cardinality: many-to-one  

---

## 📸 Screenshot
👉 Model view with relationships

<img width="844" height="494" alt="image" src="https://github.com/user-attachments/assets/967c5f40-8dad-4e13-8615-24cc4f3ba6ed" />

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

<img width="863" height="434" alt="image" src="https://github.com/user-attachments/assets/64d89439-7dbc-434e-ae74-ec35288b9731" />

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

<img width="995" height="550" alt="image" src="https://github.com/user-attachments/assets/f53439b7-4e66-4571-944f-e01de88f6ae4" />

---

# 🟦 Lab 6 — Create Data Agent

## Steps

1. Create a new Data Agent  
2. Attach the ontology  

---

## 📸 Screenshot
👉 Data agent setup screen

<img width="331" height="151" alt="image" src="https://github.com/user-attachments/assets/86e02b76-3a27-4f9d-8dbc-ee136bf94121" />

<img width="741" height="247" alt="image" src="https://github.com/user-attachments/assets/29b71bd1-9d08-431d-aabe-fa25ef0f17f4" />

---

# 🟦 Lab 7 — Add Instructions

Copy and paste:
You are a data agent that answers questions about Singapore housing resale transactions.

Keep answers short, sharp, and concise.
Use only data from HousingResaleOntology.
Do not use outside knowledge.
Support group by in GQL.

Entities:
- ResaleTransaction = one HDB resale transaction
- SaleMonth = transaction month and year
- Location = region, town, street, block
- Flat = flat type, flat model, storey range, floor area
- Lease = lease attributes

Mappings:
- resale price, price, cost -> resale_price
- price per sqm -> price_per_sqm
- town -> Location.town
- region -> Location.region
- month -> SaleMonth.month_name
- year -> SaleMonth.sale_year

Rules:
- Normalize town names to the stored format when needed.
- For aggregated queries, do not return transaction_id, location_key, or any row-level identifier unless it is grouped.
- For average price questions, return AVG(resale_price).
- For count questions, return COUNT(ResaleTransaction).
- For grouped summaries, return only grouped dimensions and aggregated measures.
- For detail questions, return row-level identifiers and measures without aggregation.
- Prefer business wording over technical column names.
- Mention SGD for prices and sqm for area.
